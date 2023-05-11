# Tokens

Nowadays, “tokens” administered on blockchains are redefining the word to mean blockchain-based abstractions that can be owned and that represent assets, currency, or access rights.

Many blockchain tokens serve multiple purposes globally and can be traded for each other or for other currencies on the global liquid markets.

In this chapter, we look at various uses for tokens and how they are created. We also discuss attributes of tokens such as fungibility and intrinsicality. Finally, we examine the standards and technologies that they are based on, and experiment by building our own tokens.

## How Tokens are used

The most obvious use of tokens is as digital private currencies. However, this is only one possible use. Tokens can be programmed to serve many different functions, often overlapping. For example a token can simultaneously convey a voting right, an access right, and ownerhsip of a resource. As the following list shows, currency is just the first "app".

> Currency

* > A token can serve as a form of currency, with a value determined trough private trade

> Resource

* > A token can represent a resource earned or produced in a sharing economy or resource-sharing environment.

> Asset

* > A token can represent ownership of an intrinsic or extrinsic , tangible or intangible asset; for example gold, real estate, a car, oil, energy, MMOG items, etc.

> Access

* > A token can represent access rights and grand access to a digital or physical property, such as a discussion forum, an exclusive website, a hotel room, or a rental car.

> Equity

* > A token can represent shareholder equity in a digital organization(e.g., a DAO) or legal entity

> Voting

* > A token can represent voting rights in a digital or legal system.

> Collectible

* > A token can represent a digital collectible (e.g., CryptoPunks) or physical collectible(e.g., a painting)

> Identity

* > A token can represent a digital identity (e.g., avatar) or legal identity (e.g., national ID)

> Attestation

* > A token can represent a certification or attestation of fact by some authority or by decentralized reputation system (e.g., marriage record, birth certificate, college degree)

> Utility

* > A token can be used to access or pay for a service.

Often, a single token encompasses several of these functions. Sometimes it is hard to discern between them, as the physical equivalents have always been inextricably linked. For example, in the physical world, a driver's license (attestation) is also an identity document (identity) and the two cannot be separated. In the digital realm, previously commingled functions can be separated and developed independently(e.g., an anonymous attestation).

## Tokens and Fungibility

Tokens are fungible when we can substitute any single unit of the token for another without any difference in its value or function.

Strictly speaking, if a token’s historical provenance can be tracked, then it is not entirely fungible. The ability to track provenance can lead to blacklisting and white‐ listing, reducing or eliminating fungibility.

Non-fungible tokens are tokens that each represent a unique tangible or intangible item and therefore are not interchangeable. For example, a token that represents ownership of a specific Van Gogh painting is not equivalent to another token that rep‐ resents a Picasso, even though they might be part of the same “art ownership token” system.

Each non-fungible token is associated with a unique identifier, such as a serial number.

## Counterparty Risk

Counterparty risk is the risk that the other party in a transaction will fail to meet their obligations.

## Tokens and Intrinsicality

Some tokens represent digital items that are intrinsic to the blockchain. Those digital assets are governed by consensus rules, just like the tokens themselves.

This has an important implication: tokens that represent intrinsic assets do not carry additional counterparty risk. If you hold the keys for a CryptoKitty, there is no other party holding that CryptoKitty for you-you ownit directly. The blockchain consensus rules apply and your ownership(i.e., control) of the private keys is equivalent to ownership of the asset, without any intermediary.

Conversely, many tokens are used to represent extrinsic things, such as real estate, corporate voting shares, trademarks, and gold bars. The ownership of these items, which are not “within” the blockchain, is governed by law, custom, and policy, sepa‐ rate from the consensus rules that govern the token.

In other words, token issuers and owners may still depend on real-world non-smart contracts. As a result, these extrinsic assets carry additional counterparty risk because they are held by custodi‐ ans, recorded in external registries, or controlled by laws and policies outside the blockchain environment.

One of the most important ramifications of blockchain-based tokens is the ability to convert extrinsic assets into intrinsic assets and thereby remove counterparty risk.

