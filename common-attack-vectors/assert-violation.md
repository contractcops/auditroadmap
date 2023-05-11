# Assert Violation

The Solidity assert() function is meant to assert invariants. Properly functioning code should never reach a failing assert statement.

However if you reach assertion, this means one of two things:

1. A bug exists in the contract that allows it to enter an invalid state.
2. The assert statement is used incorrectly, e.g. to validate inputs.

## Prevention

I.Consider whether the condition checked in the assert() is actually an invariant. If not, replace the assert() statement with a require() statement.

II.If the exception is indeed caused by unexpected behaviour of the code, fix the underlying bug(s) that allow the assertion to be violated.

Asserts are here to check states of your Smart Contract that should never be violated. For example: a balance can only get bigger if we add values or get smaller if we reduce values.

Here is an example with a simple contract:

![Alt text](<../Common Attack Vectors/image/Assert violation/assert\_implementation.png>)
