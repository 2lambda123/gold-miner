> * 原文地址：[The RedMonk Programming Language Rankings: January 2021](https://redmonk.com/sogrady/2021/03/01/language-rankings-1-21/)
> * 原文作者：[RedMonk](https://redmonk.com/sogrady/2021/03/01/language-rankings-1-21/)
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 本文永久链接：[https://github.com/xitu/gold-miner/blob/master/article/2021/The-RedMonk-Programming-Language-Rankings-January-2021.md](https://github.com/xitu/gold-miner/blob/master/article/2021/The-RedMonk-Programming-Language-Rankings-January-2021.md)
> * 译者：[霜羽 Hoarfroster](https://github.com/PassionPenguin)
> * 校对者：[Chorer](https://github.com/Chorer)、[chzh9311](https://github.com/chzh9311)

# 2021 年 1 月 RedMonk 编程语言排名

> 这个版本的 RedMonk 编程语言排名由 MongoDB 发布。从设备到云端，MongoDB 让你能够以处理代码的方式去处理数据，并且不局限于任何一种语言 —— 因此你可以更快编译和分发你的应用。如果你是一个 Python、.NET、Java 或是 JavaScript 开发者，那不妨现在就从 [MongoDB University](https://university.mongodb.com/learning_paths/developer) 开始了解吧！

一方面，鉴于现在已经是三月而不是一月了，本季度的排名似乎发布得比较晚。另一方面，考虑到这些数据与去年这个时候的数据大体相同，我们大可以说这份排名仍然是最新的。但是，无论更新得过晚还是过早，这份排名都是完整的，值得你仔细阅读。

与往常一样，这份排名是 Drew Conway 和 John Myles White 于 [2010](http://www.dataists.com/2010/12/ranking-the-popularity-of-programming-langauges/) 年末完成的第一版的后续版本。尽管数据的收集方式发生了变化，但是基本步骤仍然保持不变：我们从 GitHub 和 Stack Overflow 提取语言排名，并将它们结合起来以尝试反映该语言在代码（GitHub）和讨论（Stack Overflow）两个方面上的热度。发布排名的初衷不是展示当前语言使用情况的统计数据，而是将语言讨论和使用情况相关联，以努力提炼对潜在的未来采用趋势的见解。

## 我们的当前的处理方式

GitHub 的分析数据来源是 GitHub Archive 上的拉取请求，GitHub 也曾用这种方式整理并发布 OctoVerse 报告（GitHub 会发表年度 Octoverse 报告，呈现在开发社群、技术趋势和国际化等各方面的成果）。查询要尽可能地与上一个过程具有可比性，这是我们的一个设计理念。

* 语言基于原仓库的语言。尽管这样做会不得不注意一些事项（后面会说明），但确实具有与我们以前的方法保持一致的好处。
* 我们剔除了 fork 的仓库。
* 我们使用汇总的历史记录来确定排名（虽然它基于表结构的更改，但这项任务不再可以通过单个查询来完成。）

对于 Stack Overflow，我们使用其高效的数据浏览工具收集所需的指标。

在不加描述的情况下，请留心其他常见的注意事项。

* 此分析报告所包含的语言，必须出现在 GitHub 和 Stack Overflow 中。
* 免责声明：这些排名不一定可以充分展示语言的总体使用情况，它们不过是检验我们认为可以预测未来使用的两个方面之间的相关性，这就是其价值所在。
* 其实有着许多潜在的社区可以作为我们调查数据的来源，但我们还是选择了 GitHub 和 Stack Overflow，首先是因为它们的规模很大，其次是因为它们开放了分析所需的数据。尽管如此，我们仍然鼓励有兴趣的团队通过其他来源进行自己的分析。
* 所有的数字排名都应该用质疑的目光看待。出于兴趣考虑，我们在此处严格按数字排名。通常来说，数字排名并不如语言的等级或分组那么中肯。在许多情况下，图标的某一个位置与下一个位置是无法区分的。但是，一部分语言和另一部分语言之间的间隔通常代表相对受欢迎程度的实质差异。
* 此外，排名越低，可用于排序的数据就越少。除了顶层的那些语言之外，我们通常依赖于快照，可以评估的数据量很小。因此语言的位置越靠后，它的实际位置就越不可靠。
* 诸如 Mathematica 之类的具有除 Stack Overflow 以外社区的语言在该轴上的代表性不足。我们不可能扩展衡量一百个不同的社区站点，因为许多站点没有可用的公共衡量标准，而且相互衡量不同的社区站点在统计上是无效的。

了解这些事情之后，让我们看看 2021 年第一季度的数据：

![2021 第一季度数据](https://redmonk.com/sogrady/files/2021/03/lang.rank_.0121.wm_-1024x805.png)

除了上面的图（即使是全尺寸图也可能很难让人看清楚）之外，我们还提供以下数字排名。可以观察到，每一个分段之间有几种语言，如下所示（它们在此处按字母顺序列出，而不是合并为一个分段，因为后一种方法往往会引起误解）。

1. JavaScript
2. Python
3. Java
4. PHP
5. C＃
5. C++
5. CSS
8. TypeScript
9. Ruby
10. C
11. Swift
12. R
13. Objective-C
14. Shell
14. Scala
16. Go
17. PowerShell
18. Kotlin
19. Rust
19. Perl

在我们上次的排名中，前 20 名中的排名相当稳定（在指标的增长中这种情况并不罕见）。相反，本季度的排名具有很大的变化。前 20 名中有一半语言都经历了一定程度的变化，这是非常不寻常的。很难将其明确地归因于任何更高水平的宏观趋势，但数据与行业相吻合，该行业在最初的疫情封锁混乱之后的第三、四季度又重新找回节奏，但是为生存而不得不让步于不那么好的新发展路线。

不仅有变化的存在，而且值得注意，如果这些变化继续持续下去，可能会有些大事情发生！我们将在近期对它进行探讨。Python 排名第二是值得被我们讨论的变化之一，而 Java 跟在 Python 之后，仍然非常受欢迎，实际上相比于和 PHP 之间的差距，Java 与第一名的差距更小，但是 Python 捍卫其新的高排名的能力非常引人注目。

有了该小结，让我们来到本版排名中最重要的部分。（注：括号中的数字是语言排名相比于上次的净变化。）

* **JavaScript（0）：** 我们的分析是基于语言排名的波动的，因此这里有关 JavaScript 的讨论似乎有悖直觉，因为 JavaScript 根本就没有变化！但值得注意的是，JavaScript 的表现仍然很亮眼。尽管有来自各种新兴语言的竞争，各种碎片化的讨论，甚至还有不少对 JavaScript 语言本身的批评，它仍然非常受欢迎。自 2018 年 1 月第一季度发布报告以来，JavaScript 的拉取请求增加了 453 个百分点，而这个季度比上一季度增加了 96％！这是基于已经相当庞大的 commit 提交得到的数据。简而言之，JavaScript 尽管有其不利因素，但仍然是编程语言中最强大的那份力量，这一点在业内没有其他语言能比拟，并且数据中没有迹象表明这种情况可能很快就会改变。

* **TypeScript（1）：** 说到 JavaScript 的表现，其实 TypeScript 的排名也在稳步提升中。就其本身而言，这是令人印象深刻的。Swift 是记忆中唯一能进入前 10 名的语言（但这只是一个季度的数据，它随后迅速反弹并自 2018 年以来一直保持相对稳定地排在了第 11 位）。TypeScript 面临的第一个问题就是它是否能够坚持维持自身地位的稳定，而现在更合适的问题是该语言的上限是多少。TypeScript 在最近的八个季度中上升了六位，在整个行业中，它的受欢迎程度显而易见。然而，与增长一样有趣的是，增长源于这种语言它本身。

* **Ruby（-2）：** 如先前在这个领域中所讨论的，Ruby 一直保持平稳的排名增减，长期以来一直处于下降的状态。然而，本季度的表现使人们对它能否继续保持佳绩提出了疑问。当我们在 2012 年开始这些排名时，Ruby 是排名第五的最受欢迎的语言，并且大约五年来，它一直保持着这一地位。但自 2016 年以来，Ruby 一直在下滑，本季度它已被 CSS（是的，我们知道你们中的许多人都认为它不应该被列入排名）和 TypeScript 一起超过了。Ruby 在最近几年已努力解决一些性能问题，但撇开人们对它所标榜的与它所做到的存有疑问不谈，它对性能的关注似乎并没有在我们的衡量体系下改变自己的命运。需要明确的是有数十种乃至上百种语言会很高兴地与这些排名中的第九种语言互换位置，但是相比于实际的位置， Ruby 倡导者和用户应该更关心它的轨迹。Ruby 是一门有趣的语言，具有优美的语法，但要想在竞争激烈的语言市场中存活下去，这还远远不够。

* **Go（-1）：** 与 Ruby 一样，Go 的排名与其整体轨迹相比，也不再是一个值得关注的问题。经过最初的快速增长后，Go 在 2018 年排名第 14 位，达到它的顶峰后，Go 语言就成了一直稳定跌落的语言。正如前面所讨论的，相对于此列表中的其他一些语言，Go 的更狭窄的应用市场可以解释其中的一些问题。作为后端应用程序组合的主要竞争对手，Java 仍然是一种至关重要的且使用率很高的语言，而不是经过这么多年的服务而逐渐消失，这也没有帮助 Go。但是，不管它是稳定的还是下降的，如果 Go 有想成为真正行业力量的野心，那么其路径和结构可能需要进行一些改变。

* **R（1）：** 我们经常在 R 语言的世界中编程。R 语言是不少其他社区中学术界的主要内容。但这一种语言只是在一个领域（即数据分析）中表现出色，而在处理数据、分析数据之外，根本与那些社区或学科的内容不相关。R 语言是用来解决一个简单问题的几种语言之一：在当今多元化的世界中，一种专一化语言的命运可能是什么？它会被推拔高还是被拉低？通常，专用于特定内容的语言的性能要好于通用语言 —— 就像上面提到的 Java 与 Go。但是，R 语言是该规则的例外。尽管它的增长从未经历过快速或线性的发展，但在我们多年前开始进行排名时，该语言的排名为 17。而 R 在该分析中排名第 12，这可太有趣了！更重要的是它是通过超过 Objective C（-2）来到这个排名的！自推出预期的替代产品 Swift 以来，Objective C 这一以往前十名中的佼佼者，一直处于跌落的轨道中。然而，仍然令人惊讶的是能够看到一种专注于统计分析的语言能排在编写大多数 2014 年前的 iOS 应用程序所使用的语言之前。

* **Kotlin（1）和 Rust（1）：** Kotlin 和 Rust 彼此之间没有真正的关系，只是它们之间存在一定的功能重叠。Kotlin 是一种基于 JVM 的语言，具有现代语法，可以与 Java 进行自由混合，而 Java 是一种具有良好后端背景的语言，但它也是 Android 上的一等公民。Rust 是一种安全意识强的语言，已被 Mozilla 等组织广泛使用，也经常被认为是 Go 的替代语言。说到 Mozilla，他们将所有 Rust 商标和基础设施资产转移到了新的 Rust Foundation，Rust Foundation 是该语言的管理员。该语言也得到了 AWS、Google、华为和微软的支持。但是，Kotlin 和 Rust 的共同点是，它们在开发人员中的受欢迎程度使他们在本季度排名中分别上升了一个位置 —— Rust 排名第 19 位，Kotlin 排名第 18 位。如今 Kotlin 的发展速度已经远超 Rust，但看看 Rust 的管理员能否撼动这一局面仍然是一件很有趣的事情。

* **Dart（3）：** 在不到三年前，Dart 就被嘲笑说已经步入中年了。我们跟踪的开发人员对 Dart 的兴趣和相关的活动少之又少。但是，在 Flutter 框架推出两年后，Dart 排名上升了 3 位，刚好排在我们排名的前 20 名之外的 21 名。在 Dart 似乎停滞了两个季度之后（Kotlin 也是如此），人们怀疑 Dart 是否已经到了它的极限了。本季度的结果表明，这个猜想的答案是否定的。显然，Flutter 对这种语言的流行产生了实质性的影响，并且向大家明确了 Dart 并没有因为自身可以被编译为世界上其它的流行语言而遭受挫败，这反而对它还有利。虽然要在我们的排名中位居第 21 位已经非常困难了 —— Rust 可以证明 —— 但是看到这个季度 Dart 恢复上升的轨迹，我们可以将注意力重新放到 Dart 身上，看看它是否能够冲破 20 名的罗网，以及它可能会替代的语言。

鸣谢：我的同事 Rachel Stephens 在这些排名中编写了负责 GitHub 部分的查询。她还负责 Stack Overflow 数据的查询设计。

> 如果发现译文存在错误或其他需要改进的地方，欢迎到 [掘金翻译计划](https://github.com/xitu/gold-miner) 对译文进行修改并 PR，也可获得相应奖励积分。文章开头的 **本文永久链接** 即为本文在 GitHub 上的 MarkDown 链接。

---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[区块链](https://github.com/xitu/gold-miner#区块链)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计)、[人工智能](https://github.com/xitu/gold-miner#人工智能)等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)、[官方微博](http://weibo.com/juejinfanyi)、[知乎专栏](https://zhuanlan.zhihu.com/juejinfanyi)。
