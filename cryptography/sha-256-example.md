# Sha-256 Example

> We have the input string: "TimeIsMoney"

* We first convert it into bits using the ASCII table

> 01010100 01101001 01101101 01100101 01001001 01110011 01001101 01101111 01101110 01100101 01111001

This results in a 11 x 8 bits (88 bits)

> The input must be a multiple of 512, meaining pre-processing is necessary
>
> 88 bits round up to the closest multiple of 512
>
> 88 -> 512

* NOTE: If the input were, for example, 960 bits, it would get rounded up to 1024 (2 x 512)

#### PreprocessingFirstly, 1 bit is added, making our total bits to 89We then keep adding 0's until our input is 64 bits away from the nearest multiple of 512 (448)ORIGINAL INPUT: 01010100 01101001 01101101 01100101 01001001 01110011 01001101 01101111 01101110 01100101 01111001We add 1 bit and keep adding 0's until the input is 448 bits:01010100 01101001 01101101 01100101 01001001 01110011 01001101 01101111 01101110 01100101 011110011 ..................................................00000000The remaining 64 bits represent the length of the input (original)Our input was initially 88 bits, 88 written in binary is 1011000We keep adding 0's (57 to be exact) and then add our length at the end

#### SplittingWe now have to split up the input into 512 bit blocks. Since our input string results in 512 bits exactly, we only have 1 block. (N = 1)We then express this 512 bit block into 16 x 32 bitsM0: 32 bitsM2: 32 bits.......M15: 32 bitsWe then set the intial hash values. Those are 8x64 bit words in hex. In sha-256, those are: (256 bits total)H(0)0 = 6a09e667H(0)1 = bb67ae85H(0)2 = 3c6ef372H(0)3 = a54ff53aH(0)4 = 510e527fH(0)5 = 9b05688cH(0)6 = 1f83d9abH(0)7 = 5be0cd19Where do those words come from?
