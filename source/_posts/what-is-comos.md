---
title: 十七问讲清楚COMOS和Polkadot的区别
date: 2019-04-19 14:47:12
tags: 
- Bitcoin
- Blockchain
thumbnail: /IMG/korea.jpg
---

> 不少人一直对以太坊很着迷，大部分原因是以太坊拥有「使程序彼此通信」的能力。事实上，有人可以利用不同团队构建的不同项目的智能合同，并且无需从零开始，这意味着随着时间的推移，创建应用的机会越来越多。然而，以太坊目前使「程序彼此通信」的可组合性只能发生在链上的智能合约之间。如果想构建一个跨多个区块链发送资产的 Dapp，则需要区块链互操作性 —— 即区块链间相互「通信」的能力。加密学中多链的未来是互操作性。目前，有不少项目正致力于解决这一问题，其中最突出的项目是 Cosmos 和 Polkadot。

*作者：Sunny Aggarwal，Cosmos 团队研究员及核心开发人员*

许多人认为这两个项目很相似：两者都提出了建立连接区块链（Cosmos 的 Hub 和 Polkadot 的中继链），在其他链之间传递消息；两者都有一个工具箱（Cosmos SDK 和 Substrate），供开发人员构建可以直接插入相应生态系统的区块链；两者都有桥接机制来连接传统的区块链（Cosmos 的 peg zones 和 Polkadot 的 bridge chains）。但从设计理念，技术架构和代币用途的角度来看，这两个项目存在着巨大差异，而这些往往被忽略了。为了解其中细微差别，本文列出与 Tendermint/Cosmos 团队的研究员和核心开发人员 Sunny Aggarwal 的谈话。

![0419-comoshub](/IMG/0419-comoshub.png)

> **1、 Cosmos是什么？**

Sunny：在解释 Cosmos 是什么之前，必须先解释一下区块链行业。我喜欢把它类比历史时期，在人类文明的早期，王国和小村庄是从未真正规模化的独立政治实体。这就是早期区块链生态系统的样子。要构建自己的区块链，您必须引用比特币代码库。但是后来因为比特币代码库不是一个通用的系统（它是专门为比特币 (货币) 设计的）所有早期的区块链应用程序都类似于比特币，比如莱特币、狗狗币等，只有少数项目试过做一些创新——例如 Namecoin。但是他们的设计非常糟糕，因为他们试图使用比特币代码库去做和改代码库的设计初衷完全不相关的东西。接着以太坊出现了，它做了两件大事：第一是人们可以更容易构建应用程序。虽然以太坊的稳定性不是最好的，但比起单纯地引用比特币代码，以太坊使创建复杂的应用程序变得更容易。第二是以太坊还允许这些应用程序相互协作。建立在以太坊区块链上所有不同的应用程序之间都具有这种互操作性。

回到历史的类比：几千年来，为了实现大规模的经济一体化，我们创造帝国，因为我们意识到大规模的政治一体化实现了大规模的经济一体化。这也就是以太坊所做的。他们根据单一制的规则，将所有内容放在单一个区块链上，实现大规模的互操作性。但就像帝国一样，它也带来了许多问题。其中一个问题是可拓展性更差，这里的可拓展性意味着技术可扩展性和社会可扩展性。历史上最终罗马帝国崩溃了，因为它在境内有太多的文化和利益竞争。而以太坊也有很多利益竞争。Github 上有数百个以太坊改进报告（EIP），其中一些 EIP 甚至是矛盾的（例如，有些人想要状态租金，有些人则不想）。这就像在一个帝国内，要建立一个考虑到所有利益相关者的治理机制一样。这并不容易，且容易导致动乱。过去的一百年中人类最大的创新是 —— 意识到可以在没有政治一体化的情况下实现经济一体化，并通过如下途径做到了这一点：

- 一是自由贸易区，例如欧盟，北美自由贸易协定，世贸组织这样的组织 —— 这些组织促使自由贸易成为准则。

