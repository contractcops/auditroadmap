# Race Conditions/Front Running

As with most blockchains, Ethereum nodes pool transactions and forms them into blocks. The transactions are only considered valid once a miner has solved a consensus mechanism

> The miner who solves the block also chooses which transactions from the pool will be included in the block, typically ordered by the gasPrice of each transaction. Here is a potential attack vector. An attacker can watch the transaction pool for transactions that may contain solutions to problems, and modify or revoke the solver’s permissions or change state in a contract detrimentally to the solver. The attacker can then get the data from this transaction and create a transaction of their own with a higher gasPrice so their transaction is included in a block before the original.

![FindThisHash](<../Common Attack Vectors/image/Race conditions \~ Front Running/FindThisHash.png>)

Say this contract contains 1,000 ether. The user who can find the preimage of the above(from the code snippet) SHA-3 hash, can submit the solution and retrieve the 1,000 ether.

Let’s say one user figures out the solution is Ethereum. They call solve with Ethereum! as the parameter. Unfortunately, an attacker has been clever enough to watch the transaction pool for anyone submitting a solution.

1. They see this solution
2. Check its validity
3. And then submit an equivalent transaction with a much higher gasPrice than the original transaction.

The miner who solves the block will likely give the attacker preference due to the higher gasPrice, and mine their transaction before the original solver’s.

```
The attacker will take the 1,000 ether, and the user who solved the problem will get nothing.
```

> Keep in mind that in this type of “front-running” vulnerability, miners are uniquely incentivized to run the attacks themselves (or can be bribed to run these attacks with extravagant fees). The possibility of the attacker being a miner themselves should not be underestimated!

## Preventative Techniques

1. One method is to place an upper bound on the gasPrice. This prevents users from increasing the gasPrice and getting preferential transaction ordering beyond the upper bound.

This measure only guards against the first class of attackers (arbitrary users). Miners in this scenario can still attack the contract, as they can order the transactions in their block however they like, regardless of gas price.

2. A more robust method is to use a commit–reveal scheme. Such a scheme dictates that users send transactions with hidden information (typically a hash). After the transaction has been included in a block, the user sends a transaction revealing the data that was sent (the reveal phase).

This method prevents both miners and users from frontrunning transactions, as they cannot determine the contents of the transaction

> This method, however, cannot conceal the transaction value. In some cases this is the valuable information that needs to be hidden.

The ENS smart contract allowed users to send transactions whose committed data included the amount of ether they were willing to spend. Users could then send transactions of arbitrary value. During the reveal phase, users were refunded the difference between the amount sent in the transaction and the amount they were willing to spend.

## Real-World Example: ERC20

The ERC20 standart has a potential front-running vulnerability that comes about due to the approve function

The standard specifies the approve function as:

```
function approve(address _spender, uint256 _value) returns (bool success)
```

This function allows a user to permit other users to transfer tokens on their behalf.

The front-running vulnerability occurs in the scenario where a user Alice approves her friend Bob to spend 100 tokens.

Alice later decides that she wants to revoke Bob’s approval to spend, say, 100 tokens, so she creates a transaction that sets Bob’s allocation to 50 tokens. Bob, who has been carefully watching the chain, sees this transaction and builds a transaction of his own spending the 100 tokens. He puts a higher gasPrice on his transaction than Alice’s, so gets his transaction prioritized over hers

Some implementations of approve would allow Bob to transfer his 100 tokens and then, when Alice’s transaction is committed, reset Bob’s approval to 50 tokens, in effect giving Bob access to 150 tokens.
