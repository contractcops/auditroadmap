# Account Existence Check for low level calls

The first value of the low-level functions, for example call, delegatecall and staticcall, send and transfer returns true if the account called is non-existent. Account existence must be checked prior to calling if needed.

## Remediation

Check before any low-level call that the address actually exists, for example before the low level call in the callERC20 function you can check that the address is a contract by checking its code size.

Let's see an example:

```
    (bool success, bytes memory data) = _addr.call{value: msg.value, gas: 5000}(
        abi.encodeWithSignature("foo(string,uint256)", "call foo", 123)
    );
```

In the example above, the function foo() is being called on the address \_addr with two parameters. A custom gas amount is also spcified along with some Ether value in the msg.value field to be sent along with the transaction.

> This call is particularly useful in cases where the destination contract’s code is not available to us allowing us to directly make a function call just by knowing the function name without creating an instance of the contract.

## Issues with low-level calls

Low-level call usage has a high chance of erroring out and does not check for call success or code existence. It avoids type checking, function existence check, and argument packing, calling a contract function is generally not advised.

> It is recommended to import the contract’s interface before calling any of its functions.

When an error occurs in a Solidity smart contract, it typically causes a state-reverting exception to be thrown. This means that any changes made to the contract's state during the execution of the function are rolled back and the contract is returned to its previous state.

So let's say for example a contract uses the send call to call another, an out-of-gas exception may occur since the call is restricted to 2300 gas, but it may cost more to execute the receiver’s fallback function, where the call is aimed.

Let's see a real-world example:

![Alt text](<../Common Attack Vectors/image/Account Existence Check for low level calls/notCheckingSendFunction.png>)

It is important to note that the winner.send method does not validate the send function’s return value before setting a Boolean to true to signify that the winner has received their winnings. This flaw can lead to a situation in which the winner isn’t actually paid in ether, but the contract’s state shows that they have.

So what are the best practices: Always check for call success!

```
// bad
someAddress.send(123);
someAddress.call{msg.value: 12, gas: 1233}() // this is dangerous, as it will forward all remaining gas and doesn't check for the result
someAddress.call{msg.value: 100}(bytes4(sha3("deposit()"))); // if deposit fails, the call() will return false and the tx will not be reverted

// good
bool sent = _to.send(msg.value);
require(sent, "Failed to send Ether");
(bool sent, bytes memory data) = _to.call{value: msg.value}("");
require(sent, "Failed to send Ether");
```

## How to figure out if an account exists or not?

The assembly language that all Ethereum contracts compile down to contains an opcode for this precise operation: EXTCODESIZE

![Alt text](<../Common Attack Vectors/image/Account Existence Check for low level calls/isContract\_check if contract exists.png>)

The EXTCODESIZE opcode returns the size of the code on an address. If the size is larger than zero, the address is a contract.

But you need to write assembly code within the contract to access this opcode since the Solidity compiler does not support it directly at the moment. The above code creates a private method that you can call from within your contract to check if another address contains code. If you don't want a private method, remove the private keyword from the function header.

IMPORTANT NOTE:

> EXTCODESIZE returns 0 if it is called from the constructor of a contract. So if you are using this in a security sensitive setting, you would have to consider if this is a problem.

Actually there is a vulnerability related with EXTCODESIZE, which you can find in our repo in Common Attack Vectors.
