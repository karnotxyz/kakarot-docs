---
sidebar_position: 3
---

# Kakarot 与以太坊的区别

尽管Kakarot兼容以太坊，并旨在支持以太坊的最新功能（例如[Cancun 新操作码](https://blog.ethereum.org/2024/01/10/goerli-dencun-announcement)），但它仍然具有一些特殊情况的行为。它们列举如下：

| Item                   | Ethereum                                                   | Kakarot                                                                                                                                                                               |
| ---------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BLOCKHASH              | Get the hash of one of the 256 most recent complete blocks | The last 10 blocks are not available, and 0 is returned instead                                                                                                                       |
| BLOBBASEFEE            | Get the current data-blob base-fee                         | Return 0 as there are no blobs on Kakarot                                                                                                                                             |
| BLOBHASH               | Get blob versioned hashes at index                         | Return 0 as there are no blobs on Kakarot                                                                                                                                             |
| Block hash computation | keccak256(RLP.encode(Block Header))                        | [pedersen(Starknet Block Header)](https://docs.starknet.io/documentation/architecture_and_concepts/Network_Architecture/header/)                                                      |
| State root computation | Root of state keccak-MPT                                   | [poseidon(root of contract pedersen-MPT, root of contract class pedersen-MPT)](https://docs.starknet.io/documentation/architecture_and_concepts/Network_Architecture/starknet-state/) |
