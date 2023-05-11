# Smart Contracts and Solidity

## What Is a Smart Contract?

> immutable computer programs that run deterministically in the context of an Ethereum Virtual Machine as part of the Ethereum network protocol—i.e., on the decentralized Ethereum world computer.

* > Computer programs Smart contracts are simply computer programs. The word “contract” has no legal meaning in this context.
* > Immutable Once deployed, the code of a smart contract cannot change. Unlike with tradi‐ tional software, the only way to modify a smart contract is to deploy a new instance.
* > Deterministic The outcome of the execution of a smart contract is the same for everyone who runs it, given the context of the transaction that initiated its execution and the state of the Ethereum blockchain at the moment of execution.
* > EVM context Smart contracts operate with a very limited execution context. They can access their own state, the context of the transaction that called them, and some infor‐ mation about the most recent blocks
* > Decentralized world computer The EVM runs as a local instance on every Ethereum node, but because all instances of the EVM operate on the same initial state and produce the same final state, the system as a whole operates as a single “world computer.”

## Life Cycle of a Smart Contract

Contracts only run if they are called by a transaction. All smart contracts in Ethereum are executed, ultimately, because of a transaction initiated from an EOA.

A contract can call another contract that can call another contract, and so on, but the first contract in such a chain of execution will always have been called by a transac‐ tion from an EOA. Contracts never run “on their own” or “in the background.”

* > Successful termination means that the program executed without an error and reached the end of execution.
* > If execution fails due to an error, all of its effects (changes in state) are “rolled back” as if the transaction never ran.

A failed transaction is still recorded as having been attempted, and the ether spent on gas for the execution is deducted from the originating account, but it otherwise has no other effects on contract or account state.

It is important to remember that a contract’s code cannot be changed. However, a contract can be “deleted,” removing the code and its internal state (storage) from its address, leaving a blank account. Any transactions sent to that account address after the contract has been deleted do not result in any code execu‐ tion, because there is no longer any code there to execute.

> To delete a contract, you execute an EVM opcode called SELFDESTRUCT

> That operation costs “negative gas,” a gas refund, thereby incentivizing the release of network client resources from the deletion of stored state

> If the contract’s code does not have a SELFDESTRUCT opcode, or it is inaccessible, the smart contract cannot be deleted.

## Introduction to Ethereum High-Level Languages

The EVM is a virtual machine that runs a special form of code called EVM bytecode, analogous to your computer’s CPU, which runs machine code such as x86\_64.Most Ethereum developers use a high-level language to write programs, and a compiler to convert them into bytecode.

While any high-level language could be adapted to write smart contracts, adapting an arbitrary language to be compilable to EVM bytecode is quite a cumbersome exercise and would in general lead to some amount of confusion.

* > LLL A functional (declarative) programming language, with Lisp-like syntax. It was the first high-level language for Ethereum smart contracts but is rarely used today.
* > Serpent A procedural (imperative) programming language with a syntax similar to Python. Can also be used to write functional (declarative) code, though it is not entirely free of side effects.
* > Solidity A procedural (imperative) programming language with a syntax similar to Java‐ Script, C++, or Java. The most popular and frequently used language for Ethereum smart contracts.
* > Vyper A more recently developed language, similar to Serpent and again with Pythonlike syntax. Intended to get closer to a pure-functional Python-like language than Serpent, but not to replace Serpent.
* > Bamboo A newly developed language, influenced by Erlang, with explicit state transitions and without iterative flows (loops). Intended to reduce side effects and increase auditability. Very new and yet to be widely adopted.

## Building a Smart Contract with Solidity

Solidity was created by Dr. Gavin Wood. In this chapter we will improve our Faucet contract

> <img src="../../Mastering%20Ethereum%20Book/Mastering%20Ethereum%20Book/image/Chapter7_/faucetSol.png" alt="faucetSol.png" data-size="original">

## The Ethereum Contract ABI

An application binary interface is an interface between two pro‐ gram modules; often, between the operating system and user programs.

