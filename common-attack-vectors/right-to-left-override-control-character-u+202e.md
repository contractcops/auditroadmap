# Right-To-Left-Override control character (U+202E)

What is RTLO?

RIGHT TO LEFT OVERRIDE is a Unicode mainly used for the writing and the reading of Arabic or Hebrew text.

The properties of the character are quite unique, in the sense that the operations are switched and instead of going left to right like it usually would, they are switched.

> The right to left override attack is when U+202E character is secretly used to maliciously change the context of the code being executed.

This vulnerability is used to disguise the names of files and can be attached to the carrier like email.

> For example, the file name with ThisIsRTLOfileexe.doc is actually ThisIsRTLOfiledoc.exe, which is an executable file with a U+202e placed just before “doc.”

Let's see an example

```
// SPDX-License-Identifier: MIT
pragma solidity 0.7.5;

contract Calculator {

function add(uint256 x, uint256 y) public pure returns (uint256) {
        /* start function*/
        require(x + y >= x);
        return /*first number*/x + y/*second number*/
        /* end function*/;
    }

function sub(uint256 x, uint256 y) public pure returns (uint256) {
        /* start function*/
        require(y <= x);
        return /*bigger number*/x - y/*samller number*/
        /* end function*/;
    }

function subRTL(uint256 x, uint256 y) public pure returns (uint256) {
        /* start function*/
        require(y <= x);
        return /*bigger number‮/*rebmun rellams*/y - x/*
        ‭/*end function */;
    }
}
```

We see the sub and subRTL functions having the same logic of code. This just seems like the redundant logic of coding, right? But it is not just like that!

if you copy the Calculator smart contract code (Calculator.sol) to your favorite IDE or code editor, you will see something wrong with thesubRTL function.

![Alt text](<../Common Attack Vectors/image/Right-To-Left-Override control character (U+202E)/remixCharactersCheck.png>)

The \[U+202E] is a Right-To-Left Override character that can be used to force writings in the direction of text or even in the smart contract code. Also, the \[U+202D] is a Left-To-Right Override character that is used to force the direction of the code back to normal. The subRTL is a subtraction logic reversion.

## Solutions and notes

* This vulnerability can be detected by the solidity compiler 0.7.6 or above
* Malicious actors can use the Right-To-Left-Override Unicode character to force RTL text rendering and confuse users as the real intent of a contract
* Reading code from Pastebin or block explorer without trying to copy and paste it to IDE or Code Editor which cannot guarantee the securement of smart contracts.
