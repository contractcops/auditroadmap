# Uninitialized Storage Pointers

The EVM stores data either as storage or as memory.

## The Vulnerability

Uninitialized local storage variables may contain the value of other storage variables in the contract. This fact can cause unintentional vulnerabilities, or be exploited deliberately.

![Alt text](<../Common Attack Vectors/image/Uninitialized storage pointers/name\_registrar.png>)

This simple name registrar has only one function. When the contract is unlocked, it allows anyone to register a name (as a bytes32 hash) and map that name to an address.

Here we have a vulnerability that allows name registration regardless of the unlocked variable.

As a high-level overview, state variables are stored sequentially in slots as they appear in the contract.

```
    Thus, unlocked exists in slot[0], registeredNameRecord in slot[1], and resolve in slot[2], etc
```

Each of these slots is 32 bytes in size. The Boolean unlocked will look like 0x000...0 (64 0s, excluding the 0x) for false or 0x000...1 (63 0s) for true. As you can see, there is a significant waste of storage in this particular example.

The next piece of the puzzle is that Solidity by default puts complex data types, such as structs, in storage when initializing them as local variables. Therefore, newRecord on line 18 defaults to storage.

The vulnerability is caused by the fact that newRecord is not initialized.

Because it defaults to storage, it is mapped to storage slot\[0], which currently contains a pointer to unlocked.

> Notice that on lines 19 and 20 we then set newRecord.name to \_name and newRecord.mappedAddress to \_mappedAddress; this updates the storage locations of slot\[0] and slot\[1], which modifies both unlocked and the storage slot associated with registeredNameRecord!!

## Preventative Techniques

The Solidity compiler shows a warning for unintialized storage variables developers should pay careful attention to these warnings when building smart contracts.

It is often good practice to explicitly use the memory or storage specifiers when dealing with complex types, to ensure they behave as expected.
