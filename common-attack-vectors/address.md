# Common attacks with contract/EOA addresses

#### address.send(amount) - contractaddress.send(amount) returns a boolean, false on error. This should always be checked and handled appropriately.

#### address.call(payload) - contractLow level CALL function - it can construct a message call with a data payload. Returns a boolean, false on error. This function is unsafe: recipient can accidentally or maliciously use up all of your gas, causing your contract to halt with OOG exception. Always check the result of this function.address.call("function\_name", parameters)❗ This potentially be exposed to reentrancy attacks

#### callcode() is deprecated and shouldn't be used