> An ABI defines how data structures and functions are accessed in machine code

this is not to be confused with an API, which defines this access in high-level, often humanreadable formats as source code.

In Ethereum, the ABI is used to encode contract calls for the EVM and to read data out of transactions. The purpose of an ABI is to define the functions in the contract that can be invoked and describe how each function will accept arguments and return its result.

> A contract’s ABI is specified as a JSON array of function descriptions and events.

A function description is a JSON object with fields type, name, inputs, outputs, constant, and payable. An event description object has

> fields type, name, inputs, and anonymous.

> All that is needed for an application to interact with a contract is an ABI and the address where the contract has been deployed.

## Selecting a Solidity Compiler and Language Version

Adding a version pragma is a best practice, as it avoids problems with mismatched compiler and language versions.

## Programming with Solidity

## Data Types

First, let’s look at some of the basic data types offered in Solidity:

* > Boolean (bool) Boolean value, true or false, with logical operators ! (not), && (and), || (or), == (equal), and != (not equal).
* > Integer (int, uint) Signed (int) and unsigned (uint) integers, declared in increments of 8 bits from int8 to uint256. Without a size suffix, 256-bit quantities are used, to match the word size of the EVM.
* > Fixed point (fixed, ufixed) Fixed-point numbers, declared with (u)fixedMxN where M is the size in bits (increments of 8 up to 256) and N is the number of decimals after the point (up to 18); e.g., ufixed32x2.
* > Address A 20-byte Ethereum address. The address object has many helpful member functions, the main ones being balance (returns the account balance) and transfer (transfers ether to the account).
* > Byte array (fixed) Fixed-size arrays of bytes, declared with bytes1 up to bytes32.
* > Byte array (dynamic) Variable-sized arrays of bytes, declared with bytes or string
* > Enum User-defined type for enumerating discrete values: enum NAME {LABEL1, LABEL 2, ...}
* > Arrays An array of any type, either fixed or dynamic: uint32\[]\[5] is a fixed-size array of five dynamic arrays of unsigned integers.
* > Struct User-defined data containers for grouping variables: struct NAME {TYPE1 VARIABLE1; TYPE2 VARIABLE2; ...}.
* > Mapping Hash lookup tables for key ⇒ value pairs: mapping(KEY\_TYPE ⇒ VALUE\_TYPE) NAME.

In addition to these data types, Solidity also offers a variety of value literals that can be used to calculate different units

* > Time units The units seconds, minutes, hours, and days can be used as suffixes, converting to multiples of the base unit seconds.
* > Ether units The units wei, finney, szabo, and ether can be used as suffixes, converting to multiples of the base unit wei.

## Predefined Global Variables and Functions

Transaction/message call context The msg object is the transaction call (EOA originated) or message call (contract ori‐ ginated) that launched this contract execution. It contains a number of useful attributes:

> msg.sender:

* > It represents the address that initiated this contract call, not necessarily the originating EOA that sent the transaction. If our contract was called directly by an EOA transaction, then this is the address that signed the transaction, but otherwise it will be a contract address.

> msg.value

* > The value of ether sent with this call (in wei).

> msg.gas

* > The amount of gas left in the gas supply of this execution environment. This was deprecated in Solidity v0.4.21 and replaced by the gasleft function.

> msg.data

* > The data payload of this call into our contract.

> msg.sig

* > The first four bytes of the data payload, which is the function selector.

Whenever a contract calls another contract, the values of all the attributes of msg change to reflect the new caller’s information.

> The only exception to this is the delegatecall function, which runs the code of another contract/library within the original msg context.

## Transaction context

The tx object provides a means of accessing transaction-related information:

> tx.gasprice

* > The gas price in the calling transaction

> tx.origin The address of the originating EOA for this transaction. WARNING: unsafe!

## Block context

The block object contains information about the current block:

> block.blockhash(blockNumber)

