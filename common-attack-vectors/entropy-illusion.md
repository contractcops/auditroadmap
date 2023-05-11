# Entropy Illusion

All transactions on the Ethereum blockchain are deterministic state transition operations.This has the fundamental implication that there is no source of entropy or randomness in Ethereum.

## The Vulnerability

Some of the first contracts built on the Ethereum platform were based around gambling.

A common pitfall is to use future block variables that is, variables containing information about the transaction block whose values are not yet known, such as hashes, timestamps, block numbers, or gas limits. The issue with these are that they are controlled by the miner who mines the block, and as such are not truly random.

For example, a roulette smart contract with logic that returns a black number if the next block hash ends in an even number. A miner could bet $1M on black. If they solve the next block and find the hash ends in an odd number, they could happily not publish their block and mine another, until they find a solution with the block hash being an even number (assuming the block reward and fees are less than $1M). Using past or present variables can be even more devastating

## Preventative Techniques

The source of entropy must be external to the blockchain. This can be done among peers with systems such as commit–reveal, or via changing the trust model to a group of participants.This can also be done via a centralized entity that acts as a randomness oracle.

Block variables (in general, there are some exceptions) should not be used to source entropy, as they can be manipulated by miners.

Furthermore, block headers/transactions/etc should not be used. Avoid using anything intrinsic to Ethereum as a source of randomness. Afterall, Ethereum is not autonomous and everything is the result of a user making a transaction.