## Using Tokens: Utility or Equity

Utility tokens are those where the use of the token is required to gain access to a ser‐ vice, application, or resource. Examples of utility tokens include tokens that represent resources such as shared storage, or access to services such as social media networks.

Equity tokens are those that represent shares in the control or ownership of some‐ thing, such as a startup. Equity tokens can be as limited as nonvoting shares for dis‐ tribution of dividends and profits, or as expansive as voting shares in a decentralized autonomous organization, where management of the platform is through some com‐ plex governance system based on votes by the token holders

## It’s a Duck!

By disguising equity tokens as utility tokens, many startups hope to get around these regulatory restrictions and raise money from a public offering while presenting it as a pre-sale of “service access vouchers” or, as we call them, utility tokens. Whether these thinly disguised equity offerings will be able to skirt the regulators remains to be seen.

## Utility Tokens: Who Needs Them?

The real problem is that utility tokens introduce significant risks and adoption barriers for startups.

For a startup, each innovation represents a risk and a market filter. Think of each innovation as a filter. It limits adoption to the subset of the market that can become early adopters of this innovation. Adding a second filter compounds that effect, further limiting the addressable market. You are asking your early adopters to adopt not one but two completely new technologies: the novel application/platform/ service you built, and the token economy

For a startup, each innovation introduces risks that increase the chance of failure of the startup. If you take your already risky startup idea and add a utility token, you are adding all the risks of the underlying platform (Ethereum), broader economy (exchanges, liquidity), regulatory environment (equity/commodity regulators), and technology (smart contracts, token standards). That’s a lot of risk for a startup.

Limited liquidity, limited applicability, and high conversion costs reduce the value of tokens until they are only of “token” value. So when you add a utility token to your platform, but the token can only be used on your single platform with a small market, you are recreating the conditions that made physical tokens worthless. This may indeed be the correct way to incorporate tokenization into your project.

Adopt a token because the token lifts a fundamental market barrier or solves an access problem. Don’t introduce a utility token because it is the only way you can raise money fast and you need to pretend it’s not a public securities offering.

## Tokens on Ethereum

The introduction of the first token standard on Ethereum led to an explosion of tokens. Tokens are different from ether because the Ethereum protocol does not know anything about them.

> Sending ether is an intrinsic action of the Ethereum platform, but sending or even owning tokens is not. The ether balance of Ethereum accounts is handled at the protocol level, whereas the token balance of Ethereum accounts is handled at the smart contract level.

In order to create a new token on Ethereum, you must create a new smart contract.

Once deployed, the smart contract handles everything, including ownership, trans‐ fers, and access rights.

## The ERC20 Token Standard

ERC20 is a standard for fungible tokens, meaning that different units of an ERC20 token are interchangeable and have no unique properties.

The ERC20 standard defines a common interface for contracts implementing a token, such that any compatible token can be accessed and used in the same way.

The interface consists of a number of functions that must be present in every implementation of the standard, as well as some optional functions and attributes that may be added by developers.

## ERC20 required functions and events

An ERC20-compliant token contract must provide at least the following functions and events:

> totalSupply

* > Returns the total units of this token that currently exist. ERC20 tokens can have a fixed or variable supply.

> balanceOf

* > Given an address, returns the token balance of that address.

> transfer

* > Given an address and amount, transfers that amount of tokens to that address, from the balance of the address that executed the transfer.

> transferFrom

* > Given a sender, recipient and amount, transfers tokens from one account to another. Used in combination with `approve`

> approve

* > Given a recipient address and amount , authorizes that address to execute several transfers up to that amount, from the account that issued the approval.

> allowance

* > Given an owner address and a spender address, returns the remaining amount that the spender is approved to withdraw from the owner.

> Transfer

* > Event triggered upon a successful transfer (call to transfer or transferFrom) (even for zero-value transfers)

> Approval

* > Event logged upon a successful call to approve

## ERC20 optional functions

