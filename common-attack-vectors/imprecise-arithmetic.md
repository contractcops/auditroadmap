# Imprecise arithmetic

> Just like in any programming language, Solidity supports Substraction, Multiplication, Division, Modulus and Exponential (x\*\*y)

In the case of integer division, the result may be truncated, which would lead to an imprecise calculation and potentially a security issue within the business logic.

To prevent this issue, if there is both multiplication and division to be done, multiplication should be placed first.

* (a \* b) / c is correct
* (a / b) \* c is incorrect
