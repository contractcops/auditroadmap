# Smart contract gas griefing

> Gas griefing is when a user sends just enough gas to execute a given function but not the sub-calls of that function

> This can seriously impact the business logic, as the function that executes may require the other logic to also be performed in order to keep the contract properly operational

The worst case scenario, a low level call will be done to some external contract (with important business logic) that will be left unchecked. This way, the transaction will continue and the contract would have had executed already, making it a malicious/potentially dangerous attack.
