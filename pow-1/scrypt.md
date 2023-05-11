# Scrypt - RFC 7914

> Scrypt is a hashing algorithm used on certain Proof of Work blockchains. 
>
> It is a KDF (cryptographic key-derivation function). It is memory-intensive, designed to prevent GPU, ASIC and FPGA attacks.

Some of the advantages and applications of Scrypt are:

- Less complex in comparison to other mining algorithms
- Less energy consumption required in comparison to SHA-256 for example.
- Scrypt mining is four times faster than mining bitcoin
- Great for encyption wallets, files and passwords

> Scrypt is an attempt of improving SHA-256

<h4>Scrypt Parameters</h4>

The Scrypt configuration parameters are as follows:

* `N` – iterations count (affects memory and CPU usage), e.g. 16384 or 2048
* `r` – block size (affects memory and CPU usage), e.g. 8
* `p` – parallelism factor (threads to run in parallel - affects the memory, CPU usage), usually 1
* `password`– the input password (8-10 chars minimal length is recommended)
* `salt` – securely-generated random bytes (64 bits minimum, 128 bits recommended)
* `derived-key-length` - how many bytes to generate as output, e.g. 32 bytes (256 bits)

> Scrypt playground: https://8gwifi.org/scrypt.jsp

Scrypt is used within the Litecoin blockchain.
