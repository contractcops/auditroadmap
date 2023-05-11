# Oracle Manipulation

Oracle manipulation is when an oracle smart contract is manipulated by hackers.

Oracles are third-party service providers that supply blockchains with external or real-world data such as price feeds, weather information, statistics, etc.

Price feeds are by far the most exploited oracle data since they allow attackers to steal millions of funds from DeFi platforms.

Generally, there are two ways an oracle can gather price information. One is to seamlessly siphon price data from centralized exchanges via APIs. On the other hand, oracles can also do the calculations themselves by consulting decentralized exchanges (DEXs).

Both methods have their own sets of advantages and disadvantages, as well as ways of being manipulated.

## Vulnerability

A vulnerability arises when protocols relying on oracles automatically execute actions even though the oracle-provided data feed is incorrect.

## Spot Price Manipulation

A smart contract needs to determine the price of an asset, e.g., when a user deposits ETH into its system. To achieve this price discovery, the protocol consults its respective Uniswap pool as a source.

Exploiting this behavior, an attacker can take out a flash loan to drain one side of the Uniswap pool. Due to the lack of data source diversity, the protocol's internal price is directly manipulated, e.g., to 100 times the original value.

Then the attacker can now perform an action to capture this additional value.

The problems are two-fold:

1. The use of a single price feed source smart contract allows for easy on-chain manipulation using flash loans.
2. Despite a notable anomaly, the smart contracts consuming the price information continue to operate on the manipulated data.

Let's see real world example with Visitor Hack.

```
uint160 sqrtPrice = TickMath.getSqrtRatioAtTick(currentTick());

uint256 price = FullMath.mulDiv(uint256(sqrtPrice).mul(uint256(sqrtPrice)), PRECISION, 2**(96 * 2));
```

As this price data is fetched from an on-chain dependency, and the price data is determined in the current transaction context, this spot price can be manipulated in the same transaction.

1. An attacker can take out a flash loan on the incoming asset A and on the relevant Uniswap pool, swap asset A for asset B with a large volume.
2. This trade will increase the price of asset B (increased demand) and reduce the cost of asset A (increased supply)
3. When asset B is deposited into the above function, its price is still pumped up by the flash loan.
4. Consequentially, asset B gives the attacker an over-proportional amount of shares.
5. These shares can be withdrawn, giving the attacker equal parts of asset A and asset B from the pool.
6. Repeating this process will drain the vulnerable pool of all funds.
7. With the money gained from the withdrawal of their shares, the attacker can repay the flash loan.

## Centralized Oracles and Trust

Centralized trust can lead to the authorized user(s) getting incentivized to submit malicious data and abuse their position of power.

Additionally, such centralized systems can have an inherent risk due to compromised private keys.

## Decentralized Oracle Security

Decentralized oracles aim to diversify the group of data collectors to a point where disrupting a quorum of participants becomes unfeasible for an attacker. In a decentralized scenario, further security considerations stem from how participants are incentivized and what sort of misbehavior if left unpunished. Participants providing (valid) data to the oracle system are economically rewarded. Aiming to maximize their profit, the participants are incentivized to provide the cheapest version of their service possible.

## Freeloading Attacks

They are the simplest way to save work and maximize profit.

A node can leverage another oracle or off-chain component (such as an API) and simply copy the values without validation.

Besides the apparent data source centralization issue, freeloading attacks at scale can also severely affect the data's correctness.

Freeloading attacks can be easily prevented for more complex data feeds by implementing a commit-reveal scheme.

## Mirroring

Mirroring attacks are a flavor of Sybil attacks and can go hand-in-hand with freeloading.

A single node reading from the centralized data source then replicates its values across other participants who mirror that data.

With a single data read, the reward for providing the information is multiplied by the number of participants. As the number of mirroring participants grows, this increased weight on a single data point can significantly deteriorate error correction mechanisms.

A commit-reveal scheme is ineffective to mitigate (purposeful) mirroring attacks as it does not consider private data transfers between Sybil nodes. Due to the lack of transparency in Sybil communications, mirroring attacks can be very hard to detect in practice.

## Prevention

Use decentralized oracles, such as:

* Chainlink - the largest decentralized oracle provider, and the Chainlink network can be leveraged to bring decentralized data on-chain.
* Tellor - an oracle that provides censorship-resistant data, secured by economic incentives, ensuring data can be provided by anyone, anytime, and checked by everyone.
* Witnet leverages state-of-the-art cryptographic and economic incentives to provide smart contracts with off-chain data.
