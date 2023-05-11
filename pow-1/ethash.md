# Ethash

> Ethash was Ethereum's POW mining algorithm (Now POS)

Memory hardness is achieved with a proof of work algorithm that requires choosing subsets of a fixed resource dependent on the nonce and block header. This resource (a few gigabytes in size) is called a DAG. The DAG is changed every 30000 blocks, a ~125-hour window called an epoch (roughly 5.2 days) and takes a while to generate. Since the DAG only depends on block height, it can be pre-generated, but if it's not the client needs to wait until the end of this process to produce a block. If clients do not pre-generate and cache DAGs ahead of time the network may experience massive block delay on each epoch transition. Note that the DAG does not need to be generated for verifying the proof-of-work essentially allowing for verification with both low CPU and small memory.

The general route that the algorithm takes is as follows:

1. There exists a **seed** which can be computed for each block by scanning through the block headers up until that point.
2. From the seed, one can compute a  **16 MB pseudorandom cache** . Light clients store the cache.
3. From the cache, we can generate a  **1 GB dataset** , with the property that each item in the dataset depends on only a small number of items from the cache. Full clients and miners store the dataset. The dataset grows linearly with time.
4. Mining involves grabbing random slices of the dataset and hashing them together. Verification can be done with low memory by using the cache to regenerate the specific pieces of the dataset that you need, so you only need to store the cache.
