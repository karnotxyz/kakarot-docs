---
title: Kakarot zkEVM 在幕后运行
sidebar_position: 2
---

## 在Cairo建造的zkEVM Kakarot

Kakarot 是一个在 [Cairo](https://www.cairo-lang.org/) 中构建的 zkEVM，这是一个可证明的语言，为 [Starknet](https://starkware.co/starknet/) 和所有 StarknetOS 链（也称为 CairoVM 链或 Starknet 应用链）提供支持。Kakarot 是一个与以太坊兼容的 Layer 2，一个所谓的 [zkRollup](https://ethereum.org/developers/docs/scaling/zk-rollups)。除了兼容性外，Kakarot 还致力于向 L2 领域推动更多创新，并添加新功能，如原生账户抽象到 EVM。Kakarot 的主导理念是“证明、扩展和创新” 🥕。


具体而言，Kakarot 是一个与以太坊兼容的 zk-Rollup：

- 与以太坊兼容：以与以太坊相同的方式使用 Kakarot 和以太坊。
- 零知识（zk）：不妥协于安全性，通过数学实现最大化的数据完整性。
- Rollup：享受比以太坊主网更低的成本。

对于用户：

- 对于终端用户，可以像使用以太坊主网一样使用 Kakarot：通过任何 EVM 钱包与 dApp 进行交互，例如 Metamask 或 Rabby。
- 对于开发者和团队，您可以使用以太坊生态系统的标准工具构建 Kakarot：Solidity 或 Vyper、Foundry、Hardhat、Ether.js 等。

在 [survival guide](../survival-guide) 页面上发现 Kakarot 浏览器和其他有用的链接。

注意：Kakarot 不是一个隐私链。零知识技术可以用于两种（非排斥）目的，即扩展或隐私。Kakarot 使用前者来扩展以太坊。


## Kakarot 在底层是如何工作的？

### Kakarot 是在 Cairo 中实现的 EVM

在底层，Kakarot zkEVM 是一个实现了 EVM 指令集的 Cairo 程序。EVM 是蓝图，Kakarot 在 Cairo 中实现了它。

> Cairo 是第一个用于创建可证明通用计算程序的图灵完备语言。
> 通用计算。

Cairo 跟其他任何编程语言都一样，但是专门用于编写可证明的软件。这意味着无论在 Cairo 中写什么，从设计上都是零知识的。使用 Cairo 意味着我们利用了密码学，而无需考虑它，使用这种语言而不是 Rust 等语言，这样的功能“免费”提供了。

---

图表 - Kakarot zkEVM 的高级架构：

![Kakarot zkEVM architecture diagram](../../static/diagrams/kakarot_zkevm.png)

---

Kakarot - 网络 - 由三个部分组成：Cairo 中的核心 EVM、一个 RPC 层（RPC 服务器和 EVM 索引器）和一个底层 CairoVM 客户端（一个 StarknetOS 链）。

### Kakarot 运行在底层的 StarknetOS 客户端上

Kakarot 核心 EVM，即前面提到的我们的新 EVM 实现，部署在一个底层的 StarknetOS 链上。这意味着 Kakarot 作为一组 Cairo 智能合约在 CairoVM 驱动的链上运行。这个 CairoVM 链对用户来说是“不可见”的。用户只能通过以太坊兼容的方式通过 RPC 层与 Kakarot 进行交互。在 Kakarot zkEVM 中唯一暴露的接口是以太坊 JSON-RPC 规范。在未来，我们可以利用这一点，允许开发者编写自己的 Cairo 预编译合约，就像 [Arbitrum Stylus](https://arbitrum.io/stylus) 引入了 Rust、C 和 C++ 一样。


---

图表 - Kakarot RPC 层

![Kakarot RPC Layer](../../static/diagrams/kakarot_rpc.png)

---
Kakarot L2由用Cairo编写的EVM和一个RPC层组成，以允许用户以以太坊格式与之交互。所有Cairo执行跟踪都是可证明的，这使得Kakarot能够批处理区块并使用[Starkware Shared prover](https://starkware.co/tech-stack/)(SHARP)提交证明到L1。由于Cairo是一个充满活力的生态系统，未来将出现其他证明器实现，比如[Stark Platinum Prover](https://github.com/lambdaclass/lambdaworks/tree/main/provers)。这将实现多重证明安全，并增加Kakarot网络的健壮性。

在Kakarot zkEVM中，关于EVM程序及其Cairo等价物的设计选择如下所述。它们可能随着时间的推移而发生架构变化。免责声明：所有这些设计选择对用户都是不可见的：

- 每个EVM智能合约（称为Contract Account）都部署为一个唯一的Starknet智能合约。这个Starknet智能合约存储着自己的字节码和EVM存储槽。
- 每个EVM用户拥有的账户（称为Externally Owned Account (EOA)）都部署为一个Starknet智能合约钱包。
  - 它具有一个Starknet格式的地址（31字节的十六进制字符串），该地址与用户EOA EVM地址（20字节的十六进制字符串）唯一映射。对于用户来说，这是不可见的。
  - 它的ETH（代币与通证）原生余额在Kakarot系统中是以ERC20原生通证的形式计价的。对于用户来说，这是不可见的。
  - 它的行为完全像一个EOA，使用与以太坊主网相同的签名和验证方案，未来可以扩展支持创新功能！
- 用户发送的EVM交易被包装在Starknet交易中。衍生的EVM交易哈希与底层Starknet交易哈希一一对应。由于签名验证是在Cairo程序中完成的，因此交易在RPC层被打包（[despite being wrapped at the RPC level](https://github.com/kkrt-labs/kakarot-rpc/blob/bcadfc9b38ac934f73832b3a3485c15f08d66218/src/eth_rpc/servers/eth_rpc.rs#L236)）时仍然可以被证明地进行处理。对于用户来说，这是不可见的。
- 新的状态根使用Pedersen哈希计算，而不是keccak，因为keccak在零知识友好性方面存在问题。这不会对EVM兼容性产生影响。
- 状态trie使用Pedersen MPT计算，而不是[Keccak MPT](https://ethereum.org/developers/docs/data-structures-and-encoding/patricia-merkle-trie)。请注意，交易trie和收据trie都是作为keccak MPT进行计算的，用于区块浏览器，但作为证明承诺的Pedersen MPT进行计算。

总之，任何在Cairo中编写的东西都是可以证明的。Kakarot在Cairo中实现了EVM规范。这是设计上可证明的。所有Cairo的工作都是在幕后完成的。对于用户来说，这是不可见的。他们正在与一个EVM链进行交互。

Kakarot与其他zkEVM的区别

Kakarot zkEVM可能是最高级的zkEVM。在数学语言和多项式到人类可理解的语言之间的尺度上，Kakarot比其他zkEVM更接近于人类可读的语言。这对用户有两方面的影响：

- 由于Kakarot是建立在Cairo之上的，因此Kakarot作为一个代码库非常精简（比其他zkEVM轻一个数量级），因此非常容易维护、适应以太坊的变化或添加新功能（例如本地账户抽象）。
- Cairo（通过Starknet）是一个充满活力的生态系统，Kakarot可以轻松受益于其所有创新（相同的底层技术栈）。长期的想法可能包括并行执行、seed-less钱包（例如只依赖于面部识别）、Celestia DA集成等。

总之，通过在未来几年押注于CairoVM，Kakarot利用了整个Cairo（因此Starknet）生态系统。Cairo是生产中最先进的高级零知识工具箱，首先是[StarkEx](https://www.theblock.co/post/237064/starkex-layer-2-records-1-trillion-in-on-chain-trading-volume-since-june-2020)，现在是Starknet。


---

图表 - 如何构建 zkEVM：

![Different ways to build a zkEVM: low-level circuits or intermediary zkVM](../../static/diagrams/how_to_build_a_zkevm.png)

我们相信，只专注于工程设计，我们的方法是可扩展和可持续的。

<!-- For information unrelated to documentation effort, link to external URLs to decrease the area to maintain: docs should contain doc-related content, and for other content (e.g. how did Kakarot start, what is the roadmap, etc.), use other media -->
