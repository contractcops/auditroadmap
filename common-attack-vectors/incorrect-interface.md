# Incorrect interface

A contract interface defines functions with a different type signature than the implementation, causing two different method id's to be created. As a result, when the interface is called, the fallback method will be executed.

## Vulnerability

The vulnerability here is that the interface could be incorrectly defined.

let's see an example:

```
Alice.set(uint) 
```

takes an uint in Bob.sol but

```
Alice.set(int) 
```

a int in Alice.sol.

The two interfaces will produce two differents method IDs. As a result, Bob will call the fallback function of Alice rather than the set function of Alice.

![Alt text](<../Common Attack Vectors/image/Incorrect interface/improperInteracePicture.png>)
