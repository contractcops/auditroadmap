# Decenralized Applications(DApps)

Smart contracts are a way to decentralize the controlling logic and payment functions of applications. Web3 DApps are about decentralizing all other aspects of an application: storage, messaging, naming, etc.

let’s take a closer look at the defining characteristics and advantages of DApps.

## What Is a DApp?

A DApp is an application that is mostly or entirely decentralized. Consider all the possible aspects of an application that may be decentralized:

1. Backend software (application logic)
2. Frontend software
3. Data storage
4. Message communications
5. Name resolution

Each of these can be somewhat centralized or somewhat decentralized. For example, a frontend can be developed as a web app that runs on a centralized server, or as a mobile app that runs on your device. The backend and storage can be on private servers and proprietary databases, or you can use a smart contract and P2P storage.

There are many advantages to creating a DApp that a typical centralized architecture cannot provide:

> Resiliency

* > DApp backend will be fully distributed and managed on a blockchain platform. Unlike an application deployed on a centralized server, a DApp will have no downtime and will continue to be available as long as the platform is still operating.

> Transparency

* > The on-chain nature of a DApp allows everyone to inspect the code and be more sure about its function. Any interaction with the DApp will be stored forever in the blockchain.

> Censorship resistance

* > As long as a user has access to an Ethereum node (running one if necessary), the user will always be able to interact with a DApp without interference from any centralized control. No service provider, or even the owner of the smart contract, can alter the code once it is deployed on the network.

## Backend (Smart Contract)

Ethereum smart contracts allow you to build architectures in which a network of smart contracts call and pass data between each other, reading and writing their own state variables as they go, with their complexity restricted only by the block gas limit.

One major consideration of smart contract architecture design is the inability to change the code of a smart contract once it is deployed. It can be deleted if it is programmed with an accessible SELFDESTRUCT opcode, but other than complete removal, the code cannot be changed in any way.

The second major consideration of smart contract architecture design is DApp size. A really large monolithic smart contract may cost a lot of gas to deploy and use. Therefore, some applications may choose to have off-chain computation and an external data source.

Having the core business logic of the DApp be dependent on external data (e.g., from a centralized server) means your users will have to trust these external resources.

## Frontend (Web User Interface)

The client-side interface of a DApp can use standard web technologies (HTML, CSS, JavaScript, etc.). This allows a traditional web developer to use familiar tools, libraries, and frameworks.

> Interactions with Ethereum, such as signing messages, sending transactions, and managing keys, are often conducted through the web browser, via an extension such as MetaMask.

The frontend is usually linked to Ethereum via the web3.js JavaScript library, which is bundled with the frontend resources and served to a browser by a web server.

## Data Storage

Due to high gas costs and the currently low block gas limit, most DApps utilize off-chain data storage services, they store the bulky data off the Ethereum chain, on a data storage platform.

> That data storage platform can be centralized for example, a typical cloud database, or decentralize - stored on a P2P platform such as the IPFS, or Ethereum’s own Swarm platform.

> Decentralized P2P storage is ideal for storing and distributing large static assets

## IPFS

IPFS stands for Inter-Planetary File System, it's a decentralized content-addressable storage system that distributes stored objects among peers in a P2P network.

> Content addressable means that each piece of content (file) is hashed and the hash is used to identify that file. You can then retrieve any file from any IPFS node by requesting it by its hash.

IPFS aims to replace HTTP as the protocol of choice for delivery of web applications. Instead of storing a web application on a single server, the files are stored on IPFS and can be retrieved from any IPFS node.

## Swarm

Swarm is another content-addressable P2P storage system, similar to IPFS. It was created by the Ethereum Foundation, as part of the Go-Ethereum suite of tools.

Like IPFS, it allows you to store files that get disseminated and replicated by Swarm nodes. You can access any Swarm file by referring to it by a hash. Swarm allows you to access a website from a decentralized P2P system, instead of a central web server.

## Decentralized Message Communications Protocols

Another major component of any application is inter-process communication. That means being able to exchange messages between applications, between different instances of the application, or between users of the application.

> There are a variety of decentralized alternatives to server-based protocols, offering messaging over a P2P network. The most notable P2P messaging protocol for DApps is Whisper, which is part of the Ethereum Foundation’s Go-Ethereum suite of tools.

## A Basic DApp Example: Auction DApp

This Auction DApp allows a user to register a “deed” token, which represents some unique asset, such as a house, a car, a trademark, etc. Once a token has been registered, the ownership of the token is transferred to the Auction DApp, allowing it to be listed for sale. The Auction DApp lists each of the registered tokens, allowing other users to place bids. During each auction, users can join a chat room created specifically for that auction. Once an auction is finalized, the deed token ownership is transferred to the winner of the auction.

