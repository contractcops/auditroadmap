# Message call with hardcoded gas amount

The

```
transfer() and send() functions 
```

forward a fixed amount of 2300 gas.

Historically, it has often been recommended to use these functions for value transfers to guard against reentrancy attacks.

However, the gas cost of EVM instructions may change significantly during hard forks which may break already deployed contract systems that make fixed assumptions about gas costs.

For example the EIP 1884 broke several existing smart contracts due to a cost increase in the SLOAD instruction.

Also:

* ChainSecurity - Ethereum Istanbul Hardfork: The Security Perspective
* Steve Marx - Stop Using Solidity’s transfer() Now

## Prevention

Avoid using transfer and send. Do not otherwise specify a fixed amount of gas when performing calls. Instead use

```
.call.value(...)("")
    
```

Use the checks-effects-interactions pattern and/or reentrancy locks to prevent reentrancy attacks.
