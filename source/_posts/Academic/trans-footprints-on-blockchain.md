---
title: '译文 Footprints on the Blockchain...'
tags:
  - Academic
  - 区块链
  - 信息泄露
categories: Academic
date: 2017-03-16 20:53:35
updated:
comments:
permalink:
---

# 摘要

>This paper examines information leakage in distributed ledgers. We show how the lack of time priority in the period between the publication of a transaction and its validation by miners or designated participants can expose a transaction’s “footprint” to the market, resulting in potential front-running and manipulation. We propose a cryptology-based approach for solving information leakage problems in distributed ledgers that relies on using a “hash” (or mathematical identifier) to secure time-priority followed by a second communication revealing more features of the underlying market transaction. Solving the information leakage problem greatly expands the potential applications of private distributed ledger technology.

这篇文章研究了分布式账本技术中的信息泄露问题。我们演示了在交易的公布到比特币矿机或者特定参与者证实交易有效这段时间中，缺乏时序是怎样揭示出交易走向市场的“脚印”，从而导致潜在的非法预先交易和操作。为了解决分布式账本技术中信息泄露问题，我们提出了一个基于密码学的方法。该方法通过“哈希”（或者其他数学标识符）和紧随其后的可以显示更多潜在市场交易信息的二次通讯信息来确保交易时序。信息泄露问题的解决可以增加个人分布式账本技术的潜在应用。

<!-- more -->

# 引言

>Advocates of the blockchain, the distributed ledger technology that underlies Bitcoin,see an almost limitless future for the innovation. Apart from its role in cryptocurrencies,distributed ledger technologies are being developed to handle stock trades, clear and settle leveraged loans, handle catastrophe reinsurance, keep track of land titles and transactions,track the ownership of intellectual properties, facilitate peer-to-peer marketplaces – the list goes on and on. Much of the appeal of the blockchain lies in its use of a decentralized distributed ledger technology in which an immutable single record emerges of the various transactions encased in the blocks. The ability to trade assets in such a setting, along with the emergence of new “smart contracts” that can automatically trigger additional steps such as recording ownership changes or authorizing payments, can have, in the words of the Bank of England, “far-reaching implications for the financial industry”.

用于比特币中的分布式账本技术——区块链技术的先驱者，见证了它在技术革命中的广泛应用前景。除了在加密货币中的应用，分布式账本技术还广泛应用于股票交易， 解决杠杆贷款问题，处理巨灾再保险，跟踪解决土地所有权相关事务，知识产权的所有权鉴定，促进P2P市场发展等等。区块链的吸引力很大程度上来自于其使用分散的分布式账本技术，该技术通过不可变单记录反映区块中各种可变的事务。分布式账本技术处理资产交易的能力，连同可以自动触发额外步骤如记录交易变化的所有者或者授权支付的“智慧合同”的出现，将对金融业产生深远的影响。

>While not disputing this rosy outlook, we argue in this paper that this new technology comes with some very old problems. In particular, a fundamental problem facing traders in traditional market settings is how to hide their trades and trading intentions from other traders. Disguising a trade’s “footprint” is necessary to keep others from copying, or even worse, “front-running” your trades, thereby extracting rents that should have been yours. A variety of algorithmic and technological solutions are dedicated to this task in current markets. As we discuss here, the same information leakage can take place in a distributed ledger setting. This problem arises because even though the blocks in the blockchain are time-stamped, there is no actual time priority in the process of getting a transaction validated and onto the distributed ledger. One contribution of our paper is to show how information leakage can arise in the interim period between the publication of such a transaction and its validation by miners or designated participants, exposing participants in a distributed ledger to potential front-running and manipulation.

首先，本文主要探讨伴随这项新技术产生的一些古老的问题，而不是争论其乐观的前景。比如，在传统贸易市场中，交易者面对的一个基本问题就是怎样隐藏他们自己的交易和交易倾向。伪装交易的“足迹”是为了保障交易不被他人复制和不被他人非法预先交易而获取了本属于自己的利益。在目前的市场中，已经有许多技术解决方案应用于解决这个问题。我们要在这里讨论的是，类似的信息泄露问题同样发生在分布式账本技术中，即使区块链有时间戳，这个问题仍然越发频繁地发生，在分布式账本技术中验证一个事务有效是没有真正的时间有序的。我们的文章的贡献之一就是展示了信息泄露是怎样在类似事务的公布和其被矿机或者指定参与者验证这段时间升温的，从而导致这些参与者走向潜在的非法预先交易和操作。

>A second, and perhaps more important, contribution of this paper is to suggest a solution. We propose a cryptology-based approach for solving information leakage problems in  distributed ledgers. Our two-part process uses a “hash” (or mathematical identifier) to secure time-priority which is then followed by a second communication revealing more features of the  underlying market transaction. Because the initial hash does not reveal details of the trade, the ability to front-run a transaction before it is stored in the distributed ledger is curtailed. This solution essentially removes the ability to manipulate the order of transactions and so restores time priority to the validation process.