The overall auction process is the following:

You can find the source code for the auction DApp in the book’s repository. - https://github.com/ethereumbook/ethereumbook/tree/develop/code/auction\_dapp

![DApp\_Architecture.png](<../Mastering Ethereum Book/image/Chapter12-DApps/DApp\_Architecture.png>)

## Auction DApp: Backend Smart Contracts

The DApp is supported by two smart contracts

```
    AuctionRepository and DeedRepository
```

> The DeedRepository contract is an ERC721-compatible non-fungible token.

The Auction DApp uses the DeedRepository contract to issue and track tokens for each auction.

> The auction itself is orchestrated by the AuctionRepository contract.

## DApp governance

If you read through the two smart contracts of the Auction DApp you will notice something important: there is no special account or role that has special privileges over the DApp.

> This is a deliberate choice to decentralize the governance of the DApp and relinquish any control once it has been deployed.

Some DApps, by comparison, have one or more privileged accounts with special capabilities, such as the ability to terminate the DApp contract, to override or change its configuration, or to “veto” certain operations. Usually, these governance functions are introduced in the DApp in order to avoid unknown problems that might arise due to a bug.

The issue of governance is a particularly difficult one to solve, as it represents a double-edged sword. On the one side, privileged accounts are dangerous; if compromised, they can subvert the security of the DApp. On the other side, without any privileged account, there are no recovery options if a bug is found.

> We have seen both of these risks manifest in Ethereum DApps

## Auction DApp: Frontend User Interface

You can find the user interface code in the code/auction\_dapp/frontend folder in the book’s repository - https://github.com/ethereumbook/ethereumbook/tree/develop/code/auction\_dapp

Once the Auction DApp’s contracts are deployed, you can interact with them using your favorite JavaScript console and web3.js, or another web3 library.

## Further Decentralizing the Auction DApp

There are two things we can do to make this DApp decentralized and resilient:

1. Store all the application code on Swarm or IPFS
2. Access the DApp by reference to a name, using the Ethereum Name Service.

## Storing the Auction DApp on Swarm

Our Auction DApp already uses Swarm to store the icon image for each auction.

> This is a much more efficient solution than attempting to store data on Ethereum, which is expensive. It is also a lot more resilient than if these images were stored in a centralized service like a web server or file server.

## Preparing Swarm and Uploading files to Swarm

To get started, you need to install Swarm and initialize your Swarm node. Swarm is part of the Ethereum Foundation’s Go-Ethereum suite of tools.

Once you have your local Swarm node and gateway running, you can upload to Swarm and the files will be accessible on any Swarm node, simply by reference to the file hash.

Now, our entire Auction DApp is hosted on Swarm and accessible by the Swarm URL:

```
    bzz://ab164cf37dc10647e43a233486cdeffa8334b026e32a480dd9cbd020c12d4581
```

A URL like that is much less user-friendly than a nice name like auction\_dapp.com.

In the next section we will examine Ethereum’s name service, which allows us to use easy-to-read names but still preserves the decentralized nature of our application.

## The Ethereum Name Service (ENS)

The Ethereum Foundation donation address is 0xfB6916095ca1df60 bB79Ce92cE3Ea74c37c5d359; in a wallet that supports ENS, it’s simply ethereum.eth.

ENS is more than a smart contract; it’s a fundamental DApp itself, offering a decentralized name service. Furthermore, ENS is supported by a number of DApps for registration, management, and auctions of registered names.

## History of Ethereum Name Services

ENS was launched on Star Wars Day, May 4, 2017 (after a failed attempt to launch it on Pi Day, March 15)

## The ENS Specification

ENS is specified mainly in three Ethereum Improvement Proposals.

ENS follows a “sandwich” design philosophy: a very simple layer on the bottom, followed by layers of more complex but replaceable code, with a very simple top layer that keeps all the funds in separate accounts.

## Bottom Layer: Name Owners and Resolvers

The ENS operates on “nodes” instead of human-readable names: a human-readable name is converted to a node using the “Namehash” algorithm.

> The base layer of ENS is a cleverly simple contract (less than 50 lines of code) defined by ERC137 that allows only nodes’ owners to set information about their names and to create subnodes (the ENS equivalent of DNS subdomains).

> The only functions on the base layer are those that enable a node owner to set information about their own node (specifically the resolver, time to live, or transferring the ownership) and to create owners of new subnodes.

## The Namehash algorithm

> Namehash is a recursive algorithm that can convert any name into a hash that identifies the name.

> For example, the Namehash node of subdomain.example.eth is keccak('example.eth' node) + keccak('subdomain'). The subproblem we must solve is to compute the node for example.eth, which is keccak('<.eth>' node) + keccak('example').

