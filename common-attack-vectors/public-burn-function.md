# Public Burn Function

On the 2nd of September, 2022, ShadowFi was exploited due to public visibility of the burn function, which allowed any user to burn the tokens.

This created inflation and hence increased the worth of the token.

```
function burn(address account, uint256 _amount) public {
    _transferFrom(account, DEAD, _amount);

    emit burnTokens(account, _amount);
}
```

Steps:

1. The attacker called burn function with amount of almost 10.3M SDF
2. Then, the attacker synced the price of the SDF token in the contract, which inflated the price of the SDF tokens.
3. Then the attacker swapped the SDF token with wBNB at the inflated price. The attacker swapped around 8.4 SDF tokens for 1078 wBNB(approx $301K).

## Prevention

In the code above, we have to add simple modifier like onlyOwner

```
function burn(address account, uint256 _amount) public onlyOwner {
    _transferFrom(account, DEAD, _amount);

    emit burnTokens(account, _amount);
}
```

or by making the function internal with correct access control logic.
