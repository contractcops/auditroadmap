# Assert Attack

> In Solidity, assert is meant to check invariants.
>
> They are meant to check and assure that certain states shouldn't be changed/violated.

* **An invariant allows you to specify in code (and validate at runtime) that some constraint always holds true automatically at the end of every external call into the contract** .

Taking a good description from a software perspective:

> An invariant is any logical rule that must be obeyed throughout the execution of your program that can be communicated to a human, but not to your compiler.

When an assert(`<condition>`) fails, the states changed during the function execution are discarded and the transaction is reverted.

To clarify the difference:

* The assert function should be used for internal errors and illegal changes in the state
* The require function should be used to validate and check for inputs, conditions with the state variables or checking return values for calls to external contracts