## How to choose a valid name

You could use labels and domains of any length, but for the sake of compatibility with legacy DNS, the following rules are recommended:

1. Labels should be no more than 64 characters each.
2. Complete ENS names should be no more than 255 characters.
3. Labels should not start or end with hyphens, or start with digits.

## Root node ownership

Currently the purpose and goal of these keyholders is to work in consensus with the community to:

1. Migrate and upgrade the temporary ownership of the .eth TLD to a more permanent contract once the system is evaluated.
2. Allow adding new TLDs, if the community agrees they are needed.
3. Migrate the ownership of the root multisig to a more decentralized contract, when such a system is agreed upon, tested, and implemented.
4. Serve as a last-resort way to deal with any bugs or vulnerabilities in the top-level registries.

## Resolvers

The basic ENS contract can’t add metadata to names; that is the job of so-called “resolver contracts.” These are user-created contracts that can answer questions about the name, such as what Swarm address is associated with the app, what address receives payments to the app (in ether or tokens), or what the hash of the app is (to verify its integrity).

## Middle Layer: The .eth Nodes

At the time of writing, the only top-level domain that is uniquely registrable in a smart contract is .eth.

> .eth domains are distributed via an auction system.

## Vickrey auctions

Names are distributed via a modified Vickrey auction. In a traditional Vickrey auction, every bidder submits a sealed bid, and all of them are revealed simultaneously, at which point the highest bidder wins the auction but only pays the second-highest bid. Therefore bidders are incentivized not to bid less than the true value of the name to them, since bidding their true value increases the chance they will win but does not affect the price they will eventually pay.

On a blockchain, some changes are required:

* > To ensure bidders don’t submit bids they have no intention of paying, they must lock up a value equal to or higher than their bid beforehand, to guarantee the bid is valid.
* > Because you can’t hide secrets on a blockchain, bidders must execute at least two transactions (a commit–reveal process), in order to hide the original value and name they bid on.
* > Since you can’t reveal all bids simultaneously in a decentralized system, bidders must reveal their own bids themselves; if they don’t, they forfeit their locked-up funds. Without this forfeit, one could make many bids and choose to reveal only one or two, turning a sealed-bid auction into a traditional increasing price auction.

Therefore, the auction is a four-step process:

![steps\_in\_vickery\_auction.png](<../Mastering Ethereum Book/image/Chapter12-DApps/steps\_in\_vickery\_auction.png>)

## Top Layer: The Deeds

The top layer of ENS is yet another super-simple contract with a single purpose: to hold the funds.

When you win a name, the funds are not actually sent anywhere, but are just locked up for the period you want to hold the name This works like a guaranteed buyback: if the owner does not want the name any more they can sell it back to the system and recover their ether.

ENS creates a deed contract for each new name. The deed contract is very simple (about 50 lines of code), and it only allows the funds to be transferred back to a single account (the deed owner) and to be called by a single entity (the registrar contract). This approach drastically reduces the attack surface where bugs can put the funds at risk.

## Registering a Name

Registering a name in ENS is a four-step process, as we saw in Vickery auctions.

* > First we place a bid for any available name
* > Then we reveal our bid after 48 hours to secure the name

![timeline.png](<../Mastering Ethereum Book/image/Chapter12-DApps/timeline.png>)

## Managing Your ENS Name

Once you have registered an ENS name, you can manage it using another user-friendly interface: ENS Manger

## ENS Resolvers

In ENS, resolving a name is a two-step process:

1. The ENS registry is called with the name to resolve after hashing it. If the record exists, the registry returns the address of its resolver.
2. The resolver is called, using the method appropriate to the resource being requested. The resolver returns the desired result.

> This two-step process has several benefits. Separating the functionality of resolvers from the naming system itself gives us a lot more flexibility. The owners of names can use custom resolvers to resolve any type or resource, extending the functionality of ENS

## From App to DApp

The auction DApp uses the decentralized storage system Swarm to store application resources such as images. The DApp also uses the decentralized communications protocol Whisper to offer an encrypted chat room for each auction, without any central servers.

The final result is a DApp that has no central point of authority, no central point of failure, and expresses the “web3” vision.

![fromApp\_to\_DApp.png](<../Mastering Ethereum Book/image/Chapter12-DApps/fromApp\_to\_DApp.png>)

## Conclusions

Decentralized applications are the culmination of the Ethereum vision, as expressed by the founders from the very earliest designs. While a lot of applications call themselves “DApps” today, most are not fully decentralized.

Over time, as the technology matures further, more and more of our applications can be decentralized, resulting in a more resilient, censorship-resistant, and free web.
