# Unsafe Ownership Transfer

Transferring ownerhip of your contract, e.g. when transferring governance rights or even changing your proxy admin, it's scary.

If you transfer the rights to the wrong account, you are most likely losing the ownership forever.

## Current state

![Alt text](<../Common Attack Vectors/image/Unsafe Ownership Transfer/ownershipCurrentState.png>)

As you can see, you must own the contract and call transferOwnership, that’s it. The ownership is transferred to the address passed as parameter right away.

## Proposed improvement

Imagine if instead of having a transferOwnership that does the transfer in a single step, we would have a two step process, adding a confirmOwnershipTransfer function.

if you were transferring rights to the wrong account, you will realize it when failing at trying to confirm it with the expected new owner and, if that’s the case, you can call transferOwnership again overriding the new owner candidate (you can even use the current owner as candidate if you want to completely cancel the transfer operation).

Here is how the flow described above could look like:

![Alt text](<../Common Attack Vectors/image/Unsafe Ownership Transfer/ownerhipProposal.png>)

## Going a bit further

If we want extra security, we could even require both the new owner candidate and the current owner to call confirmOwnershipTransfer.

this extra step prevents the mistake of performing a transferOwnership to a wrong address that was active and executed the confirmOwnershipTransfer to accept the erroneous transfer before you override the candidate with a correct one.

Here you have an implementation of the described above:

![Alt text](<../Common Attack Vectors/image/Unsafe Ownership Transfer/ownershipFurther.png>)
