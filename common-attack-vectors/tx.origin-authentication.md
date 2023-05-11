# Tx.Origin Authentication

tx.origin returns the address of the account that sent the transaction. (always an EOA)

What is the difference between msg.sender and tx.origin ?

msg. sender is the last caller while tx. origin is the original caller of the function.

Ok, so now let's observe the following code:

![Alt text](<../Common Attack Vectors/image/Tx.Origin Authentication/wallet\_phishing.png>)

Can you guess where is the vulnerability?

The vulnerability lies in the use of tx.origin in the transfer function. tx.origin refers to the address that initiated the transaction, which may not necessarily be the same as the owner's address. An attacker can exploit this vulnerability by using a contract to initiate the transaction and spoof the tx.origin to be the owner's address.

Here is an example of an attack:

![Alt text](<../Common Attack Vectors/image/Tx.Origin Authentication/malicious\_\_wallet\_phishing.png>)

Now, if the owner of the Wallet contract sends a transaction with enough gas to the Attack address, it will invoke the fallback function, which in turn calls the transfer function of the Wallet contract with the parameter attacker.

As a result, all funds from the Wallet contract will be withdrawn to the attacker's address. This is because the address that first initialized the call was the victim (i.e., the owner of the Wallet contract).

## Preventative Techniques

To fix this vulnerability, the transfer function should use msg.sender instead of tx.origin to ensure that the transaction is initiated by the owner of the wallet.

![Alt text](<../Common Attack Vectors/image/Tx.Origin Authentication/wallet\_phishing\_prevention.png>)

In general tx.origin should not be used for authorization in smart contracts.This isn’t to say that the tx.origin variable should never be used. It does have some legitimate use cases in smart contracts.

For example, if one wanted to deny external contracts from calling the current contract, one could implement a require of the form

```
require(tx.origin == msg.sender).
```

This prevents intermediate contracts being used to call the current contract, limiting the contract to regular codeless addresses.
