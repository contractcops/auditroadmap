# Not enough gas for ether transfer

> In Solidity, most best practices show that when sending ether from one address to another, `transfer()` is the best option to use at it is capped at 2300 gas and helps prevent re-entrancy

* What isn't taken into account is that OPCODES change during hard forks, which means more gas might be required to send funds in the future.

Instead, call() should be used which allows a custom amount of gas to be specified

```solidity
(bool success, ) = msg.sender.call.value(amount)("");
```

To prevent re-entrancy, a `public` lock should be used (check RO Reentrancy to read on why it should be public).