* > The block hash of the specified block number, up to 256 blocks in the past. Dep‐ recated and replaced with the blockhash function in Solidity v0.4.22.
* > block.coinbase The address of the recipient of the current block’s fees and block reward.

> block.difficulty

* > The difficulty (proof of work) of the current block.

> block.gaslimit

* > The maximum amount of gas that can be spent across all transactions included in the current block.

> block.number

* > The current block number (blockchain height).

> block.timestamp

* > The timestamp placed in the current block by the miner (number of seconds since the Unix epoch).

## Address object

Any address, either passed as an input or cast from a contract object, has a number of attributes and methods:

> address.balance

* > The balance of the address, in wei. For example, the current contract balance is address(this).balance.

> address.transfer(amount):

* > Transfers the amount (in wei) to this address, throwing an exception on any error. We used this function in our Faucet example as a method on the msg.sender address, as msg.sender.transfer

> address.send(amount)

* > Similar to transfer, only instead of throwing an exception, it returns false on error. WARNING: always check the return value of send

> address.call(payload):

* > Low-level CALL function—can construct an arbitrary message call with a data payload. Returns false on error. WARNING: unsafe—recipient can (accidentally or maliciously) use up all your gas, causing your contract to halt with an OOG exception; always check the return value of call. //OOG exception is Out Of Gas exception

## Built-in functions

> addmod, mulmod

* > For modulo addition and multiplication. For example, addmod(x,y,k) calculates (x + y) % k

> keccak256, sha256, sha3, ripemd160

* > Functions to calculate hashes with various standard hash algorithms.

> ecrecover

* > Recovers the address used to sign a message from the signature.

> selfdestrunct(recipient\_address)

* > Deletes the current contract, sending any remaining ether in the account to the recipient address.The address of the currently executing contract account.

## Contract Definition

Solidity’s principal data type is contract; Similar to any object in an object-oriented language, the contract is a container that includes data and methods.

> Interface

* > An interface definition is structured exactly like a contract, except none of the functions are defined, they are only declared. This type of declaration is often called a stub; it tells you the functions’ arguments and return types without any implementation. An interface specifies the “shape” of a contract; when inherited, each of the functions declared by the interface must be defined by the child.

> Library

* > A library contract is one that is meant to be deployed only once and used by other contracts, using the delegatecall method

> Functions

* > Within a contract, we define functions that can be called by an EOA transaction or another contract. In our Faucet example, we have two functions: withdraw and the fallback function.

The syntax we use to declare a function in Solidity is as follows:

> <img src="../../Mastering%20Ethereum%20Book/Mastering%20Ethereum%20Book/image/Chapter7_/functionsSyntax.png" alt="functionsSyntax.png" data-size="original">

> FunctionName

* > The name of the function, which is used to call the function in a transaction (from an EOA), from another contract, or even from within the same contract. One function in each contract may be defined without a name, in which case it is the fallback function, which is called when no other function is named. The fall‐ back function cannot have any arguments or return anything

> Parameters

* > Following the name, we specify the arguments that must be passed to the func‐ tion, with their names and types. In our Faucet example we defined uint with draw\_amount as the only argument to the withdraw function.

The next set of keywords (public, private, internal, external) specify the func‐ tion’s visibility:

> public

* > Public is the default; such functions can be called by other contracts or EOA transactions, or from within the contract. In our Faucet example, both functions are defined as public.

> external

* > External functions are like public functions, except they cannot be called from within the contract unless explicitly prefixed with the keyword this.

> internal

* > Internal functions are only accessible from within the contract—they cannot be called by another contract or EOA transaction. They can be called by derived contracts (those that inherit this one).

> private

* > Private functions are like internal functions but cannot be called by derived contracts.

> ❗Keep in mind that the terms internal and private are somewhat misleading. Any func‐ tion or data inside a contract is always visible on the public blockchain, meaning that anyone can see the code or data. The keywords described here only affect how and when a function can be called.

The second set of keywords (pure, constant, view, payable) affect the behavior of the function:
