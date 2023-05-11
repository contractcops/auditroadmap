# Signature Replay

Signatures can be used in Ethereum transactions to validate computation performed off-chain, helping to minimize on-chain gas fees. Signatures are primarily used to authorize transactions on behalf of the signer and to prove that a signer signed a specific message.

It is somethimes necessary to perform signature verification in smart contracts to achieve better usability or to save gas cost.

A secure implementation needs to protect against Signature Replay Attacks.

What are Signature Replay Attacks?

## Vulnerability

An attacker can replay a previous transaction by copying its signature and passing the validation check.

Firstly let's observe the attack workflow:

![Alt text](<../Common Attack Vectors/image/Signature Replay/signatureReplaySmartContract.png>)

_image from https://www.youtube.com/watch?v=jq1b-ZDRVDc_

At first glance, looking at this image may be complicated. But let's break it down to steps.

For Eve to withdraw 1 ether, Alice signs a message that contains her signature. Eve can add her signature and send a transaction to the wallet requesting some ether. This method involves signing a message _off-chain_. It reduces gas fees.

There are three ways in which Eve can perform the replay attack in these scenarios:

1. Because Alice's message was signed off-chain and sent to Eve, Eve can decide to withdraw another 1 ETH without the knowledge of Alice. Eve can do this because she already has the signature of Alice. The contract will recognize the signature and approve the transaction.
2. If the contract prevents the above scheme from working, Eve can decide to deploy the contract at another address. Doing this will allow her to perform the same transaction without any hurdles.
3. Eve can deploy the contract by using CREATE2 and calling selfdestruct(). If this is done, the contract can be recreated at the same address and reused with all the previous messages.

## Prevention

First and foremost we must find a way to make each off-chain signature unique.

We can achieve this by the following ways:

* Adding a nonce in the contract

This way, once a signature has been used, an attacker cannot reuse a signature because the contract will recognize the nonce once a signature has been used.

* Including contract's address inside the signature

If the contract is deployed at another address, we can prevent the replay attack by including the contract's address inside the signature. We will add a nonce to prevent the first case.

## CREATE2 and SELFDESTRUCT() case

In the scenario where a contract is created with CREATE2 and selfdestruct() is called, there is no way to prevent the replay attack. We cannot prevent it because when selfdestruct() is called, the nonces are reset, and the contract no longer recognizes previously used nonces.

## Let's look at some code!

![Alt text](<../Common Attack Vectors/image/Signature Replay/signatureReplayVulnerable.png>)

We fisrt import the ECDSA.sol from OpenZeppelin

```
import "github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v4.5/contracts/utils/cryptography/ECDSA.sol";
```

Then we are initializing the variable that holds the address of the two contract admins

```
address[2] public admins
```

And then assign the values of the admins in the constructor

```
constructor(address[2] memory _admins) payable {
    admins = _admins;
}
```

The code has a deposit() function to deposit money.

```
function deposit() external payable {}
```

The transfer() function takes three parameters. First, it recreates the hash that was signed from the \_sendto and \_amount parameters.

```
bytes32 txHash = getTxHash(_sendto, _amount);
```

Next, it checks the two signatures against the hash.

```
require(_checkSignature(_sigs, txHash), "invalid sig");
```

If the two signatures are valid, it proceeds to transfer the ether.

```
// send ether if signature is valid
(bool sent, ) = _sendto.call{value: _amount}("");
require(sent, "Failed to send Ether");
```

This function uses the keccak256 hashing algorithm to hash the \_sendto and \_amount.

```
function getTxHash(address _to, uint _amount) public view returns (bytes32) {
    return keccak256(abi.encodePacked(_to, _amount));
}
```

Finally, there is a function called \_checkSignature(). The job of this function is to check if the signer's signature corresponds to the admin's signature.

First, it recomputes the signed hash by calling toEthSignedMessageHash().

```
bytes32 ethSignedHash = _txHash.toEthSignedMessageHash();
```

Next, it runs a for loop that recovers the signer of each signature.

```
for (uint i = 0; i < _sigs.length; i++) {
    // get the signer of the signature
    address signer = ethSignedHash.recover(_sigs[i]);

    //check if the signer is an admin
    bool valid = signer == admins[i];

    if (!valid) {
        return false;
    }
}
```

The loop then checks to confirm if the signer of the message is indeed an admin of the contract. If not, it returns false.

Now that we have analyzed the contract let us look at ways to protect it from a signature replay attack.

We need to make each sign unique for each transaction by creating a unique transaction hash. We can do this by including a nonce in the transaction hash.

After this, we will invalidate the hash once the transaction is carried out.

We use this mapping to invalidate each hash after a transaction has been carried out

```
mapping(bytes32 => bool) public is_executed;
```

To do this, we first check if is\_executed is false.

```
// check if is_executed is still false
require(!is_executed[txHash], "transaction has been previously executed");
```

If it is and the signatures are valid

```
require(_checkSignature(_sigs, txHash), "invalid sig");
```

We set is\_executed to true

```
is_executed[txHash] = true;
```

then send the required ether.

```
// send ether
(bool sent, ) = _sendto.call{value: _amount}("");
require(sent, "Failed to send Ether");
```

With the preventive measures taken, we can protect our contract from a replay attack that uses the signature of the admins.

Next, we must protect the contract against a replay attack where the attacker deploys the contract at _another address_.

We can do this by including the address of the contract inside the getTxHash() function.

```
function getTxHash(address _sendto, uint _amount, uint _nonce) public view returns (bytes32) {
    return keccak256(abi.encodePacked(address(this), _sendto, _amount, _nonce));
}
```

So whenever the admins sign the txHash, they sign a hash unique to the contract.

The full code for the contract protected against both forms of replay attack can be found below:

![Alt text](<../Common Attack Vectors/image/Signature Replay/signatureReplayNonceAddress.png>)

_The code is from the website: https://docs.celo.org/blog/tutorials/solidity-vulnerabilities-signature-replay-attack_
