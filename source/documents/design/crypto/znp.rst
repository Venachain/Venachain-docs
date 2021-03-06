.. _zero-knowledge:

=============
零知识证明
=============

零知识证明这个概念最早由Goldwasser、Micali和Rackoff提出的，其表达核心思想是证明者要向验证者证明一个statement的正确性，与此同时不泄露任何额外的信息。它具有如下三个重要的性质: 

-  完备性completeness

-  合理性soundness

-  零知识性zero-knowledge

近几年零知识证明被广泛应用到区块链中，如可验证的外包计算、匿名证书、范围证明、隐私密码学货币等需要平衡隐私性和机密性的应用场景，它已经在隐私性和可扩展性方面成为了一个非常重要的工具。在实际应用中客户端下载和验证交易频繁，因此要部署高效实用的零知识证明协议，需要该协议的证明足够小，验证足够高效（small proof size and fast verification）。随着密码学技术的不断发展，零知识证明的落地应用层出不穷。这些优秀的项目对零知识证明的技术的实用性进行了一系列的探索: 

-  **基于UTXO模型的零知识证明协议** ：零知识证明在密码学货币上具有广泛的应用，Zcash是zk-SNARKs的首个应用，它利用Groth16协议，实现了基于UTXO模型下交易双方地址和金额的完全隐藏，并且能够生成较短的高效的可验证的证明。但是该技术需要采用CRS来构造zk-SNARK，即需要引入可信第三方来生成一组公共参考串，与此同时也引入了对可信第三方的信任问题，尽管可以利用MPC等技术实现分布式协作生成CRS，但是依然无法完全解决CRS的引入带来的信任问题。

-  **基于账户模型的零知识证明协议** ：当前的区块链隐私支付系统，如Zcash和Monero等，均是基于UTXO模型，而Zether协议能实现基于账户模型的隐私支付，且能实现交易双方和交易金额的匿名化，该协议以智能合约的形式，方便地部署到基于账户模型的区块链系统中，而不需要修改底层链的逻辑。此外AZTEC协议采用Plonk零知识证明协议，实现了基于Ethereum账户模型下的隐私交易。

-  **去CRS的零知识证明协议** ：目前在区块链领域中，大部分高效可验证的零知识证明协议都依赖于CRS。因此去CRS的零知识证明协议也是各个项目重点关注的话题。

-  **CRS的可更新的零知识证明协议** ：基于CRS的零知识证明协议会引入第三方信任问题，完全去CRS的零知识证明协议产生的证明较大，验证不够高效，因此有项目尝试探索介于两者直接的一种零知识证明协议，即基于可更新的结构化参考串（updatable structured reference string）的协议，如Sonic、Plonk它们支持SRS的可更新操作，这两个协议本质上仍然是需要可信的参数设置，但是通过MPC等技术一定程度上提高了用户对CRS安全性的信心。目前AZTEC利用Plonk协议实现了Ethereum上的隐私交易。

-  **可扩展性的零知识证明协议** ：zk Rollup是一种新型的Layer2扩容方案，将链上的数据放到layer2解决。用户发送的交易，由relayer收集，生成零知识证明将发布交易后的新状态跟之前的状态捆绑在一起，保证用户状态变更的正确性。链上只存储用户状态的merkle树根，通过智能合约验证零知识证明的正确性。

-  **基于具体应用需求的零知识证明协议** ：在实际中还有许多基于具体应用需求，并利用上述提及的零知识证明技术设计出的协议，如在存储场景下，filecoin的时空证明（proof of spacetime）和复制证明（proof of replication）是利用zk-SNARKs的Succinct特性的典型案例；在公平交易的场景下，zkPoD实现零信任的去中心化公平交易系统，在不可信双方之间进行交易，确保买卖双方间交易的公平性。

目前零知识证明技术在实际应用中非常成熟，在我们的联盟链中，我们会根据实际的场景需求，采用或设计对应的零知识证明协议来满足我们的场景需求。
