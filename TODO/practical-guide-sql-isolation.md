
> * 原文地址：[Practical Guide to SQL Transaction Isolation](https://begriffs.com/posts/2017-08-01-practical-guide-sql-isolation.html)
> * 原文作者：[Joe Nelson](http://github.com/begriffs)
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 本文永久链接：[https://github.com/xitu/gold-miner/blob/master/TODO/practical-guide-sql-isolation.md](https://github.com/xitu/gold-miner/blob/master/TODO/practical-guide-sql-isolation.md)
> * 译者：[sigoden](https://github.com/sigoden)
> * 校对者：[mnikn](https://github.com/mnikn), [tmpbook](https://github.com/tmpbook)

# SQL 事务隔离实用指南

你可能已经在你的数据库文档中看到过隔离级别这一个概念，虽然感到有点不安，但是并没有太放在心上。一些日常的例子中使用到的事务本质上是隔离。大多数人使用数据库的默认隔离级别，并期望得到最好结果。隔离级别是一个必须要理解的基本概念，而且如果你花点时间学习这个指南，你会觉得生活更惬意。

我从学术论文中，从 PostgreSQL 文档中，在与同事就**什么是**隔离级别，**什么时候**使用它们能在保持应用程序的正确性的同时获得最大运行效率等问题答案的讨论中收集了本文需要的信息。

## 基本定义

为了正确理解 SQL 隔离级别，我们需要先思考事务本身。事务的概念来自于如下契约规则：合法交易必须具有原子性（所有条款都同时适用或同时失效），一致性（遵守法律协议），持久性（承诺后各方不能收回承诺）。这些性质就是数据库管理系统中众所周知的缩写词 ACID 中的 A，C 和 D。最后一个字母 I，意思是隔离，就是本文要重点讨论的了。

在数据库中而非法律意义中，事务是一组操作，将数据库从一个一致性状态转变到另一个一致状态。这意味着，如果所有的数据库一致性约束条件在执行事务前是满足的，那么在执行后仍然是满足的。

数据库能将这一思想更进一步，在每一条 SQL 数据变更语句中都强加约束吗？现有的 SQL 命令做不到。它们表达力不足以保证用户的每一步执行都保持一致性。举一个经典的例子，将一个银行帐户的钱转移到另一个账户这个过程中，在我们将钱从一个账户扣除之后，并把钱计入另一个账户之前，存在着一个暂时的不一致状态。因为这个原因，事务而不是语句被作为一致性的基本单位。

在这一观念之上，我们可以想象事务在数据库上连续运行着，并一直等待直到轮到它来独自处理数据的时候。在这个有序的世界中，数据库将从一个一致的状态移动到另一个一致的状态，中途会短暂地出现的不一致状态，但并不会造成有害的影响。

然而，串行事务这么乌托邦的事情在任何多用户数据库系统都几乎是不可行的。想象一下，一家航空公司的数据库因为一个用户预定航班而被锁定，导致**任何人**都无法访问。

值得庆幸的是完全串行执行事务通常是不必要的。许多事务不会对其它事务产生干扰，因为它们更新或读取的信息被完全隔离。同时运行此类事务（交错执行其命令）的最终结果与选择在另一个事务之前才运行这个事务没有什么区别。这种情况下的事务，我们称之为**可串行化**。

然而，并行执行事务确实有造成冲突的风险。没有数据库监督，事务会干扰彼此的工作数据，并运行在不正确的数据库状态中。这可能会导致查询结果不正确和违反约束。

现代数据库提供了一些方式自动地选择性地在一个事务中通过低延时或重试命令来避免干扰。数据库为了预防事务间的干涉提供了几种严格程度递增的模式，被称作隔离级别。更高等级的隔离级别在检测和处理冲突上更有效，但也更耗费资源。

并发事务提供了不同等级的隔离级别给开发者，开发者能够平衡并发量和吞吐量，由此来确定隔离等级。较低的隔离级别会提高事务并发量，但也增加了事务运行在某种不正确数据库状态中的风险。

我们首先要理解哪些并发交互会对应用所需的查询操作造成威胁，然后才能选择合适的隔离等级。正如我们将看到的，有时一个应用程序可以通过手动操作（如采取显示锁定）来降低它常规情况下需要的隔离级别。

在研究隔离级别之前，让我们先停下来看看“动物园”中圈养的事务问题。文献称这类问题为“事务现象”。

## 事务现象“动物园”

对于每一种现象，我们将深入探究它并发命令示意图，分析它为什么有问题，它在何种情况下可以被接受，以及它在什么情况下是我们为达到特定效果有意使用的。

我们将用一种速记符号表示事务 T1 和 T2 的执行。下面是一些例子：

- `r1[x]` —— T1 读取行 x 的值
- `w2[y]` —— T2 写入行 y 的值
- `c1` —— T1 提交
- `a2` —— T2 中断

### 脏写

事务 T1 修改条目，事务 T2 在事务 T1 提交或回滚前进一步修改。

![脏写示意图](https://begriffs.com/images/isolation-dirty-write.jpg)

#### 模型

![w1[x]…w2[x]…(c1 or a1)](https://begriffs.com/images/isolation-diagram-dw.png)

#### 危害

如果我们允许脏写，那么我们将不能确保一定可以回滚事务，想一下这种情况：

- { 数据库在状态 A }
- w1[x]
- { 数据库在状态 B }
- w2[x]
- { 数据库在状态 C }
- a1

我们应该回退到状态 A 吗？不，因为那样我们会失去 w2[x] 。所以我们应该保持在状态 C。如果 c2 发生那么一切就正常了。然而如果 a2 发生了会怎样？我们不能回退到状态 B，因为那样会丢弃 a1。但我们不能回退到状态 C，因为那样会丢弃 a2。归谬法可以论证。

因为脏写打破了事务的原子性，即使是在最低隔离级别，没有任何的关系数据库允许这些操作。通过抽象的方式考虑这个问题，是很具有启发性的。

脏写还会破坏一致性。例如，假设约束是 x=y。事务 T1 和 T2 单独执行都能保证约束，但是它们一起执行将违法约束。

- start, x = y = 0
- w1[x=1] … w2[x=2] … w2[y=2] … w1[y=1]
- now x = 2 ≠ 1 = y

#### 合理用法

在任何情况下，脏写的都是没有意义的，也不能提供便捷。因此，没有数据库允许它们。

### 脏读

一个事务读取了另一个未提交的并发事务写入的数据。（同上面的情景，未提交的数据被视为“脏”）。

![脏写示意图](https://begriffs.com/images/isolation-dirty-read.jpg)

#### 模型

![w1[x]…w2[x]…(c1 or a1)](https://begriffs.com/images/isolation-diagram-dr.png)

#### 危害

假设 T1 修改行后，T2 读取了它，接着 T1 回滚了。现在 T2 就持有了“不存在"的一行数据。基于不存在的数据对未来做决策是不正确的。

脏读，也为违反约束大开方便之门。假设存在约束 x=y。接着假设 T1 同时将 x 和 y 的值增加 100，T2 同时将值翻倍。任何一个事务单独执行时都能保证 x=y。但脏读 w1[x += 100], w2[x \*= 2], w2[y \*= 2], w1[y += 100] 违反了约束。

最后，即使没有对并发事务进行回滚，在另一个事务进行中间操作时启动的事务也会因为脏读从而造成数据库状态不一致。我们希望事务启动时处于一个一致的状态。

#### 合理用法

当一个事务需要追踪另一个事务时，脏读是有用的，例如调试和进度监控。也比如，当有一个事务在插入数据时再开一个事务反复运行 COUNT(\*) 以获取插入速度／进度，但这也仅适用于脏读不产生危害时。

此外，脏读这种现象不会发生在对早已不再变动的历史信息进行查询的时候。没有新的写入就不会产生问题。

### 不可重复读，不对称读

事务读取它先前已读取过的数据时，发现它已经被另一个事务更改了（在初次读操作之后有发生提交）。

注意，这不同于脏读，因为另外的事务进行过提交。此外这种现象需要两次读取才会显现。

![不可重复读示意图](https://begriffs.com/images/isolation-non-repeatable.jpg)

#### 模型

![r1[x]…w2[x]…c2…r1[x]…c1](https://begriffs.com/images/isolation-diagram-nrr.png)

上面的过程涉及到两个值时称作不对称读：

![r1[x]…w2[x]…w2[y]…c2…r1[y]…(c1 or a1)](https://begriffs.com/images/isolation-diagram-rs.png)

不可重复读是一种特殊形式的读倾斜：b=a

#### 危害

如同脏读，不可重复读允许一个事务读取一个不一致的状态。它发生的方式稍微不同。假设存在约束 x=y。

- start, x = y = 0
- r1[x] … w2[x=1] … w2[y=1] … c2 … r1[y]
- 从 T1 的视角看 x = 0 ≠ 1 = y

T1 至始至终没有读取任何脏数据，但读取过程中 T2 插入，改变一些值，并在 T1 再次读取前进行了提交。注意这个违规操作甚至不要求 T1 重新读取相同的值。

不对称读可能造成两个相关元素之间的约束被破坏。例如，假设存在约束 x+y > 0，且：

- start, x = y = 50
- r1[x] … r1[y] … r2[x] … r2[y] … w1[y=-40] … w2[x=-40] … c1 … c2
- T1 和 T2 各自观察到 x+y=10，但它们一起提交后导致 x y 的和为 -80。

另一个涉及到两个值的违法约束的情况出现在外键和其目标之间。不对称读会让它们混乱。例如，T1 从一个与表 B 相关联的表 A 中读取了一行，但是 T2 从表 B 中删除了该行并进行了提交，这造成表 A 觉得行仍存于表 B 但却无法读取到它。

当备份数据库的同时运行事务将是灾难性的，因为观察到的状态可能不一致，将造成无法执行还原。

#### 合理用法

非可重复读允许访问最新提交的数据。这可能在对大数据（或经常重复数据）进行聚合报告时有用，因为它们可以容忍读操作时短暂地违反约束。

### 幻读

事务再次执行返回一组满足搜索条件的行的查询时，发现满足该条件的行的集合由于另一个刚刚提交的事务而发生了更改。

幻读类似于不可重复读，但幻读发生的条件是其匹配查询条件的集合改变了，而不是单条数据。

![幻读示意图](https://begriffs.com/images/isolation-phantom-read.jpg)

#### 模型

![r1[P]…w2[y in P]…c2…r1[P]](https://begriffs.com/images/isolation-diagram-pr.png)

#### 危害

有一种情况是，当一个表包含代表资源分配的行（如雇员和他们的工资）时，其中一个事务作为“调控者”会增加每行代表的资源，而另一个事务会插入新行。幻读会包含新行，使调控者预算超标。

再举一个相关例子。考虑这样一个约束：它要求一系列工作任务排单后总时长不能超过 8 小时。T1 读取了排单，发现总时长只有 7 个小时，于是它添加了一个时长 1 小时的新任务，同时并发事务 T2 也做了同样的事情。

#### 合理用法

分页查询结果中的新返回页面包含新添加条目就很合适。同样，插入或删除项目后用户翻页时商品条目能自动调整。

### 更新丢失

T1 读取了一条数据，同时 T2 更新了这条数据。T1 根据读取的内容也更新了这条数据，然后提交。T2 进行的更新丢失了。

![更新丢失示意图](https://begriffs.com/images/isolation-lost-update.jpg)

#### 模型

![r1[x]…w2[x]…w1[x]…c1](https://begriffs.com/images/isolation-diagram-lu.png)

#### 危害

在某些方面，这几乎感觉不到异常。这并不会违反数据库约束，因为更新丢失只是造成一些工作没有提交而已。这种情况与应用程序连续对同一个值进行两次提交类似。

然而，这毕竟是一个异常，任何其他事务都没有机会看到更新，而且 T2 的提交行为变得像回滚一样。但一批命令串行执行时**有些命令**可能观察到变化，至少它们在检查值的时候可以。

在真实世界里，应用程序在执行读和写操作时，丢失更新会造成特别恶劣的影响。

例如，同时有两人试图购买某个活动剩下的最后一张入场券，这触发了两个事务，事务读取到还剩下一张未卖出的票。应用程序在单独线程中生成可打印票据并将其加入邮件队列，同时修改剩余票数为 0。在两个更新同时完成后，剩余票数为 0，这是正确的。然而有一个客户收到的邮件中的票据是重复的。

最后，请注意，当应用程序（通常通过 ORM）更新行中的所有列，而不仅仅是那些自读取后才更改的列时，丢失更新的风险会增加。

#### 合理用法

在像 `UPDATE foo SET bar = bar + 1 WHERE id = 123;` 这样的原子读取并更新语句中，更新丢失是不会发生的，因为其它事务不能在 bar 的值读取和更新之间执行写操作。这种现象发生在应用程序读取条目，内部对它进行计算，接着写入新值的过程中间。我们之后会深入分析。

有时候应用程序在历史更新中丢失一些值是可以接受的。传感器频繁的覆盖它通过多线程度量到的值，我们也只需要读取它最近记录的有意义的值。这种情况下，虽然略有做作，但可以容忍更新丢失。

### 不对称写

两个并发事务读取对方正在写入的数据集来确定它们写入的内容。

![不对称写示意图](https://begriffs.com/images/isolation-write-skew.jpg)

#### 模型

![r1[x]…r2[y]…w1[y]…w2[x]…(c1 and c2 occur)](https://begriffs.com/images/isolation-diagram-ws.png)

注意，如果 b=a 那么上述情况就变成了更新丢失。

#### 危害

不对称写造成事务历史不可序列化。回想一下，这意味着一个接着一个运行事务得到的结果没有办法与交错运行时相同。

我见过的最明显的例子是黑白行。照搬 PostgreSQL 维基文档：有下面一种情况，一些行包含一个颜色列，它的值或是“黑”或是“白”。有两个用户同时试图将所有行的颜色变得一致，但是它们尝试的方向却是反的。一个用户试图将所有行的颜色变为黑色，另一个用户试图将所有行的颜色变为白色。

如果这些更新是串行执行的，所有的颜色最终会变得一致。然而如果没有任何数据库保护措施，交错更新将简单的相互逆转，留下一堆混合的颜色。

不对称写也会打破约束。假设我们要求 x + y ≥ 0。且

- start, x = y = 100
- r1[x] … r1[y] … r2[x] … r2[y] … w1[y=-y] … w2[x=-x]
- now x+y = -200

两个事务都读到 x 和 y 的值是 100，所以对单个事务来说将某个值变为负数是可以的，得到的和仍然是非负数。然而它们同时将值变为负导致 x+y=-200，这违反了约束。想要感性的理解的话可以类比银行账户，银行账户的账户收支可以为负数，只要总的余额保持非负数。

### 只读串行异常

事务可以看到更新了的用来指示批处理已完成的控制记录，但未看到其中一个记录着批处理逻辑部分的详细记录，因为它读取的是早期的控制记录修订。

前面列举的异常只需要两个并发事务就能产生，但是这个需要是三个。它在 2004 年被发现后就一直引人注意，因为它揭示了快照隔离级别（稍后讨论）的缺陷，且它是唯一一个在不执行写入的三个事务的执行中表现出来的异常。

![只读异常示意图](https://begriffs.com/images/isolation-read-only-anom.jpg)

#### 模型

事务竞争进行如下三件事，

- T1: 为当前批处理生成报告
- T2: 为当前批处理添加新的任务
- T3: 将新的批处理激活成“当前”

![r2[b]…w3[b++]…r1[b]…r1[S_b]…w2[s in S_b]](https://begriffs.com/images/isolation-diagram-ro.png)

#### 危害

历史证明上述异常不可串行化。顺序执行事务带来不变性，即在生成报告的事务显示了特定批次的总数之后，后续事务不能更改总数。

数据库一致性保持完好，这种异常，仅导致报告的结论是不正确的。

#### 合理用法

鉴于直到 2004 年才有人注意到这种现象，它不太可能像其它现象那样容易引发问题。尽管它在任何时候都不该出现，但它也不是很严重。

### 其它？

我们已经罗列了所有可能出现的事务异常现象吗？这很难知道；ANSI SQL-92 标准表示它们已经列出了所有异常：脏读，不可重复读，幻读。直到 1995 年，贝伦森等人才发现其他串行异常，只读异常直到 2004 年被指出。

第一个关系数据库使用锁来管理并发。SQL 标准用事务现象而不是锁来描述问题，它允许基于非锁的策略来实现标准。然而，标准作者未能发现其他异常的原因是因为他们发现的三个异常都是“伪装的锁”。

我不知道是否还有更多的没有列出的事务异常现象，但似乎很有可能有。现在有众多论文在研究可串行性本身的性质，因为它看起来像理论基础。

## 隔离级别

商业数据库通过一系列隔离级别实现并发控制，这些隔离级别实际上是受控的反串行。应用程序为了获得较高的性能通常选择较低的隔离级别。高的隔离级别意味着更好的事务执行效率和更短的事务平均响应时间。

如果你理解了上一节中“动物园”中的并发问题，那么你也充分地睿智地理解了如何为应用程序选择正确的隔离级别了。这里需要深入理解的不是隔离级别**如何**防止了异常现象，而是隔离级别阻止了**什么**异常现象。

![隔离级别节点图](https://begriffs.com/images/isolation-levels.png)

在最顶部，串行化时任何异常现象都不会发生。随着箭头，阻止标记着的异常发生的保护逻辑被移除。

蓝色的三个节点表示的隔离级别被 PostgreSQL 提供。令人费劲的地方是 SQL 规范提供的隔离级别数不足，PostgreSQL 将这些规范中的定义的隔离级别映射到它实际支持的隔离级别。

| 你需要的 | 你得到的 |
| --- | --- |
| 串行化 | 串行化 |
| 可重复读 | 快照隔离 |
| 读已提交 | 提已提交 |
| 读未提交 | 读已提交 |

例如：

> BEGINISOLATIONLEVEL REPEATABLE READ;
>
> -- 现在我们进入快照隔离

读已提交是默认的隔离级别，现在想象一下，如果你现有的应用程序没有采取预防措施，你可能遇到的并发问题。

### 乐观 vs 悲观

正如前面所提到的，我们不必深入了解每一个 PostgreSQL 隔离级别可以防止哪些并发现象，但是我们需要了解两种一般性方法：乐观和悲观并发控制。这是因为每一种方法对应不同的应用程序设计技术要求。

悲观并发控制将对数据库行进行锁定，以强制事务等待其执行读写操作的时机。因为它总是需要时间来获取和释放锁，沮丧地假设会有冲突，所以叫做“悲观”。

乐观控制不会占用锁，它只是为每个事务生成单独的数据库当前状态快照，观察可能发生的冲突。如果一个事务干扰了另一个事务，数据库将阻止造成干扰的事务并清除它完成的作业。这种方式是有效的，因为干扰其实很少见。

遇到冲突的数量取决于以下几个因素：

- 对单个行的竞争。如果试图更新同一行的事务的数量增加，造成冲突的可能性会变大。
- 隔离级别为不可重复读时读取了多行。读的行越多，并发事务可能更新同样的行的机会越大。
- 隔离级别在阻止幻读级别时的扫描范围尺寸。扫描的范围越多，并发事务遭遇幻行的可能性越大。

在 PostgreSQL 中，有两种使用乐观并发控制的隔离级别：可重复读（实际就是快照隔离）和可串行化。**这些隔离级别并不是万能药，撒在不安全的应用程序上，就能解决所有的问题。**使用它们需要修改应用逻辑。

在构建一个与使用隔离级别由乐观并发控制的 PostgreSQL 交互的应用程序时必须小心。要知道任何变更在提交前都是不确定的，所有作业在一瞬时都可能被抹除。应用程序必须时刻准备着，如果检查到查询返回错误 40001 （代表 `serialization_failure`），就要重新执行事务。通用，应用程序在这种事务中不应该执行不可逆的真实操作。应用程序必须使用悲观锁来包含这种行为，或者在收到成功提交的结果后再执行操作。

你可能觉得可以在一个 PL/pgSQL 函数中缓存串行化异常并执行重试，可惜重试不能在那儿执行。整个函数运行在一个事务**内部**, 在调用前就失去了对执行的控制。不幸的是在提交的时刻发生串行错误的可能性最大，而对于函数来说，已经来不及进行捕捉了。

重试必须由数据库客户端进行控制。许多语言提供了帮助函数来处理类似任务。这儿列举了一些。

- Haskell: [hasql-transaction](https://hackage.haskell.org/package/hasql-transaction) 自动重试并且在禁止任何不可重复的副作用的 Monad 下运行事务
- Python: [psycopg2 how to retry](https://www.slideshare.net/petereisentraut/programming-with-python-and-postgresql)
- Ruby: [auto-retrying in sequel](https://github.com/jeremyevans/sequel/blob/master/doc/transactions.rdoc#automatically-restarting-transactions) or the [transaction_retry gem](https://github.com/qertoip/transaction_retry)

因为重新生成事务很浪费，所以最好在有限的时间内存储事务已避免作业丢失。

### 低隔离级别补偿

一般来说，最好使用合适的隔离级别，以避免异常和查询的扰乱。最好让数据库做它最擅长的。然而，如果你确定某些异常不会发生在您的使用场景，你可以选择使用一个包含悲观锁的低隔离级别。

例如我们可以在读和更新之间添加一个锁来避免读提交事务的更新丢失。这只需要我们在选择类语句中添加 "For Update"。

    BEGIN;

    SELECT *
      FROM player
     WHERE id = 42
      FOR UPDATE;

    -- 一些游戏逻辑

    UPDATE player
      SET score = 853
    WHERE id = 42;

    COMMIT;

任何试图选择同样的行进行更新的事务都会阻塞，直到第一个事务完成操作为止。这个使用选择的更新技巧甚至可以在串行事务中被用来避免串行错误导致的重试，特别是你本打算在应用程序中采取非幂操作时。

最后，你可以冒着计算不准的风险使用较低的隔离级别。快照隔离级别被采用的一个主要原因是它比串行有更好的性能，同时还避免了大部分串行化能避免的并发异常。如果在你的场景中不会发生不对称读，你可以降低隔离级别使用快照。

## 引用的源和进一步阅读

感谢那些为我写的这篇文章提供建议的人。

- 在 Freenode IRC 频道 #postgresql 上：Andrew Gierth (RhodiumToad) and Vik Fearing (xocolatl)
- 私人对话：Marco Slot，Andres Freund，Samay Sharma，和来自 [Citus Data](https://www.citusdata.com) 的 Daniel Farina

进一步的阅读

- [Joe Celko’s SQL for Smarties](http://shop.oreilly.com/product/9780128007617.do), chapter 2
- [A Critique of ANSI SQL Isolation Levels](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-95-51.pdf)
- [Transaction Isolation](https://www.postgresql.org/docs/current/static/transaction-iso.html) in PostgreSQL docs
- [A Read-Only Transaction Anomaly Under Snapshot Isolation](http://www.cs.umb.edu/~poneil/ROAnom.pdf)
- [Serializable Snapshot Isolation in PostgreSQL](http://vldb.org/pvldb/vol5/p1850_danrkports_vldb2012.pdf)
- [Data Consistency Checks at the Application Level](https://www.postgresql.org/docs/current/static/applevel-consistency.html) in the PostgreSQL docs
- [The Transaction Concept: Virtues and Limitations](http://jimgray.azurewebsites.net/papers/thetransactionconcept.pdf)


---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[React](https://github.com/xitu/gold-miner#react)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计) 等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)、[官方微博](http://weibo.com/juejinfanyi)、[知乎专栏](https://zhuanlan.zhihu.com/juejinfanyi)。