- 第二个是体系，例如联合国这样的非政府体系，它只是一个共同的论坛，主权国家可以聚集在一起讨论和谈判，不必采取全球冲突来解决分歧。

- 第三个创新就是「集装箱标准化」（containerization）。我们对集装箱进行了标准化，使得世界上任何港口都可以接受同一个集装箱，比如现在可以把阿根廷某个港口的集装箱运送到世界上任何港口。Cosmos 想要尝试的就是这种「集装箱标准化」。
- 第四个是互联网。在地球这一边，可以与地球另一边的任何人做生意，这显然也有助于经济一体化。

回到 Cosmos 上，我们意识到为了解决以太坊帝国的治理问题，需要创建一个可以拥有很多区块链的系统，每个应用程序或社区都应该拥有自己的区块链，以便对其进行治理。只要这些系统能够进行相互操作和发送资产，我们就能实现 Cosmos 的愿景。

> **2、Cosmos 的 IBC （跨链通信）的架构大致是什么？**

Sunny：首先 Cosmos 采用了 Blockstream 多年前的侧链提案，我们的目标是创建一个通用标准——即称之为跨链通信（IBC），它允许两个链相互具有另一个链的轻客户端。简单地说，假设我们有两个智能合约链：以太坊和 Tezos。以太坊在两条链上都有智能合约。在以太坊上，我们可以从 Tezos 区块链上提交块头的智能合约，然后会在以太坊上处理块头以确保它们是有效的。同样的，Tezos 将接受来自以太坊的块头。Tezos 接收块头的目标只是提供一种标准方法向另一个区块链证明它的状态。而其实关于「状态」人们可以建立的更高级别的协议。我们现在在实现代币转移或资产转移，因为这将解决大多数人 80％的运用，这可能是我们能做的最简单的事情之一。

> **3、 Cosmos 资产转移如何运作，以及如何证明资产确实在两条链之间转移？**

Sunny：基本上我们要做的就是——如果你要在以太坊和 Tezos 之间进行资产交易。你在以太坊链上取一个代币，在一个智能合约中把它锁定，然后全网广播：「看，我的这个代币被锁定在以太坊」，这就证明了资产转移。然后将在 Tezos 链获得一个新代币，上面写着「这个代币标志着对以太坊上的锁定资产的占有权」，假设 Tezos 上的代币以 1：1 的比例可兑换为以太坊上的指定资产，相当于在 Tezos 链上占有了在以太坊上相应的资产。然后可以使用 Tezos 链上这枚新代币并在 Tezos 智能合约中使用它，或者可以在 Tezos 的 dex 上使用它。使用后，就等于是销毁了在 Tezos 上的代币，在那个销毁上向以太坊提交一个证明：「看，这证明了我在 Tezos 上的确销毁了代币！」然后以太坊智能合约将解锁此前冻结的代币。这就是实现一对一的互操作性的过程以及两种资产如何保持对等。

> **4、那么 Cosmos 的代币交换方式与现有的跨链原子互换实现方式有何不同？**

Sunny：原子交换与 Cosmos IBC 不同。原子互换允许两个人交换他们持有的资产。比如说，我有价值 5 美元的 BTC，你有价值 5 美元的 ETH。原子互换意味着我在比特币区块链上转给你价值 5 美元的 BTC，你在以太坊区块链上转给我价值 5 美元的 ETH。事实上立即交换的只是两条链上那笔资产的所有权（或者非立即交换，因此称为「原子」）。但正如你注意到的那样，BTC 仍然在比特币区块链上，ETH 在以太坊区块链上。实际上，没有任何价值跨链。

Cosmos 提出了完全不同的东西。Cosmos 说的是，我希望我的 BTC 在以太坊区块链上。以 Maker 为例，目前，Maker 的 Dai 都是由 ETH 抵押。也许你想用 BTC 来抵押 Dai，但是你需要有一种方式能在以太坊区块链上拥有 BTC。原子互换无法解决这个问题。原子互换无法让 BTC 离开比特币区块链。在 Cosmos 的模式下，我们允许所有资产本身在链上转移。