其次，这篇文章更加重要的贡献在于我们提出了一个基于密码学的方法来解决分布式账本技术中的信息泄露问题。我们的两步走程序通过“哈希”（或其他数学标识符）来确保时序，紧随该时序的二次交流可以显示出市场交易的更多潜在的信息。因为原始的哈希不会显示交易的细节，所以可以减少在交易存储前进行非法预先交易。这个方案基本上消除了操纵交易顺序和恢复验证程序的时序的可能性。

>Solving the information leakage problem in distributed ledgers greatly expands the feasibility of distributed ledger applications. To date, proposed uses of distributed ledgers are focused mainly on clearing and settlement where there is little ability to profit from any information leakage. In market settings, however, where information leakage is important, our analysis shows how distributed ledgers could be used to operate decentralized markets that safeguard participants’ information. As perhaps befits a new technology, we argue that new cryptography-based tools can provide a solution to an old problem in a new setting.

解决分布式账本技术中的信息泄露问题可以极大地增加分布式账本技术的应用。至今为止，分布式账本技术的应用主要集中于解决哪里有信息泄露的可能，然而，在市场中，哪里有信息泄露很重要，我们的分析策略演示了怎样利用分布式账本技术操作分散市场来保护参与者的信息。也许这更适合于一项新的技术，但是我们认为一项新的基于密码学的工具应该是可以解决新环境中的老问题的。

>2. Information Leakage in Distributed Ledgers

# 2. 分布式账本技术中的信息泄露

>To understand why information leakage can occur, it is useful to first set out the distinction between centralized and decentralized systems, and how their differences affect transactions processing. Centralized systems (for example, exchanges) are straightforward: they have a single party, usually in a single location, processing transactions and assigning timestamps to them. Decentralized systems (for example, the Bitcoin blockchain) are much more complex: there is no single party, and instead it relies on a network of independent parties geographically dispersed executing an algorithm to process transactions. The most relevant part of this process for the focus of our paper is the sequencing of transactions, where each transaction is assigned a timestamp and an order relative to every other transaction. As we will discuss, it is the limitations of this sequencing process that sets the stage for information leakage to occur.

为了理解信息泄露为什么会发生,首先有必要了解集中式系统和分散式系统的区别，以及他们对交易产生对不同影响。集中式系统（例如外币兑换）是明确的：通常有一个单方面的机构，来处理交易进程和分配时间戳。分散式系统（例如比特币）则复杂得多：没有独立多机构，相反它依赖一个由地理位置上分散的且互相独立的机构组成的网络通过一种算法来进行交易。本文关注的最有意义的一点就是交易的时序，在这个时序中每笔交易被打上时间戳以及与其他任意一笔交易的先后顺序。正如我们想提出的，这个交易时序进程的限制导致了信息泄漏的发生。

>A. Transaction processing in decentralized systems

## A. 分散系统中的交易进程

>Due to transmission delays in a network, it is impossible to reach a global agreement on the actual order that transactions happen in a decentralized system. Such latency issues are an all too familiar problem in the current high frequency market structure characterizing equity trading, but for a number of reasons this problem is far more challenging in a distributed ledger setting. One such reason is that while the number of nodes (or exchanges) in equity trading is fairly small, the nodes in a distributed ledger system can number in the thousands, or even tens of thousands. A second is that these nodes can be distributed globally, making transmission delays an inevitable feature of the system.

由于网络传输的延迟性，在交易发生的分散系统中不可能达成一个确切时序的共识。这个延迟问题在高频交易市场的公平交易中经常发生，但是这个问题由于一些原因导致其在分布式账本系统中更具挑战性。其中一个原因就是交易节点（比如交易所）的数量在股票交易中非常少，但是在分布式账本系统中的交易点却成千上万。其次，这些交易节点分散在整个系统中，使得交易延迟不可避免。

>As a result, the various algorithms employed in a distributed ledger can only attempt to make all parties in the network agree on what (for short time intervals) is essentially an arbitrary ordering of the transactions. In the period between the broadcast of a Bitcoin transaction and its being processed by the network, for example, there are typically no guarantees offered with regards to its order relative to other transactions in the same state or to what timestamp the transaction will eventually be assigned.

因此，目前应用于分布式账本系统中的各种算法仅仅能让网络中各方（在一小段时间内）在什么是交易无序上达成一致。比如，在比特币交易的发布到通过网络传输这段时间中，交易之间的时序没有任何证明，也没有对交易时间戳的分配的证明。