In addition to the required functions listed in the previous section, the following optional functions are also defined by the standard:

> name

* > Returns the human-readable name (e.g., “US Dollars”) of the token.

> symbol

* > Returns a human-readable symbol(e.g., "USD") for the token.

> decimals

* > Returns the number of decimals used to divide token amounts. For example, if decimals is 2, then the token amount is divided by 100 to get its user representation.

## The ERC20 interface defined in Solidity

![interfaceERC20.png](../../Mastering%20Ethereum%20Book/Mastering%20Ethereum%20Book/image/Chapter10/interfaceERC20.png)

## ERC20 Data structures

If you examine any ERC20 implementation you will see that it contains two data structures, one to track balances and one to track allowances. In Solidity, they are implemented with a data mapping

```
    mapping(address => uint256) balances;
```

The first data mapping implements an internal table of token balances, by owner. This allows the token contract to keep track of who owns the tokens. Each transfer is a deduction from one balance and an addition to another balance:

The second data structure is a data mapping of allowances. The ERC20 contract keeps track of the allowances with a two-dimensional mapping, with the primary key being the address of the token owner, mapping to a spender address and an allowance amount

```
    mapping(address => mapping(address => amount)) public allowed
```

## ERC20 workflows: “transfer” and “approve & transferFrom"

The ERC20 token standard has two transfer functions.

> The first is a single-transaction, straight‐ forward workflow using the transfer function.

This workflow is the one used by wallets to send tokens to other wallets. The vast majority of token transactions hap‐ pen with the transfer workflow. Executing the transfer contract is very simple. If Alice wants to send 10 tokens to Bob, her wallet sends a transaction to the token contract’s address, calling the transfer function with Bob’s address and 10 as the arguments. The token contract adjusts Alice’s balance (–10) and Bob’s balance (+10) and issues a Transfer event.

> The second workflow is a two-transaction workflow that uses approve followed by transferFrom.

This workflow allows a token owner to delegate their control to another address. It is most often used to delegate control to a contract for distribution of tokens, but it can also be used by exchanges

For the approve & transferFrom workflow, two transactions are needed. Let’s say that Alice wants to allow the AliceICO contract to sell 50% of all the AliceCoin tokens to buyers like Bob and Charlie.

1. First, Alice launches the AliceCoin ERC20 contract, issuing all the AliceCoin to her own address.
2. Then, Alice launches the AliceICO con‐ tract that can sell tokens for ether.
3. Next, Alice initiates the approve & transferFrom workflow
4. She sends a transaction to the AliceCoin contract, calling approve with the address of the AliceICO contract and 50% of the totalSupply as arguments. This will trigger the Approval event.
5. Now, the AliceICO contract can sell AliceCoin.

When the AliceICO contract receives ether from Bob, it needs to send some Alice‐ Coin to Bob in return Within the AliceICO contract is an exchange rate between Ali‐ ceCoin and ether. The exchange rate that Alice set when she created the AliceICO contract determines how many tokens Bob will receive for the amount of ether sent to the AliceICO contract.

When the AliceICO contract calls the AliceCoin transfer From function, it sets Alice’s address as the sender and Bob’s address as the recipient, and uses the exchange rate to determine how many AliceCoin tokens will be transfer‐ red to Bob in the value field.

The AliceCoin contract transfers the balance from Ali‐ ce’s address to Bob’s address and triggers a Transfer event. The AliceICO contract can call transferFrom an unlimited number of times, as long as it doesn’t exceed the approval limit Alice set. The AliceICO contract can keep track of how many Alice‐ Coin tokens it can sell by calling the allowance function.

## ERC20 implementations

> Consensys EIP20

* > A simple and easy-to-read implementation of an ERC20-compatible token.

> OpenZeppelin StandardToken

* > This implementation is ERC20-compatible, with additional security precautions. It forms the basis of OpenZeppelin libraries implementing more complex ERC20-compatible tokens with fundraising caps, auctions, vesting schedules, and other features.
