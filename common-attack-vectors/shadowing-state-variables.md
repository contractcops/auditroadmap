# Shadowing State Variables

Solidity allows for ambiguous naming of state variables when inheritance is used.

> In a nutshell, the vulnerability occurs when the same state variable is declared in two places in contracts.

Example:

Contract A with a variable x could inherit contract B that also has a state variable x defined.

This is certainly a problem.

This would result in two separate versions of x, one of them being accessed from contract A and the other one from contract B. In more complex contract systems this condition could go unnoticed and subsequently lead to security issues.

Shadowing state variables can also occur within a single contract when there are multiple definitions on the contract and function level.

## Vulnerability examples

![Alt text](<../Common Attack Vectors/image/Shadowing State Variables/shadowingInFuncitons.png>)

![Alt text](<../Common Attack Vectors/image/Shadowing State Variables/shadowingInInheritance.png>)

## Prevention

Review storage variable layouts for your contract systems carefully and remove any ambiguities. Always check for compiler warnings as they can flag the issue within a single contract.
