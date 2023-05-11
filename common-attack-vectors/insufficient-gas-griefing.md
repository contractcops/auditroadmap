# Insufficient Gas Griefing

Insufficient gas griefing attacks can be performed on contracts that accept data and use it in a sub-call on another contract.

If the sub-call fails, either the whole transaction is reverted, or execution is continued.

In most cases, this results in uncontrolled behavior that could have a dangerous impact on the business logic.

> In the case of a relayer contract, the user who executes the transaction, the 'forwarder', can effectively censor transactions by using just enough gas to execute the transaction, but not enough for the sub-call to succeed.

## Vulnerability

This vulnerability exists in smart contracts that either do not check the required gas to execute a sub call, or does not check the returned value of the call itself.

Let's see an example:

![Alt text](<../Common Attack Vectors/image/Insufficient Gas Griefing/insufficientGasFirstExample.png>)

Let’s suppose that the “Target” contract is the contract that the “Relayer” contract communicates with.

The relayer contract is the vulnerable contract. The vulnerability is one the line

```
(bool success, ) = address(target).call(abi.encodeWithSignature("execute()", _data));
```

There is no check of the remaining gas nor the result of the call, the function continues its execution without any problem.

This behavior can cause a serious impact on the application logic.

If you compile those two contracts and try to execute the relay function normally without increasing the gas limit asnd then you check the value of the result variable in the Target smart contract, you will notice that its value didn’t change.

Due to insufficient gas, the contract Target didn’t finish its execution correctly.

You can increase the gas limit and observe that the result variable value will be changed.

## How to detect gas griefing vulnerability

Firstly, understand the business logic of the smart contract(s). Then start looking at operations that perform external communication without result verification.

Each time a contract sends, receive or even calls a smart contract function, there is a risk to find this vulnerability.

## Prevention

Prevention of this vulnerability is one of the most difficult tasks.

Your first approach whould be to require the user to specify the gas to be sent to the Target contract. And if the gas is less than the submitted value, the transaction will revert.

Let's see an example:

![Alt text](<../Common Attack Vectors/image/Insufficient Gas Griefing/insufficientGasSWCExample.png>)

But the problem is that the relayer may not revert as well if no gas was left for the execution of the Target contract.

In addition, the user may also forget or maliciously specify a \_gasLimit that is enough to execute the Relayer contract but not to call the Target contract.

So there is a risk with this approach, let's find another solution:

![Alt text](<../Common Attack Vectors/image/Insufficient Gas Griefing/gasGriefingFixing.png>)

```
estimatedGasValue
```

this variable represent the amount of gas required by the call() function to correctly execute the Target smart contract function.

```
gasNeededBetweenCalls 
```

this is the estimated gas cost

```
gasAvailable/64
```

this represents the amount of gas that will remain in the contract after calling the Target function. The 64 number comes from the fact that by default the smart contract leaves 1/64 of the amount of gas sent to the callee smart contract to allow the caller smart contract to finish its execution after that call finishes.

The steps are the following:

1. firstly the contract calculates the amount of gas that will be available for the contract while reaching the call() function
2. Then it checks if the amount of gas after deducing that value from the gas sent by the user will remain higher than the estimatedGasValue for the call()

In general, the idea here is that the developer is the one who precise how much gas should be sent and not the user.

NOTE: This solution is not perfect, as if the gas cost gets higher with time, the solution may fail. The best way to deal with this problem is by giving the EVM the ability to calculate the required gas for each call and revert the transaction if it is not sufficient.
