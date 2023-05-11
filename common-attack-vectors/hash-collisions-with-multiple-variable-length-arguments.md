# Hash Collisions With Multiple Variable Length Arguments

Using abi.encodePacked() with multiple variable length arguments can, in certain situations, lead to a hash collision.

Since abi.encodePacked() packs all elements in order regardless of whether they're part of an array, you can move elements between arrays and, so long as all elements are in the same order, it will return the same encoding.

> In a signature verification situation, an attacker could exploit this by modifying the position of elements in a previous function call to effectively bypass authorization.

Let's observe this contract below:

![Alt text](<../Common Attack Vectors/image/Hash Collisions With Multiple Variable Length Arguments/access-control-encodePacked.png>)

## The Vulnerability

There is a very nasty issue with the line

```
bytes32 hash = keccak256(abi.encodePacked(admins, regularUsers));
```

To put it in perspective, let's see these two lines:

```
bytes32 hash = keccak256(abi.encodePacked([addr1, addr2], [addr3, addr4]));

bytes32 hash = keccak256(abi.encodePacked([addr1, addr2, addr3], [addr4]));
```

The following two statements return the same value, even though the parameters are unique.

Given that different parameters can return the same value, an attacker could exploit this by modifying the position of elements in a previous function call to effectively bypass authorization.

For example if an attacker saw

```
addUser([addr1, addr2], [addr3, <attacker’s address>, addr4], sig)
```

They could call

```
addUser([addr1, addr2, addr3, <attacker’s address>], [addr4], sig)
```

Since the return values are the same, the signature will still match, making the attacker an admin.

> Though the contract should have proper replay protection, an attacker can still bypass this by front-running.

## Prevention

There are a few different remediation's we can take to prevent this vulnerability.

The first option is to not allow arrays as parameters, instead of passing a single value.

![Alt text](<../Common Attack Vectors/image/Hash Collisions With Multiple Variable Length Arguments/access-cointrol-singleValue.png>)

The second option is to have fixed size arrays, so the positions cannot be modified

![Alt text](<../Common Attack Vectors/image/Hash Collisions With Multiple Variable Length Arguments/access-control-fixedSizeArrays.png>)

The third option is to use abi.encode instead of abi.encodePacked

![Alt text](<../Common Attack Vectors/image/Hash Collisions With Multiple Variable Length Arguments/access-control-encode.png>)
