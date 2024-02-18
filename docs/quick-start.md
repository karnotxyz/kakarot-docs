---
title: 快速开始- 5分钟概述
sidebar_position: 1
---

## Kakarot zkEVM是什么?

Kakarot是用一种可证明的语言[Cairo](https://www.cairo-lang.org/)构建的zkEVM，为[Starknet](https://starkware.co/starknet/)和所有StarknetOS链（也称为CairoVM链或Starknet应用链）提供支持。Kakarot是与以太坊兼容的第2层，即[叫做zkRollup](https://ethereum.org/developers/docs/scaling/zk-rollups)。
除了兼容性之外，Kakarot还努力推动L2领域的更多创新，并为 EVM 添加新功能，如原生账户抽象。Kakarot的驱动精神是**证明、扩大和创新**🥕。

## 如何使用Kakarot zkEVM?

Kakarot zkEVM是一个以太坊兼容的rollup，这意味着作为用户和开发人员，您可以与Kakarot zkEVM进行交互，就像您与以太坊主网或任何其他基于以太坊的链进行交互一样(使用Metamask,Rainbow，用Foundry或Hardhat等构建)。**更改RPC URL，它就能“正常工作”了**。话虽如此，Kakarot仍然处于alpha testnet阶段🚧，仍然可能发生意外错误。通过[discord](https://discord.gg/kakarotzkevm)联系我们，报告错误🐛。

虽然我们的目标是在以太坊和Kakarot zkEVM之间实现零差异，而且我们实际上正在努力通过[官方以太坊基金会的100%测试](https://github.com/ethereum/tests)—— 但目前在这个文档网站的[Kakarot 与以太坊之间的差异](differences)页面中记录了一些小的差异。

### 作为用户，我如何与Kakarot zkEVM进行交互?

前往[生存指南](survival-guide) 部分，查找alpha testnet的有用链接。同样，将钱包中的网络更改为Kakarot，它就会"正常工作"。

示例教程 - 向Metamask添加新网络：打开Metamask扩展。点击"添加网络"。选择"手动添加网络"。然后填写字段：

| Category       | Value                           |
| -------------- | ------------------------------- |
| Network Name   | Kakarot zkEVM                   |
| RPC URL        | https://sepolia-rpc.kakarot.org |
| Chain Id       | 10710711648                     |
| Symbol         | ETH                             |
| Block Explorer | https://sepolia.kakarotscan.org |

🚧警告🚧:链接尚未建立。

### 作为一名开发人员，我如何在Kakarot zkEVM上进行构建?

对于开发人员来说，更改RPC URL，它应该可以“正常工作”。

如果您遇到一些未知的错误或想要讨论新功能，您可以:

- 加入我们的discord并获得支持: https://discord.gg/kakarotzkevm
- 在推特上咨询: https://twitter.com/KakarotZkEvm

## Kakarot zkEVM有什么不同?

Kakarot是使用图灵完备的零知识领域特定语言（zkDSL）Cairo编写的唯一可证明的EVM实现。

在这方面，Kakarot更接近于一个EVM客户端，而不同于其他zkEVM。这使得我们的方法更加灵活。依赖于加密原语（例如circuits）的底层方法是为特定的EVM版本量身定制的。它们更难以维护和适应。例如，由于我们的方法是可适应和可持续的，我们可以从第一天开始支持所有以太坊的硬分叉，从而最大程度地减少了EVM的碎片化。

在未来几年，随着以太坊进行更多的升级，zkEVM需要易于适应以保持可持续性。换句话说，它需要轻松地融合以太坊主网的变化。否则，zkEVM的根本目的将部分丧失：利用zk来改进以太坊，但同时阻止协议的任何其他未来演进，并坚持某个给定的版本。开发人员将不得不注意其Solidity或Vyper编译器的版本。用户需要查阅差异检查表。

要深入了解Kakarot设计，请查看[架构概述](architecture/understanding-zkevm)。