> **5、Peg zong 究竟是什么呢？**

Sunny：目前 IBC 的一个主要问题是设计时要考虑到 BFT （拜占庭容错机制）。顺便说一句，早在 2014 年我们开始开发时并没有 BFT，Tendermint 是第一家考虑 BTF 的公司。Jae Kwon 写了一篇论文回顾了 90 年代所有共识协议，并意识到你可以通过 POS 机制将 BFT 共识协议运用在公共区块链中。于是，他开始创立了 Tendermint。然后，许多其他项目开始采用 BFT，比如以太坊的 Casper，Dfinity 等，这些区块链现在基本上都是基于 BFT 的系统。IBC 有点像集装箱化，Cosmos 说，「好吧，让我们推进这些共同标准，以便世界上所有的区块链都可以相互通信」。回到「集装箱标准化」这个类比，世界上所有港口都可以接受相同的集装箱。如果有一个港口非常固执不愿意接受这个标准，你怎么办？这就是比特币。比特币永远不会采用 IBC 模式！ 我们仍然希望让 BTC 成为这个 Cosmos 生态的一部分，但是我们不能强迫它使用 IBC。所以这就是我们提出另一个概念桥接分区（Peg zongs）的原因，Peg zongs 是一种将 IBC 嵌入到一条区块链上的方式。

![pegzong](/images/pegzong.png)

> **6、那么 Peg zong 是如何运行的？**

Sunny：您必须为此创建一个「翻译型」链。我们将一个 Tendermint 区块链放在比特币区块链前，这个 Tendermint 区块链充当了反射出比特币区块链上发生的事情的镜子。它将一个确定性小工具放在比特币区块链上。所以我们会说，一旦比特币多了 12 块区块，比特币将反映在 Tendermint BFT 链上，我们会使用 Tendermint BFT 链与 Cosmos 通信。它就像是启用了 IBC 的比特币的镜子。Tendermint 桥接分区的验证者将拥有比特币区块链上的多重组合，他们将在那里执行该翻译任务。 如果他们行为不端，他们在 Tendermint 和比特币链上的 stake 就会减少一半！

> **7、 对 Cosmos 与 Polkadot 竞争有什么看法？Cosmos 的互操作性方法有何不同？**

Sunny：我认为 Polkadot 变得越来越像 Cosmos。Polkadot 认为中心有一个中继链，并且所有的平行链都连接到 Polkadot 中继链。不想与成为网络的一部分，就会作为桥接分区被记录下来。Cosmos 则相反——从设计系统的方法就与 polkadot 不同，在系统中事物具有默认主权。每个链都有自己的验证者集合，并且拥有很高的主权。我们这样设计使它可以在没有中心区块链的情况下运行。Cosmos Hub （连接所有链条的「指挥」区块链）不一定是该系统的核心。反而我们推动的是想要有许多中心的概念。所以现在我们已经有了两个中心区块链。一个是 Cosmos Hub，这是我们上周推出的。还有一个叫做 Iris Hub。Iris Hub 更专注于企业客户，尤其是中国客户。这真的很酷，因为那时你可能有连接不同中心的不同的生态系统。

在我看来，Polkadot 可以成为的 Cosmos 的互联区块链的中心。

> **8、那么，Cosmos 在哪些领域与 Polkadot 竞争呢？**

Sunny：我最大爱好是将区块链与历史类比。与帝国相比，小主权国家的问题是他们往往没有能力保卫自己。我们所做的是创建联盟和系统，让较大的国家可以将其安全性「出租」给较弱的国家。北约就是一个例子。一群国家聚集在一起，聚合安全性，当你这样做时，你不可避免地会放弃一些主权。我认为没有人会说欧盟或北约就像一个帝国一样——你仍然拥有很高的主权，但你确实放弃了一些主权。例如，瑞士和比利时——两者在人口统计学上的特点非常相似，但具有不同的倾向。瑞士高度重视在其他方面的主权。所以他们拒绝加入这些更大的国际组织。而像比利时这样的国家，由于历史原因，他们更愿意加入大型组织。也许放弃了一点主权，但获得更高的安全性。我们也看到了英国脱欧 —— 一个国家选择争取更多主权的例子。

