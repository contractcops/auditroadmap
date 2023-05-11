# Denial of Service (DoS)

Although this category is very broad, it consists of attacks that where users can render a contract inoperable for a period of time, or in some cases permanently. This can trap ether in these contracts forever.

There are three types of DoS attacks in smart contracts:

1. Looping through externally manipulated mappings or arrays
2. Owner operations
3. Progressing state based on external calls

For example:

## Looping through externally manipulated mappings or arrays

This pattern typically appears when an owner wishes to distribute tokens to investors with a distribute-like function, as in this example contract:

![Alt text](<../Common Attack Vectors/image/Denial of Service(DoS)/DistributeTokens.png>)

Notice that the loop in this contract runs over an array that can be artificially inflated. An attacker can create many user accounts, making the investor array large. In principle this can be done such that the gas required to execute the for loop exceeds the block gas limit, essentially making the distribute function inoperable.

## Owner operations

Another common pattern is where owners have specific privileges in contracts and must perform some task in order for the contract to proceed to the next state.

One example whould be an ICO contract that requires the owner to finalize the contract, which then allows tokens to be transferable. For example:

![Alt text](<../Common Attack Vectors/image/Denial of Service(DoS)/DoS\_Finalize.png>)

In such cases, if the privileged user loses their private keys or becomes inactive, the entire token contract becomes inoperable. In this case, if the owner cannot call finalize no tokens can be transferred, the entire operation of the token ecosystem hinges on a single address.

## Progressing state based on external calls

Contracts are sometimes written such that progressing to a new state requires sending ether to an address, or waiting for some input from an external source. These patterns can lead to DoS attacks when the external call fails or is prevented for external reasons.

In the example of sending ether, a user can create a contract that does not accept ether.

> If a contract requires ether to be withdrawn in order to progress to a new state, the contract will never achieve the new state, as ether can never be sent to the user’s contract that does not accept ether.

## Preventative Techniques

1. In the first example, contracts should not loop through data structures that can be artificially manipulated by external users. A withdrawal pattern is recommended, whereby each of the investors call a withdraw function to claim tokens independently.
2.  In the second example, one solution is to make the owner a multisignature wallet. Another solution is to use time-lock such that

    ```
      require(msg.sender == owner || now > unlockTime)
    ```

That allows any user to finalize after a period of time specified by unlockTime.

This kind of mitigation technique can be used in the third example also.

> Of course, there are centralized alternatives to these suggestions: one can add a maintenanceUser who can come along and fix problems with DoS-based attack vectors if need be. Typically these kinds of contracts have trust issues, because of the power of such an entity.
