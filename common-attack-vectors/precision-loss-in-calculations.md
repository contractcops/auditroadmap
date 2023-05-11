# Precision Loss in Calculations

When performing Integer division, Solidity may truncate the result.

Hence we must multiply before dividing to prevent such loss in precision

For example

```
50*100*15/15 = 5000
```

Let’s start with the division. After all, abc/c is equal to a/c \* bc.

```
50/15*100*15 = 4999.9999999999995
```

This is a result of a floating point error. Instead of floating points, we get rounding errors in Solidity, and the second operation in Solidity would produce 4999.

We minimize rounding issues as much as possible by performing all multiplications first and division later.

## Multiplication and Division of Contract Value

Imagine a function that calculates a fee based on the number of coins held.

![Alt text](<../Common Attack Vectors/image/Precision Loss in Calculations/contractFee.png>)

If we perform division first the result would always result in zero

If we were to use

```
(coins * fee)/Total_coins
```

then it would have been greater than 1. And in this example it returns 3.

Therefore we must perform multiplication operations before division unless the limit of a smaller type which makes the operation fatal.

## Prevention

* Multiplication should always be performed before division to avoid loss of precision.
* The calculated result for division and multiplication can be stored in an integer with more bits, but the operands must also be integers of the same size
* The operands for the exponentiation function must be unsigned integers. Unsigned Integers with lower bits can be calculated and stored as unsigned integers with higher bits.
* To apply an arithmetic operation to all of the operands, they must all have the same data type otherwise, the operation will not be performed.