因此，对于 Cosmos，我们想法是让区块链选择他们想要的模型。如果他们想要拥有完整的主权，他们就可以拥有完整的主权。但随后 Cosmos Hub 就会类似 NATO —— 一种超安全的区块链层。区块链有权选择要求中心验证区块链，而中心将收取费用。中心仍将让区块链完全控制自己的治理。Polkadot 的目标是提供共享的安全机制（区块链放弃共识主权以换取共享安全）。

> **9、由于 Polkadot 默认情况下汇集了建立在 polkadot 上的平行链的安全性，而 Cosmos 区块链必须确保他们自己的安全性，开发人员在他们没有足够的资源来保护自身区块链的安全时，是否会更有动力建立 Polkadot？**

Sunny：Cosmos Network 和 Cosmos Hub 之间存在差异。 Cosmos Hub 区块链基本上为 Cosmos 生态系统中的链提供相同的服务（类似 Polkadot 中继链），Cosmos 生态系统将为需要安全性的链提供聚集性安全性。所以 Cosmos Hub 就与 Polkadot 中继链非常相似。

> **10、因此 Polkadot 的区别在于，如果你是一个平行链，那么你必须参与聚合安全性。而若是在 Cosmos 上，是否有权选择不参与？**

Sunny：对的，可以选择不参与。例如，Binance 正在使用 Cosmos 的框架（Cosmos SDK）构建他们的 dex。我认为他们可能会持有一个主权链。这只是猜测，但我认为他们可能会尝试使用他们的 BNB 代币作为 Stake 代币，因为实际 BNB 有足够的价值来提供合法安全性。但是，也许另一个 dapp 开发人员明天会说，「我创建了这个很酷的 dapp，但我不知道在哪里让社区在链后真正引导 Stake」。开发人员基本上可以向 Cosmos Hub 提交验证请求。

> **11、我知道对于 Polkadot 来说，用于平行链的插槽数量有限。整个治理过程决定谁成为平行链。Cosmos 是一样的吗？Cosmos 的治理过程是什么？**

Sunny：不一样。这是 Cosmos Hub 与 Polkadot 的共享安全理念之间的主要区别。

Polkadot 将有数以千计的验证者，基本上将验证者分配给特定的链。因此，在 1,000 个验证器中，50 个分配给一个平行链，50 个分配给另一个，依此类推。Cosmos Hub 将采用不同的方法。区块链可以随时建立并向 Cosmos Hub 请求验证，并为验证工作，通胀时间安排，预期的交易费用等提供相应的奖励。然后，Cosmos Hub 上的每个验证者都可以单独说「我想验证该链。」然后验证器将把它们的 Atoms 放在 Hub 上 Stake 并执行验证。如果他们验证失败，他们的 Atom stake 在 Hub 上被削减。这在一些方面上是有好处的。我个人经营一家小型验证公司，没有那么大的验证能力。我部分时间运行，因为我主要关注 Cosmos 核心开发。因此，我只能验证两个或三个链。但是一些验证者是公司——例如，我在 Epicenter （Podcast）的联合主持人，Brian，在一家名为 Chorus One 的验证公司全职工作。也许 Chorus One 已准备好验证成百上千个区块链。他们有自己的公司，有很多服务器。因此，利用 Stake 机制（您所拥有的 Atoms 越多，您可以执行的验证工作越多），资本和资源分配就越有效。

> **12、这对终端用户有何影响？**

Sunny：目前，Polkadot 和以太坊中，所有区块链都必须为相同的安全性付费。所以这意味着在以太坊上，使用 CryptoKitties 的人必须与使用 Dai 交易数百万美元的人支付相同的交易费。这是不对的，因为这意味着使用 CryptoKitties 的人为安全性付出了过高的代价。这就是 CryptoKitties 非常昂贵的原因。

