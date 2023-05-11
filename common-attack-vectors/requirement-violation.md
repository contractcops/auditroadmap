# Requirement Violation

The require() construct is meant to validate external inputs of a function.

In most cases, such external inputs are provided by callers, but they may also be returned by callees.

Violations of a requirement can indicate one of two possible issues:

1. A bug exists in the contract that provided the external input.
2. The condition used to express the requirement is too strong.

Let's look at this example:

![Alt text](<../Common Attack Vectors/image/Requirement Violation/requireUsage.png>)

The first time is to check the sender’s balance and ensure that it is above the required balance.

The second time we are making sure that the user cannot send money to himself.

In both of these “require” calls, we are using the second parameter to show the error to the user in case the ‘require’ returns a false value.
