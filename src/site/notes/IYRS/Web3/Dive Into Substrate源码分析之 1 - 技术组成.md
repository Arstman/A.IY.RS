---
{"dg-publish":true,"dg-permalink":"dive-into-substrate-source-code-1-tech-components","permalink":"/dive-into-substrate-source-code-1-tech-components/","tags":["Substrate","Rust","web3"]}
---


如果你是最近才了解Substrate, 那么要恭喜你!

因为Substrate 官网的文档最近做过大的升级, 你可以看到现在的官方文档其实是非常详细的, 比如会有一个专门的章节来介绍区块链的基础知识, 而老版的文档则是直接假定读者已经具备区块链的基础知识了.

而我这一系列虽然不做假定, 但我还是非常推荐各位应该首先去官网看过官方的[教程和文档](https://docs.substrate.io/fundamentals/why-substrate/), 因为我的文章更多是从一个实际开发者的角度来理解的, 如果你也是一个开发者, 或许你更加能与我共鸣.

废话少说, 首先, 我们要理解Substrate, 就必须要理解Substrate的技术构成.

我的本职工作是一个架构师, 架构师的一个重要职责, 就是为团队圈定技术方向, 为项目敲定技术选型. 那么, 就从我的工作方向出发, 假设今天我们要开发一个类似Substrate的区块链框架, 我们应该怎么做?

也就是说, 我们试着从一个开发者的角度, 来扮演一下Gavin Woods的角色, 看看如果你要开发一个区块链框架, 你该如何做技术选型?

Wasm, XCM和NPoS
--------------

要回答这个问题, 我们首先要看下当时的区块链世界有哪些突破, 又有哪些痛点; 同时当时的技术大环境里, 又有哪些新技术可用.
当时(2017~2018)的区块链世界中, 最大的突破莫过于2个, 一是比特币的诞生并发展第一次将区块链这一技术带入世界, 开启了区块链时代, 第二则是以太坊开创的智能合约时代, 正是因为智能合约的引入, 区块链才第一次有向实际应用的可能性迈出了一大步, 开启了智能合约时代.
但是几乎所有的其它区块链都是尽可能地模(chao)仿(xi)比特币或者以太坊, 但无论是比特币还是以太坊都存在几个非常严重的难题:

1.  其底层的共识采用的PoW, 即工作量证明模式, 这一模式最大的问题是对资源的消耗极大, 而这消耗的资源实际上什么也没干, 就是在白白消耗. 这也是基于Pow的挖矿模式被各国打击的主要原因.
2.  升级困难. 再牛逼的程序员都会写出bug, 所以就必定需要升级打补丁; 而区块链由于其特殊性(必须全体保持一致), 升级非常困难, 不仅升级困难, 升级后的新老数据迁移也是非常难搞. 最终只能用分叉(fork)这种方式来完成. Fork方式费时费力, 动辄要数年功夫, 而且还不一定能达成社区共识, 很有可能会造成社区分裂.
3.  生态封闭, 交易承载有限. 这里的生态封闭是相对而言, 其实智能合约的出现, 已经让区块链的生态大大丰富起来, 但一个以太坊的承载容量有限, 当成千上万的智能合约涌入的时候, 基于PoW共识的以太坊很难招架的住; 而且以太坊的生态和其它公链生态无法做到互通, 这就;形成了一个个**区块链孤岛.**

以上就是当时最主要的问题. 要解决这些问题, 有两种方式:

1.  在现有的体系上升级打补丁
2.  开发一种全新的区块链体系.

以太坊选择了第一种, 今天的以太坊2.0, 抛弃了PoW方式, 采取的全新的分片模式, 算是很大程度上解决了以上的难题.
而Gavin当时则选择了第二条路, 开发了一种全新的区块链和区块链开发体系, 这就是Substrate和波卡系的由来.

对于上叙3个问题, Substrate分别使用了3个技术来解决:

1.  对于PoW造成的资源浪费和TPS过低问题, Substrate使用了一种全新的共识引擎, 称之为NPoS(提名权益证明)
2.  对于升级困难的问题, Substrate将一个节点分为Runtime和Client两个部分, 其中Runtime只负责链上状态迁移的计算, Client则只负责网络接连/数据持久化等, 这样最为核心的状态迁移模块就独立出来, 并且Substrate使用Wasm技术来实现这一模块, 这就使得Runtime的升级变得非常轻松, 当Runtime统一升级后, Client的升级就不会对链上升级造成影响了, Substrate将这种无需fork就可以轻松达到升级的方式称之为”Forkless Upgrade”.
3.  对于单链生态封闭, 区块链孤岛的问题, Substrate使用了一种全新的技术, 称之为”跨链”技术, 包含XCM, XCMP等一系列组件, 并采用中继链+平行链的方式来实现跨链互通.

由此我们可以大致了解, Substrate的技术涉及以下几个方面:

1.  Wasm技术, 构成substrate的runtime和智能合约模块
2.  libp2p技术 和KV数据库, 主要构成substrate的client模块
3.  NPoS相关的共识引擎, 包括GRANDPA, BABE等, 这些是构成整个Substrate区块链网络的治理基础

![](https://search.pstatic.net/common/?src=https://i.imgur.com/FNFiw0k.png)

因此, 如果想要深入研究Substrate, 那么对于以上3种技术的了解还是很有必要的. 这其中, Wasm和共识引擎我们将放在不同的章节部分来讲述, libp2p我们则单独用一篇文章来讲解.