>Crucially, the content of any transaction (where we are using transaction in a broader sense to include events such as a trade, a quote, or indication of interest) that will be posted to  the distributed ledger will be observable by all or parts of the network in this potentially lengthy period. This visibility allows other participants to act upon the information carried in  the transaction before it has been processed and received a guaranteed ordering relative to other transactions. Depending on the algorithm employed, some participants may also be able to alter the relative ordering of transactions, arbitrarily and potentially to their own benefit. And, depending on the extent of these alterations, any changes can range from being undiscoverable to the rest of the network, to being probable but unprovable. Once the transaction has been processed by the network, it will be permanently stored and given a definitive place in the order sequence in the distributed ledger. Only at this point is it possible to offer the guarantee that no new transactions may end up with an earlier time priority than the original transaction.

更重要地，将要在分布式账本系统中发布的交易（我们使用广泛意义上的交易来包含交易、引用、预购等等）内容在这一段潜在的漫长时间内变得对所有或者网络中的部分参与者可见，这就让其他参与者可以在交易得到证明之前根据交易信息进行操作。通过算法，参与者也可以修改交易的相关顺序，从而有意无意地增加自己的收益。根据这个修改的影响范围，任何改变都可以变得对其余网络不可见，可信但无法证明。一旦交易在网络中达成，它将被永久存储，并且在分布式系统中分配一个绝对序号。只有在这个时候才能保证新的交易不会再原交易之前完成。

>The lack of time priority in the validation stage applies generally across distributed ledger settings, but its severity depends upon the validation process employed. In particular, public distributed ledgers and private distributed ledgers use different validation methods and, as we discuss below, these methods can affect the information leakage problem.

验证阶段缺少时序通常应用于分布式账本系统中，但是其严重程度取决于验证过程。特别地，公共分布式分类帐和私人分布式分类帐使用不同的验证方法，并且，正如下面我们讨论的，这些方法会影响信息泄露。

>B. Transaction processing in Bitcoin and Ethereum

B. 比特币和以太坊中的交易进程

In a public distributed ledger system like the Bitcoin and Ethereum networks, transactions are processed by “miners” (essentially high powered computers) who write these transactions into blocks, and collect the fees associated with each transaction. There are numerous miners on the network, and which one writes a specific block is random, with the probability of doing so equal to their share of mining power relative to the total. When a transaction is broadcasted, it is forwarded by the network and kept in a transaction pool known as a mempool. When a miner mines a new block, it will consist of transactions from this mempool. The miner can reorder or censor transactions as they see fit, to the point where empty blocks (with no transactions) are perfectly valid. This behavior, combined with infrequent generation of blocks, can result in unpredictable delays between a transaction being broadcast and it being assigned an order relative to other transactions in the distributed ledger. As Figure 1 shows, these delays in Bitcoin have typically been around 10 minutes, but they can be much longer due to there being no theoretical upper bound on how long it can take.

在如比特币和以太坊为代表的公共分布式分类账本系统中，交易通过“矿机”（通常是高性能电脑）向对应区块中写入交易信息并收集每笔交易的费用进行。互联网上的矿机数量非常庞大，矿机写入的区是随机的，就有可能
当一个交易发布后，它将通过网络推进并且存储在一个称为交易池的内存池中，矿机写入新的区块中的信息包含来自内存池中的交易。矿机根据配置重新排序或审查交易，在这个时候，空区（没有任何交易的区）也是完全合法的。这种行为，连同区块的少有的分裂，会导致在分布式账本技术中，交易的公布到其被确定分配相对于其他交易的顺序之间出现不可预见的延迟。如图1所示，在比特币中这类延迟达到了10分钟，但是由于没有一个理论上的上界所以这种延迟可能会更长。

Most miners are agnostic to the content or source of transactions, and will seek to optimize their own returns by including as many transactions as they can in each block (thus collecting the fees). This category of miner will not have any incentive to manipulate the relative order of transactions, but neither will they have any interest of keeping them in any particular order. Thus, transactions containing conflicting orders may be shuffled around arbitrarily for a time period of multiple minutes.

绝大部分矿机并不知道交易的内容和源头，它们会通过尽可能多地收集每个区块中的交易信息（收集费用）来优化返回内容。这类矿机不会改变交易的相对顺序，也不会让交易保持某种顺序。因此，顺序冲突的交易可能在几分钟之内被重新排序。

Other miners, however, may have motivations beyond simply collecting transaction and block-fees. For example, they can decide to inspect the orders awaiting processing in the mempool, and then put in their own conflicting orders in front of the other orders. The fact that they can blame the resulting sequencing on network delays, which do occur in decentralized systems, makes this a tricky problem to fight. That most miners are anonymous only contributes to the difficulty of knowing whether such data snooping (and front-running) has actually occurred. What is clear is that there should be no expectation of privacy for broadcast data in a public distributed ledger.

然而，其他矿机可能对简单收集交易和区块费用感兴趣。比如，它们可以监控时序，等待内存池中的进程，然后把它们自己冲突的序列提交到其他交易序列的前面。它们可以将结果序列归咎于网络延迟，这个情况确实在分散系统中存在，使得这个问题变得十分棘手。大多数的矿机是匿名的，这使得它们很难知道这种数据监听和前线运行是否正常。