# Assert require revert

> assert(), require() and revert() all behave very similarly, but should be use at different circumstances

Take a look at the following examples of how each one is used

> require(msg.sender == owner);
>
> assert(msg.sender != owner);
>
> if (msg.sender != owner) {revert()}

As we can see, they are all used in a different way and serve different semantic purposes.

* Require is used when some user input or condition must be present at the time of execution of the transaction
* Assert should be used for when a condition should NOT be true. This is usually to check state variables for unexpected and indesirable changes
* revert() is similar to require() in a sense that when combined with a check, it reverts the transaction, mitigating any changes that had been done to state variables.

### Where does the issue come into play?Usually, when an assert fails, this indicates that there is a vulnerability with the state variables (usually connected with a security issue or a code mistake)Sometimes, require() statements can be too strong, in the sense that the condition can't be passed, making the function virtually useless.
