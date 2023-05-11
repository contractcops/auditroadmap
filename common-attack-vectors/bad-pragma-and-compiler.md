# Bad pragma and compiler

> When possible, contracts should always be up to date with the latest compiler version. This can be inspected with the pragma at the top part of each contract.

Some compiler versions are known to have issues or have deprecated OPCODES. Deploying such a contract will only lead to failure, as sooner or later a certain OPCODE will be obsolete and get removed, making the contracts useless.

Contracts should always consider the newest versions.
