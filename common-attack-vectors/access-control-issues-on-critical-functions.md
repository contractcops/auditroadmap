# Access control issues on critical functions

Access control in smart contracts can be related to governance and critical logic like minting tokens, voting on proposals, withdrawing funds, pausing and upgrading contracts, changing ownership, etc.

The majority of the smart contracts use OpenZeppelin’s Ownership library. Smart contracts can define custom Roles according to their requirements. These roles can be assigned to privileged users, and the Openzeppelin libraries help validate these roles.

So, what are the common vulnerabilities, related to access control?

We can narrow them to these three categories:

## 1.Missed modifier validations

If you see the example below, there is an obvious issue with the setPrice function

![Alt text](<../Common Attack Vectors/image/Access control issues on critical functions/DummyToken\_missedModifier.png>)

If you read carefully, you will observe that if setPrice doesn't have a proper modifier, everyone can set the price of the token, which is not desirable. Instead we should do something like this:

![Alt text](<../Common Attack Vectors/image/Access control issues on critical functions/DummyToken\_fixMissedModifier.png>)

We are adding modifier so that only the owner is able to invoke this function plus we are adding requirement that the price shouldn't be less or equal to zero in the constructor.

## 2.Incorrect Modifier Names

Due to the developer’s mistakes and spelling errors, it may happen that the name of the modifier or function is incorrect than intended.

Attackers may also exploit it to call the critical function without the modifier, which may lead to catastrophic results.

## 3.Overpowered Roles

The Overpowered Roles vulnerability is a type of security issue that can arise in smart contracts when the role-based access control mechanisms are not properly implemented. This can lead to certain users having more permissions and privileges than intended, which can result in unintended consequences.

HospoWise Hack — A case scenario

Recently Hospowise was hacked because of an access control issue that allowed anyone to burn the Hospo tokens.

```
function burn(address account, uint256 amount) public {
    _burn(account, amount);
}
```

The burn function is public and hence any user can call the burn function.

Now an attacker could purchase any token and then call the public burn function to burn all the hospo tokens on UniSwap, creating inflation and hence increasing the worth of the token and then swapping it for ETH till the pool is exhausted.

This could have been prevented if the function had access control implemented like onlyOwner or the function was internal with correct access control logic.
