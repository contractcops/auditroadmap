# Cryptography

#### Keys and addresses

> Ownership of ether by EOAs is established with digital private keys, an Ethereum address and digital signatures.

* The private key is one of the most important concepts of cryptography in ethereum. The private key can also be used to derive an Ethereum address -> an account.

> Private keys are not directly used in the Ethereum system in any way. They are never transmitted or stored. They shouldn't appear in messages nor should they be passed around in the network.

> Access and control of funds is done with digital signatures. The digital signatures are created using the private key.

#### Public Key Cryptography and Cryptocurrency

> Public key cryptography relies on special mathematical functions that have a specific property: they are easy to calculate, but hard to reverse-engineer

![1678313125545](<../Mastering Ethereum Book/image/Chapter4-Cryptography/1678313125545.png>)

> One of the advanced category of mathematical functions that Ethereum uses is based on artithmetic operations on an elliptic curve

* The elliptic curve used by Ethereum is called secp256k1 curve
* The equation for the curve is y^2 = x^3 +7

![1678313579099](<../Mastering Ethereum Book/image/Chapter4-Cryptography/1678313579099.png>)

In elliptic curves, adding two points results in a third new point (that intersects the curve). After getting that third point, it is then reflected across the x axis.

> In order for it to be even more secure, a starting point P is added to itself to receive a new point, which is then reflected:

![1678313834824](<../Mastering Ethereum Book/image/Chapter4-Cryptography/1678313834824.png>)

> Doing the above step 10 times can be calculated in four addition operations.
>
> P+P = 2•P
>
> 2•P+2•P = 4•P
>
> 4•P+4•P = 8•P
>
> 2•P+8•P=10•P

How many steps would it take to compute x•P, where x is a random 256-bit integer? In this case, x can range anywhere from 0 to 1.1579209e+77

> Computing P would never require more than 510 point addition operations

> There is no known algorithm or computer that could calculate this. Even if the calculations are started in the middle of the operations, on average it would still take about 2^128 point addition operations.

The problem with the above shown elliptic curve is that some of the coordinates might end up being too large to be stored in a standard 512-bit public key.

> y² = x³+ax+b

is transformed to

> y² mod p = (x³ + ax + b) mod p.

![1678314409601](<../Mastering Ethereum Book/image/Chapter4-Cryptography/1678314409601.png>)

> X=x•P, where x is a random 256-bit integer, how can you prove to someone that you know the x that corresponds to X without revealing any useful information about x?

We can use the point addition property for the modified equation:

> hash(m, r•P)•n•P+r•P = (hash(m, r•P)\*n+r)•P

After simplifying to hash(m, R)•X+R = s•P, we are left with the fact that if you can provide an m, R and s that satisfy the above equation, then this proves that you know the x corresponding to the X in x.P = X equation.

**Digital signatures**

A specific message can be made so that it is required for the verification to be succesful. We can use the m, R and s to form a digital signature for that message. Usually, the message is the unsigned part of a transaction. Generally, the digital signature for a transaction is the x-coordinate of R concatenated with s.

#### Back to the bookFor example, the multiplication of two prime numbers sounds very trivial. However, in the case where you get a number such as 8,018,009, finding the two prime numbers that lead to that number is no longer that simple.❗ However, this is also a trapdoor function. Given the fact that one of the prime numbers is 2003, we can easily find the other by dividing 8018009 / 2003 = 4003In Ethereum (as mentioned in previous slides in this repository), assymetric cryptography is used. A pair of private : public key is used. Furthermore, the public key represents an address (or the accound handle). The private key contains the access to any ether in the account and to authentication over any smart contracts owned by the account.❗ The private key controls the access by being a unique piece when needed to create digital signatures. Those digital signatures are used to "sign" transactions and to spend any funds from the account. The digital signatures also act as a proof of authentication when it comes down to smart contracts or transaction owners.

#### Digital signaturesDigital signatures can be used to sign any messages. For Ethereum transactions, the details of the transaction are the message itself.❗ A transaction is basically a request to access a particular account on the Ethereum network in order to move funds or to interact with a contract.❗ When a transaction is made (sent) on the Ethereum blockchain, it needs to be sent together with a digital signature. With the help of assymetric cryptography (or in this case elliptic curves), anyone can verify that the details of the transaction are valid (authentic) with only the public key. The private key remains secret and does not get exposed to anyone.❗ Important note ❗ - In Ethereum there is no encryption at all. All of the messages/transactions on the Ethereum blockchain can be read/viewed by everyone. Private keys are used to create a digital signature which is used only to verify the validity and authenticity of the owner/contents of a transaction/message.

**Private keysPrivate keys are basically very long unsigned numbers that are picked at random. The private keys must be kept secret at all costs, because their exposal means giving access to all of the ether on that particular account, as well as access to all of the smart contracts.**

**How are private keys in Ethereum generated?The Ethereum key is basically just a randomly generated number. It is basically between 1 and 2 ^256. Ethereum's software uses the underlying OS to generate 256 random bits.❗ This is usually achieved by getting a long set of characters and feeding it to a hashing function, usually keccak256 or sha256, both of which produce a 256 bit output. We then check if the number is within the suitable range and if not - repeat the process once again.❗ The process of generating a random number is offline. The random number generator is not done by using a pseudo-random function such as rand in most languages.**

**Public keysA public key in ethereum is a point on a so called elliptic curve. The one used by ethereum is a standard followed by the USITS.The public key is generated by using the following equationK = k \* g, where k is the private key and G is some constant point that is called the generator point.This results in some point that is impossible to trace back to.The generator point is specified as part of the secp256k1 standard (which is the above mentioned elliptic curve that is used by Ethereum)❗ Thisga generator point is the same one for all of the users in Ethereum, meaning that a private key multiplied by the generator point will always result in the same public key K.❗ The relationship of k (private key) to K one directional and can only be calculated one way. Thats why the public keys can be freely shared between users without the worry of exposing a private key.**

#### Cryptographic hash functions❗ Hash functions are a one way mathematical functions where a given input gets changed by addition, swapping and other operations.❗ Hash functions are used in Ethereum to determine the address given a public key.Hash functions are very important in cryptography as they are a way to get a unique identifier that is impossible to guess or to bruteforce. Hash functions have some very important properties, which are the following:Deterministic: The same hash function input should always lead to the same output.Collision resistant: The function should be with a one to one mapping. This means that given an input x, there is only one output y.Avalanche effect: A small change in the input results in a completely different hashThe hash output should be irreversible.Used in message integrity, digital fingerprints, unique identifiers, authentication (password hashing)Ethereum uses keccak-256 for its hash function. An easy way to test what function you are currently using is to do the null input test:Keccak256("")=c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470SHA3("")=a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a

#### Ethereum addressesIn essence, the Ethereum addresses are the last 20 byts of keccak256(public key)In the early stages of Ethereum, the address formats was something that got overlooked and a lot of funds were lost due to the fact that people were mispelling it and there was no checksumLater on, a standard is involved (namely EIP-55) which basically a backward-compatible comparison which allows for capitalization to be ignored. (meaning it is case insensitive.)
