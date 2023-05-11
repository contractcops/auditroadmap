# Flash Loan Attack

> Flash loans are when an attack is able to borrow a huge sum of money (without collateral) from one exchange, manipulate the price and quickly sell it on the other exchange.

> This is a smart contract security exploit, where the contract allows the attacker to take big sums without staking anything as a security measure.

* Unlike traditional loaning, flash loans to do not require borrowers to provide the required documentation (POI, reserves, collateral).

Flash loans are used in order to give borrowers the opportunity to benefit from arbitrage opportunities quickly. It provides the borrowed funds to purschase a crypto asset, sell it, pay back the loan and make a profit.

Taken from shardeum.com:

> "_In layman’s terms, a flash loan works as follows: I will lend you as much money as you need for this single transaction. However, by the end of this transaction, you must repay me at least the amount I lent you. If you are unable to do so, your transaction will be automatically rolled back!_"

Another big part of the problem is that crypto prices are different in exchanges. Finding one true price for a crypto asset is very difficult.

Following the markets and trading at an upsell is a traditional way of generating profit, however, flash loan attacks manipulate prices and exploit the suddent shift to sell at an artificial amount. In hand, this creates a sharp drop in the price of a crypto asset.

### Preventative measuresOne way to prevent flash loans is by using Oracles for getting the relevant current prices. This way, the price would be more up to date and the transaction would fail if noticed that the attacker has manipulated the cryptocurrency.
