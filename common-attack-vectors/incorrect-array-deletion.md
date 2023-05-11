# Incorrect array deletion

> Just like javascript, Solidity has push() and pop() functions that either add to an array or remove from it.

Let's imagine the array: `array = [1,2,3,4]`

At index 0, the element upon initialization is 1. This means that if we use the `delete` keyword, it should be 2, right?

`delete array[1]` - This can be a big pitfall, as this operation does nothing more than set the element at the specified index (in this case 1) to 0. If that is the intended result, it would be better for it to be replaced with `array[1] = 0` for clarity.

### Preventative measuresDo not use the delete keyword as means to remove some element from an array. Instead, use the array.pop(`<index>`) function, which just like in javascript, removes the element at the specified index.
