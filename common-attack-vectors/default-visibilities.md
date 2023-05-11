# Default Visibilities

Functions in Solidity have visibility specifiers that dictate how they can be called. The visibility determines whether a function can be called externally by users, by other derived contracts, only internally, or only externally.

## The Vulnerability

The default visibility for functions is public, so functions that do not specify their visibility will be callable by external users. The issue arises when developers mistakenly omit visibility specifiers on functions that should be private(or only callable within the contract itself).

![Default\_visibilities](<../Common Attack Vectors/image/Default\_visibilities/Default\_visibilities.png>)

## Preventative Techniques

It is good practice to always specify the visibility of all functions in a contract, even if they are intentionally public.
