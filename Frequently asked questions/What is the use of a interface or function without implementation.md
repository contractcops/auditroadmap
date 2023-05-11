# What is the use of a interface or function without implementation?

Let's observe the example below:

![Alt text](<image/What is the use of a interface or function without implementation/withInterfaceExample.png>)

And now see the next example, you may wonder: "What is the difference between first and the second code, why we are using interface and is it actually needed?"

![Alt text](<image/What is the use of a interface or function without implementation/withoutInterfaceExample.png>)

So the answer is multi-layered:

1. To economize on bytecode size in calling contracts.
2. To establish standards and ensure compliance.
3. To detect developer errors and oversights.

Let's see them one by one

## To economize on bytecode size in calling contracts

Consider contract Client that will interact with Test(from the examples above). One can create an instance of Test in the client, like this:

![Alt text](<image/What is the use of a interface or function without implementation/instanceOfTestContract.png>)

This will work but Client will have bytecode size of Test size plus Client size because the compiler will roll up the complete Test implementation into Client.

> Client doesn't need all of that code. It only needs the interface for the functions it will call.

It can achieve the same result with a smaller bytecode if uses the interface contract instead - Calculator(as the example above).

![Alt text](<image/What is the use of a interface or function without implementation/CalculatorInterface.png>)

As an aside, you can cast the input (will still be an address) as the contract Type:

```
constructor(Calculator calculator) {
    c = calculator;
}
```

## To establish standards and ensure compliance

Many standards are defined as function interfaces and specifications of precise behaviour.

For example, ERC20 is an interface but without particular implementation as long as the functions work a certain way - that can be confirmed with a test suite. It is common to see something like:

```
contract MyToken is IERC20 {
    ...
}
```

where IERC20 is the defined interface for an ERC20 contract.

If MyToken is deployable, then that establishes that there is an implementation for each function required for the IERC20 interface. This is related to ...

## To detect developer errors and oversights

Inconsistency can creep into intricate systems in the development stage. It can also be cognitively heavy for reviewers to detect inconsistency. Interfaces can function as a sort of error-detection.

Suppose someone decides that

```
getResult() 
```

needs an argument, e.g.

```
getResult(address user) .... 
```

That's a non-trivial design change that will affect a lot of things. Since

```
contract Test is Calculator {...}
```

Test will not deploy unless the interface is updated.

## Why/How?

Function signatures are the first 4 bytes of a hash of the function name and arguments, so getResult() and getResult(uint) are completely different functions at the bytecode level.

In practice, that means Test will compile, but it won't deploy because it will not define one of the mandatory functions defined in the inherited interface.

That is exactly what we want because either:

1. The interface has been redefined, meaning all clients need to adjust
2. The implementation is incorrect, meaning adding that argument is unacceptable
3. There is a missing implementation and the one provided is in addition to the mandatory functions, but not a replacement (both are needed).

For style and consistency, it can be a good habit to define interfaces for implementations on a 1:1 basis.