Cosmos Hub 拥有共享安全模型，经济上相对不重要的区块链将获得更少的奖励，更少的交易费用，因此验证它们的验证者更少。具有高价值的应用程序的区块链需要高昂的交易费用支付他们拥有的大量验证者的费用。这就是配置效率对终端用户的影响。

> **13、有没有一些案例比起使用 Polkadot 更适合使用 Cosmos 呢？Polkadot 似乎更关注区块链链之间的任意函数调用，而 Cosmos 似乎更专注于链之间交换资产。**

Sunny：当然了。您可以将 IBC 视为 TCP IP。这都是互联网协议。但是在 TCP IP 之上，您需要这些更高级别的协议。网络上需要 HTTP，文件传输需要 FTP 文件传输，电子邮件需要 SMTP 电子邮件。与 IBC 相同，这只是简单的基本协议。我们在基本协议上将构建其他协议，但我们只在代币交易阶段。据我所知，Polkadot 还没有真正构建任何更高级别的协议（Jason：Polkadot 采访中有更多细节）。他们甚至不知道不同链上会存在哪种虚拟机（VM）。因此，从这方面来看，Cosmos 和 Polkadot 能力相同。

Cosmos Tendermint 开发团队遵循迭代设计理念。我们不想夸大我们想要建造的东西，因为当人们过度兴奋时发现我们正在推销 5 年后，10 年后的事情，就会变得非常失望。复杂的跨链合约调用是完全可行的。我们正在帮助那些能够实现这一目标的人，比如 Agoric。但是，让我们首先设定一些预期并专注于代币交易。这是我们的首要任务。

> **14、Polkadot 区块链如果想通过加入中继链来与其他区块链建立安全保障，就必须放弃他们的共识。开发人员在 Cosmos 上构建时会放弃什么？这里的权衡是什么？**

Sunny：主要是开发人员必须确保实施 IBC 标准。在一些桥接分区上，我们将允许「黑客式」例外——比如比特币和诸如此类——高价值的东西，值得允许例外。但是，如果你不是那几个区块链中的一个，你必须确保将 IBC 构建到你的核心协议中。这是开发权衡中，这是为了成为系统的一部分该做的事。现在，假设拥有 IBC，那么还有什么必须放弃？很少。现在请记住，对于某些 dapps——例如财务问题——他们并没有使用 Cosmos Hub 来提高安全性。事实上，如果您根本不想使用 Cosmos Hub，您甚至不必连接到 Cosmos Hub。您可以连接到其他 Hub，例如 Iris Hub，或者您可以直接连接到需要与之通信的区块链上。但是如果每个区块链都试图通过 IBC 连接与其他链相互连接，那么你就有了 n 的平方的连接。在我看来，这并不是非常有效。但我很高兴被证明是错的。这就是出现的紧急行为。

我认为，在我们在现实世界中看到的大多数网络中，经常最终会回到中心辐射架构或多中心辐射架构。例如航班。理论上，任何飞机都可以从世界上任何机场起飞。但你最终还是会看到世界上有一些主要机场。我只是在 Cosmos 中看到类似的属性，你将拥有主要 Hub 链，例如 Cosmos Hub，Iris Hub，也许还有一个 Polkadot Hub，这些中心将帮助区块链链更容易连接。

> **15、基于此，听起来像是 Cosmos 运行中，Cosmos Hub 并不是必要的。因此，通过扩展，是否需要 Atom 代币才能完全实现 Cosmos 愿景？**

Sunny：不必要。您不必须使用 Cosmos Hub。因此，Cosmos 网络不必要存在 Atom 代币，但 Cosmos Hub 需要 Atom 代币存在。这个想法只是为了创建一个高价值，高安全性的区块链，因为 Atom 代币是有价值的。请记住，Atoms 并不是钱。我们并没有制造货币代币。我们不是在制造我们的货币——实际上，Atom 是恶性通货膨胀，目的地试图阻止它们成为货币。我们的确想过创建一个 Stake 代币。如果你愿意，你可以把它们想象成一个美国出租车运营许可证（价格有波动），你需要这个标记才能成为一个 staker 并参与 Cosmos Hub 的权益证明。而且你知道，你可以拿着许可证并希望它能够涨价，但你可以获得的最大价值就是去开出租车获得收益或参与权益证明并且帮助确保安全和验证。

