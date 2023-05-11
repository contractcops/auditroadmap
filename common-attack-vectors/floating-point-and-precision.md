# Floating Point and Precision

Solidity does not support floating point values there are many reasons but the most important is that solidity deals with real money and as we all know while division we lose some minor point values when we convert the value back to an integer due to that solidity does not support it.

This means that floating-point representations must be constructed with integer types in Solidity. This can lead to errors and vulnerabilities if not implemented correctly.

> There is no native support for floating-point numbers in the core language, but they are available via libraries.

## The Vulnerability

![Alt text](<../Common Attack Vectors/image/Floating point and precision/floating point\_and\_precision.png>)

This simple token buying/selling contract has some obvious problems. Although the mathematical calculations for buying and selling tokens are correct, the lack of floating-point numbers will give erroneous results.

For example, when buying tokens, if the value is > 1 ether the initial division will result in 0, leaving the result of the final multiplication as 0.

Similarly, when selling tokens, any number of tokens less than 10 will also result in 0 ether. In fact, rounding here is always down, so selling 29 tokens will result in 2 ether.

This can get tricky when dealing with decimals in ERC20 tokens when you need higher precision.

## Preventative Techniques

1. You should ensure that any ratios or rates you are using allow for large numerators in fractions.

For example, we used the rate tokensPerEth in our example. It would have been better to use weiPerTokens, which would be a large number. To calculate the corresponding number of tokens we could do msg.sender/weiPerTokens. This would give a more precise result.

2. Order of operations

the calculation to purchase tokens was

```
    msg.value/weiPerEth*tokenPerEth.
```

Notice that the division occurs before the multiplication

This example would have achieved a greater precision if the calculation performed the multiplication first and then the division; i.e.,

```
msg.value*tokenPerEth/weiPerEth
```

3\. convert values to higher precision -> perform all mathematical operations -> convert back down to the precision required for output

Typically uint256s are used (as they are optimal for gas usage). These give approximately 60 orders of magnitude in their range, some of which can be dedicated to the precision of mathematical operations.

It may be the case that it is better to keep all variables in high precision and convert back to lower precisions in external apps

> This is essentially how the decimals variable works in ERC20 token contracts.
