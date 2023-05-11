# Constructors with Care

Before Solidity v0.4.22, constructors were defined as functions that had the same name as the contract that contained them. In such cases, when the contract name is changed in development, if the constructor name isn’t changed too it becomes a normal, callable function. As you can imagine, this can lead to some interesting contract hacks.

## The Vulnerability

![Alt text](<../Common Attack Vectors/image/Constructors with care/constructors\_with\_care.png>)

This contract collects ether and allows only the owner to withdraw it, by calling the withdraw function. The issue arises because the constructor is not named exactly the same as the contract:

> The first letter is different!

Thus, any user can call the ownerWallet function, set themselves as the owner, and then take all the ether in the contract by calling withdraw.

## Preventative Techniques

This issue has been addressed in version 0.4.22 of the Solidity compiler. This version introduced a constructor keyword that specifies the constructor, rather than requiring the name of the function to match the contract name.