> **16、那么 Cosmos 如何与以太坊竞争？**

Sunny：人们说 Cosmos 试图窃取以太坊社区时，事实并非如此。有几种方式去考虑这个问题。

在开发人员社区方面，我们只是尝试构建一个免费的开源工具。我们所有的工具 —— Tendermint，Cosmos Core，SDK —— 所有这些都只是开源工具。我们构建工具目的是为了更容易实现多链，我们认为我们的工具比 EVM 更好。因此，我们希望那些真正想要推动这一愿景的开发人员选择最适合的工具。如果包括创建主权 Comos 链，那就动手做吧。如果包含到与 Cosmos Hub 建立共享安全性的区块链链，那也动手吧。我想我们只是在 dapp 开发人员的工具库中添加更多工具。我甚至不是说 Cosmos SDK 是最佳架构。Substrate 可能是比 Cosmos SDK 更好的架构！我们从合作中受益——我从 Casper 那里学到了很多关于共识的知识，我确信 Casper 也已经从 Tendermint 那里学到了很多关于共识的知识。我们在某些方面竞争吗？是的。但我希望我们竞争的目的是创造最好的工具。现在除了有开发者社区之外，还有物品称之为「信仰」社区的社区。「信仰」社区中的人是那些相信 ETH 是金钱的人，或者是那些试图将其变成模因币的人。我认为到目前为止，比特币和莱特币可能是两种加密货币模仿货币方面做得最好的。我个人对 Grin 也非常感兴趣 —— 我认为 Grin 比比特币有正确的说法和更好的货币政策。但是谈到像 ETH 社区，我认为 Cosmos 本身并没有在这个游戏中占有一席之地。我们不是在创建加密货币。我们不是在争相成为货币，但我们一直在思考这件事。我们有一个名为 Photons 的提议，试图创建一个加密货币。但主要目标是尝试不断发行货币供应——这些货币并不像 BTC 或 ETH 那样过度通货紧缩。

但随后 Grin 出现并做到了。所以现在，我认为我们不再需要做 Photon 了。我们想要的主要实由 Grin 社区完成的。而且他们的模仿方式比我们以往任何时候都要好得多。所以我们并没有与以太坊社区有真正的竞争关系（在货币状况上），我希望他们能够让 ETH 成为最好的加密货币。

> **17、预计在未来 5 年，互操作性领域是什么样的？ Cosmos，Polkadot 和以太坊会如何？**

Sunny：我认为 Tendermint BFT 在可扩展性方面有很大帮助，这将有助于创建许多在当前系统下可能无法实现的 dapps。我希望人们对资产的看法发生变化。我认为我们会看到更多抵押代币出现，我认为这些代币会因具有基础功能而具有价值。在拥有 Cosmos 的世界中，我现在可以使用 ZCash 状态机，并将其作为 Cosmos 区块链。我可以将我的 BTC 移到 Zcash，或者我可以将我的 Dai 移动到 Zcash。我可以同时使用该区域的零知识资产和任何资产。那么在那个世界里，ZCash 加密货币的价值到底是什么？ 我不太确定！我提到 ETH 正试图从空气币转向金钱币，因为他们意识到要赚钱。我们有必要借鉴，不能仅仅依赖于底层功能，因为在 Cosmos 的世界中，我们将资产与其底层逻辑分开，任何资产都可以使用任何逻辑系统。我们将看到很多人正在重新思考当前区块链使用的代币模型。



------

关注博客：**[比特兽](https://btc.monster)**（BTC.Monster）

关注微信公众号：**区块链猎场**（ID：BlockchainTalkShow）