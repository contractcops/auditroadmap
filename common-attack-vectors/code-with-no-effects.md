# Code With No Effects

In Solidity, it's possible to write code that does not produce the intended effects.

Let's see an example:

![Alt text](<../Common Attack Vectors/image/Code With No Effects/depositBox\_no-effect-code.png>)

The line

```
    balance[msg.sender] == amount;
```

has an error which is very unpleasant. Instead the right way is to assign the amount to the balance of the msg.sender, like this:

![Alt text](<../Common Attack Vectors/image/Code With No Effects/depositBox\_no-effect-code-fixed.png>)

Another bad example is this code snippet:

![Alt text](<../Common Attack Vectors/image/Code With No Effects/wallet\_no-effect-code.png>)

As you can see, here we are not checking the return value of the call on this line:

```
    msg.sender.call.value(amount);
```

And the fix is the following:

![Alt text](<../Common Attack Vectors/image/Code With No Effects/wallet\_no-effect-code-fixed.png>)
