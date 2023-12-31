> * 原文地址：[Create A Real-Time Medical App With React Native](https://medium.com/javascript-in-plain-english/create-a-real-time-medical-app-with-react-native-637618b7883f)
> * 原文作者：[Sophia Martin](https://medium.com/@sophiamartin121)
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 本文永久链接：[https://github.com/xitu/gold-miner/blob/master/article/2022/Create-A-Real-Time-Medical-App-With-React-Native.md](https://github.com/xitu/gold-miner/blob/master/article/2022/Create-A-Real-Time-Medical-App-With-React-Native.md)
> * 译者：[Badd](https://juejin.cn/user/1134351730353207)
> * 校对者：[chaxus](https://github.com/chaxus)

# 用 React Native 创建实时医疗应用

![](https://miro.medium.com/max/1280/1*_IpH2_91cvm-PQfSljRyaQ.png)

如今的世界，是科技驱动的世界，人们已经养成了用移动应用来解决大部分问题的习惯。在医疗保健领域也不例外。此外，随着慢性健康问题的日益普遍，以及可穿戴设备的日益普及，毫无疑问，健康科技行业正在蓬勃发展，并为医疗保健应用创造了美好的未来。

“[统计数据表明](https://www.statista.com/topics/2263/mhealth/#dossierKeyfigures)，2020 年移动端医疗保健的全球市场规模为 13.9 亿美元，预计将在 2025 年激增到 38 亿美元。”

所有这些因素都使移动应用行业成为医疗服务提供商的一个有利可图的机会，并鼓励投资者和企业家关注医疗科技应用的开发解决方案。

在开发医疗保健移动应用时，医疗服务的质量可能会是你扭转乾坤的关键。应用的质量和性能与医疗保健技术直接相关。

开发健康技术应用的目的，是管理用户的诊所数据、简化预约流程、接入大量医疗保健专家、与各种设备同步等等，因此，许多中小企业和商业投资者仍然没有入局.无论是初创公司还是行业龙头，应用程序开发成本和时间都是投资者最关心的问题。

而这正是接近原生体验的高质量跨平台应用开发解决方案的用武之地。在众多框架和技术选择中，React Native 是构建健康技术混合应用的跨平台框架的优质选择，可节省高达 30% 至 40% 的开发成本。

许多人都很好奇 React Native 是如何降低医疗应用的开发成本的。但在我们深入细节之前，先来了解一下本文的核心观点：

- 健康科技行业：为什么移动端健康应用是 2022 年投资者的大好机会？
  - 医疗保健行业市场统计及未来展望
- 用 React Native 可以开发的医疗保健应用的类型
  - 选择最适合业务需求的应用类型
- 用 React Native 开发医疗应用
  - 实现实时性的关键开发阶段
- 如何为健康应用开发增加价值？
  - 要在应用程序中添加的特性和功能
- 用 React Native 开发医疗保健应用需要多少成本？
- 结论：准备好启动实时医疗应用

下面让我们直接深入细节，以便更好地理解重点。

## **健康科技行业：为什么移动端健康应用是 2022 年投资者的大好机会？**

新兴技术和创新解决方案正在迅速取代传统医疗服务。近年来，移动应用已成为各种投资者和企业的大好机会。但是，如果你想知道医疗移动应用如何为企业主增加价值，那么可以快速了解以下几个优点：

- 能帮助企业主提高客户忠诚度，并提供有价值的服务体验。
- 时过境迁，人们更愿意使用移动应用，因为能更方便地获取医疗保健信息和服务。
- 能通过推送通知、促销新闻、特别优惠、预约提醒等方式与客户建立持久的关系。
- 是收集客户信息的最简单方法。
- 使你能够吸引更多客户，而且有多种与客户建立联系的便捷方式。

这些是医疗移动应用需求激增的几个原因。如果你还是没有被说服，那暂且不必相信我们的话，先来看看健康科技市场的宏观图景。

> **健康科技行业市场洞察及未来展望**

- 根据调查报告，健康科技市场预计将达到 6000 亿美元的规模，复合年增长率（CAGR）为 25%。
- 最近的研究表明，健康科技行业的体量预计将增长 3.7 倍，到 2020 年将超过 210 亿美元。
- 最新的健康趋势显示，在不久的将来，预计 38% 的欧洲电子健康行业从业者将会重点关注患者的健康数据。

尽管如此，根据应用程序的使用情况，大多数医疗保健应用的下载量不到 10000 次。但是你可以试试雇佣[**应用程序开发公司**](https://www.xicom.biz/services/mobile-app-development/)在应用程序中轻松地实现一些最佳特性和功能，将业务推向新的高度。但问题在于，你能选择哪种类型的医疗援助应用来开发？

下面会给出一个快速上手指南。

许多人肯定对进入移动应用开发行业感到兴奋，但还不确定哪种类型的应用适合自己的业务。为了让应用开发更轻松，我们列出了三大类别中的几种应用类型，以帮助你了解，哪种应用类型是最适合你的。

> **如果你的目标客户是专业的医护人员，你的应用可以支持下列功能**

- 样本采集和远程诊断
- 在线监测和跟踪医疗疾病
- 预约管理
- 诊所管理和预约调度
- 远程医疗
- 提供有关药物、医学文章或健康问题症状信息的医学教育

但是，如果你的目标客户是患者，或者说想要通过应用解决他们的健康问题，需要哪些功能呢？

如果是这样，以下是你可选的类型

- 客制化健身和健康服务
- 在线预约安排
- 心理健康和冥想
- 自我健康跟踪和监控
- 客制化养老服务
- 健康管理和远程医疗
- 怀孕追踪

无论你需要哪种应用程序，要想使其完美、稳定地运行，你都需要选择合适的技术和[**软件开发公司**](https://www.xicom.biz/)，来保证应用的良好运转。在这种情况下，选用 React Native 来开发一个健康应用，你不会后悔。

接着让我们了解一下如何使用 React Native 开发健康应用。

如果你最终决定开发一个健康应用，你要明白，医疗保健行业在过去两年中已经发展壮大。由于 Covid-19 的严重打击，来自世界各地的医疗保健投资者已经开始投资开发基本的医疗应用了，因此，核心问题是，你应该开发哪种类型的应用程序，以及如何在竞争中脱颖而出？

“试想一下，要是推出一款能让员工/医生/患者之间进行文字/语音/视频聊天的医疗应用，会有怎样的反响？除了实现这些功能外，一些客户还会要求用 React Native 和 WebRTC 实现蓝牙体重测量功能。”

此外，像测量血压、患者健康数据跟踪之类的功能，都在移动应用中得以实现。

曾经有客户问我们，为什么要选择 React Native？

下面就是要开发且要用 React Native 开发医疗移动应用的原因：

- 选择 React Native 开发医疗应用的最大好处，是它能轻松集成蓝牙功能。
- 用 React Native 能够快速地开发出实时聊天功能及至整个应用。
- 它允许开发者用通用的原生 UX 设计工具来构建强大的解决方案。

那么，就让我们跳到应用开发的主要部分，了解 React Native 是如何在医疗应用中实现实时聊天的。

## **开发 React Native 医疗应用：在应用中实现实时聊天和 WebRTC**

为了集成实时视频/语音聊天功能，我们要用到 WebRTC 技术。许多人会问，我们为什么要选 WebRTC 技术？

对于这个问题，可以看一下它的优点：

- 在应用中添加实时聊天功能时，开发者最关心的，是在实时模式下建立安全的点对点音频/视频连接。WebRTC 技术为应用赋予了安全连接的能力。
- WebRTC 实时通信技术是开源技术，也更易用。
- 要运行实时的文字/视频/音频通信、文件共享/传输、屏幕共享、网络摄像头/麦克风调用，只要有了 WebRTC ，就无需再安装内部/外部插件或任何其他附加工具。

**第 1 步：用本地流传输视频/音频数据**

万丈高楼的第一块砖，是用本地流将数据（视频/音频）流畅地传输到远程设备。在使用 WebRTC 技术时，可以参考下面的代码。

![](https://miro.medium.com/max/1276/1*diiOoVempsoySdSYRxqd2g.jpeg)

**第 2 步：建立安全的点对点私有连接**

向应用中添加实时功能，如站内消息（无论是音频还是视频），是[**应用开发公司**](https://www.xicom.biz/services/mobile-app-development/)的主要要求之一。要在设备之间的建立安全的、私有的点对点连接，你可以按照以下命令进行操作：

![](https://miro.medium.com/max/1264/1*Ve_YFAT_EmIzjkPdWBkgww.jpeg)

**第 3 步：实现 WebRTC 协议**

给医疗应用添加实时通信是很值得的。但用正确的协议实现方式才能保证安全，这是开发者最复杂的任务之一。实现用文本通道发送消息，最简单的方法就是使用 WebRTC 协议。下面给出简单可行的示例代码：

![](https://miro.medium.com/max/842/1*vAavjV_0quwh6rGdwn2QWg.jpeg)

**第 4 步：用蓝牙集成体重测量功能**

在使用 React Native 开发高性能医疗应用时，另一个要点是通过蓝牙来实现体重测量功能，因此我们也写好了集成代码。你可以[**雇一位移动应用开发者**](https://www.xicom.biz/offerings/hire-mobile-developers/)来完成这项任务，这是你在转换不同重量的物品时面临的最大问题。专家们可以利用以下算法，用从体重秤接收的适当单位转换体重信息。

> 以下是可供参考的代码：

![](https://miro.medium.com/max/832/1*NqUsgLLu90OAcAot_HRpSg.jpeg)

**简而言之**：通过使用 WebRTC 实现实时聊天和蓝牙功能，你就可以搜索在线用户并进入聊天窗口。在聊天窗口中，你可以向用户发送短信、保持通话和进行视频通话，这些通信都是安全的点对点连接。

此外，使用 React Native 蓝牙集成模块，你可以将应用与可穿戴设备连接，并能够检测体重测量设备，体重信息会迅速显示在移动端屏幕上。因此，你就能够直接与在线医生共享数据，也可以将其复制并在单独的消息中发送。

以上就是在使用 React Native 框架开发时，你可以添加到自己的健康应用中的独特功能。但问题是，还有哪些功能需要添加？

开发健康应用，目的就是为了实现方便的健康检查，甚至无需去诊所。而且，在当今的快节奏生活中，没有什么比用应用来跟踪和监控健康记录更容易了。

如果你最终决定使用 React Native 开发健康应用，并计划与行业龙头竞争，那么你需要了解该领域的当前趋势，并提高自己的应用在市场上的知名度。

因此，要做出有实际效益的医疗保健应用，你的应用要有符合需求的特性和功能，以吸引用户并确保切实的收益。既然开发应用的主要目标是让患者的生活更轻松、减少去诊所的次数，那么它必须具备每个成功的医疗应用都具备的功能。

> 以下是可供参考的功能列表：

- **注册**：为了使用户的注册过程更快、更顺畅，请确保提供多种注册选项，包括电子邮件、电话号码和社交网络账号。确保注册过程简单易懂。
- **编辑详细信息**：允许用户添加、删除或编辑他们的个人详细信息，例如添加健康史（对于患者）、年龄、姓名、照片、资格和证书（对于医生）。
- **高级搜索选项**：允许用户根据各种筛选条件搜索医生，例如医生的经验、预约费用、评级、专业化等。
- **安排和管理预约**：要确保预约安排流程对用户和医生来说透明、简单和容易。借助应用内日历，用户可以轻松查看不同日期下医生是否有空，并能够在一分钟内进行预约。另一方面，医生通过查看当天的预约安排，可以查看工作日历并管理工作量。
- **语音和视频通话**：应用内实时通信已成为医疗保健应用的重要功能。你可以雇佣应用开发公司来支持发送短信、保持通话、进行视频通话等功能，这样就能帮医生和患者保持实时、不间断的通信体验。
- **接入多种支付方式**：为了让拓宽应用场景，你需要接入各种第三方支付，让医生能通过各种平台收款。此外，为确保出色的用户体验，应该支持 PayPal、Stripe 等支付网关，以及 Visa、MasterCard 等信用卡。
- **评分和评论**：根据经验，最好能让用户向服务提供商分享反馈、给出评分。
- **分享处方**：这可能是健康应用里最重要的功能，它允许用户上传处方，并能够查看附近基本药物的供应情况。
- **分享健康进度报告**：增加与可穿戴设备连接的灵活性，让用户持续监控自己的健康进展，并让他们能与医生或社交媒体平台分享锻炼结果。
- **症状诊断**：随着健康问题的增加，将这种功能添加到应用中非常有用。用户可以跟踪症状并查出是什么问题。

你可以将更多功能集成到健康应用中。使用 React Native 开发医疗保健应用的最大好处是可以节省大量的开发时间。

那么让我们来了解一下使用 React Native 开发一个医疗应用需要多少成本。

**使用 React Native 开发医疗保健应用需要多少成本？**

如果不知道应用的结构，就不能贸然估算开发一个基于 React Native 的优秀健康应用的成本。

由于每个企业都有不同的需求，并且有不同的目标受众，因为，没有适合所有需求的应用。你需要定制应用的规格以满足业务需求，并且应用结构上的每一点差异都会放大体现在开发成本和时间上。

就算你知道了影响开发成本的因素有哪些，估算应用开发成本仍然很难做到精细准确。因此，程序复杂性、操作平台、特性和功能、UI/UX 设计、测试、后端、前端开发以及开发团队实力等重要因素会极大地影响整体开发成本。

如果我们考虑所有这些因素，然后计算开发一个具有所有这些特性和功能的健康应用的平均成本，那么每个平台（Android/iOS）的成本将在 20000 美元到 35000 美元之间，具体取决于应用的复杂度。

但是，在使用 React Native 开发时，你可以轻松节省高达 40% 的成本，并且应用可以用于两个平台。

在得出任何结论之前，你最好与专家讨论一下，并根据业务需求进行估算。

使用 React Native 开发一个有实际效益的健康应用，不仅可以满足市场不断增长的需求，还可以为用户带来价值，这不是一件容易的事。虽然它涉及很多开发步骤、特性和功能等等，但如果你选择聘请[**应用开发公司**](https://www.xicom.biz/services/mobile-app-development/)使用 React Native 定制应用功能，这就容易得多了。

要做出成功的医疗保健应用，你需要找一位对医疗保健行业有深刻了解并具备出色开发技能的专家，来打造出超越一流质量的解决方案。

> 如果发现译文存在错误或其他需要改进的地方，欢迎到 [掘金翻译计划](https://github.com/xitu/gold-miner) 对译文进行修改并 PR，也可获得相应奖励积分。文章开头的 **本文永久链接** 即为本文在 GitHub 上的 MarkDown 链接。

---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[区块链](https://github.com/xitu/gold-miner#区块链)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计)、[人工智能](https://github.com/xitu/gold-miner#人工智能)等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)、[官方微博](http://weibo.com/juejinfanyi)、[知乎专栏](https://zhuanlan.zhihu.com/juejinfanyi)。
