# Loop through long arrays

Keep in mind that executing functions in Ethereum costs gas (money), and transactions have a gas limit by definition (the gas limit of a single block). If for some reason your smart contract uses a very long array, and at some point, you need to iterate through it, you might reach the gas limit making the function unexecutable.

## Prevention

Always try to use mappings and avoid loops when you expect a long list of values.
