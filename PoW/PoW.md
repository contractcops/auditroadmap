# PoW

## PoW

## Proof of Work

👉In short, the purpose of this consensus mechanism and others like it, is to reach consensus between a certain number of parties.

### Consensus = agreement.

👉nodes must figure out complex mathematical equations before they can validate transactions.

👉This process is called mining, and those who take part in it are known as miners.

👉These mathematical problems are hard to compute but easy to verify.

👉Once miners solve them, they are rewarded with the corresponding digital currency, referred to as a block reward.

👉As time goes by the mathematical equations become harder and harder.

👉This increase in difficulty, known as block difficulty, is intentional. The idea is to ensure that each mined coin requires a substantial amount of electricity.

❗If we take a more technical perspective though, they’re not exactly “processing transactions”. All transactions are already fully prepared to be included in the blockchain when they reach miners. Miners only need to check their validity.

### The real processing that happens is more of an endless competition between all miners, the winner of which gets to create the next block in the chain.

👉The only way for an entity to disrupt the system and take over the network is by controlling 51% of the hashing power.

👉Ultimately, a Proof of Work system ensures that a blockchain is valid.

👉For Bitcoin, it also means that the coins are not mined too quickly and miners have incentives to maintain the network. On the flipside, this is also an infinitely scalable protocol.

👉Another big drawback is the formation of mining pools (groups of miners who pool their resources together) who could potentially control the network and collectively shepard the network, adding a very real danger to a decentralized solution.

### Cryptographic hashes

Properties:

1. hard to fake
2. easy to validate In other words hash is like digital signature

1.Hard to produce, making it impossible to create (fake) a valid signature without first having access to the original data;

2.Trivial to verify. So that anyone can quickly validate data.

\
👉Each transaction hash is derived from the data of the transaction itself, and serves as a unique identifier. - For example like we discussed with SHA-256 already in the other files.

### Each block has a hash, too. And that hash needs to follow certain rules.

\


## How does a miner prove he has done the work?

👉Each miner is constantly trying to mine the next block in the chain. And they’re on a constant race with each other.

👉As they aggregate transactions sent in by users, they aggregate them in a new block, and then try to create a hash for the entire block.

👉This hash can’t be just any hash tough. The blockchain imposes some rules on it. Bitcoin, for example, requires the hash to start with a certain number of leading zeros. The actual amount of zeros required is called the network’s difficulty, and it’s a number that is adjusted every few blocks.

👉And because each and every node is doing these hard computations, you now know that they **prooved** that they've done the work without actually checking yourself. In other words you are trusting them and agree with the validations of the nodes.

👉Couple this with the fact that hashes are, for all intents and purposes, randomly generated and unguessable, and miners have only one way to produce different hashes: brute force. They keep trying over and over to generate new hashes, until they find one that fulfils the requirement.

👉Since the same block always generates the same hash, they need to change it slightly for every new attempt. This is often done by adding a random number within the block’s data. The only purpose of this number is for miners to change it in order to be able to make different blocks, and therefore, different hashes.

👉The better your hardware, the more hashes you can produce. That’s called your **Hash Rate**

👉Since this process is essentially random, having slower hardware still gives you a small chance at preventing more powerful miners from getting 100% of the blocks.

👉This difficulty is adjusted automatically every few blocks, in response to changes in the total hash rate of the network.

### 👉If a lot of miners significantly increase their hash rate, or if a lot of miners join the game, the average time to mine a block will decrease

👉In response to this, the difficulty may increase, to keep the average block time around 10 minutes.

## Steps

1. A transaction is requested and authenticated
2. A block representing that a transaction is created
3. The block is sent to every node in the network
4. Nodes validate the transaction
5. Nodes receive a reward for Proof of Work, typically in cryptocurrency
6. The block is added to the existing blockchain
7. The update is distributed accross the network
8. The transaction is complete

### How does the validation of block by node work?

👉Blocks must first be validated to be added to the blockchain. The most accepted form of validation for open-source blockchains is proof of work—the solution to a mathematical puzzle derived from the block’s header.

> The actual work of validating a block of transactions is guessing the correct nonce of the next block. Only blocks with a valid nonce may be added to the chain.

> ❗ When racing to create the next block, miners repeatedly put a certain dataset through a mathematical function. This dataset can only be obtained by downloading and running the full chain

> ❗ The dataset is used to generated a mixHash below a target that is dictated by the block difficulty.

> ❗ Once generated, it is very easy for all of the other nodes to verify its integrity and add it onto the blockchain.

> ❗ Whoever guesses the nonce first is the one to be rewarded.
