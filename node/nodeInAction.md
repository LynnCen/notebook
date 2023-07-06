# node in action 

## 前言 

写一本关于nodejs的书籍是一项很有挑战性的工作。

## Part1 Node fundamentals（node 基本原理）

## Welcome to Node.js 

这个章节覆盖以下内容：
1. node.js是什么 
2. 服务器上的javascript 
3. Node 的异步和事件性质
4. Node 设计用于的应用程序类型
5. Node示例程序

所以node.js是什么？它可能是你已经听到过的术语。或许已经使用node。也许你对此感到好奇。目前，Node 非常流行，年轻（2009年首次亮相）。这是 GitHub 上观看次数第二多的项目，它在 Google 群组中拥有相当多的追随者(http://groups.google.com/group/nodejs) 和 IRC 频道 (http://webchat.freenode.net/?channels=node.js)，并且拥有超过 15,000 个社区 mod-规则发布在 NPM（包管理器）(http://npmjs.org) 中。这一切都是为了说，这个平台背后有相当大的吸引力。

瑞安·达尔 (Ryan Dahl) 谈 Node：你可以在JSCONF Berlin 2009网站上观看Node的第一次演示，演讲者是创建者Ryan Dahl。链接是http://jsconf.eu/2009/video_nodejs_by_ryan_dahl.html。

官方网站（http://www.nodejs.org）将Node定义为“基于Chrome的JavaScript运行时平台，可轻松构建快速、可扩展的网络应用。Node.js使用事件驱动、非阻塞I/O模型，使其轻巧高效，非常适合跨分布式设备运行的数据密集型实时应用。”
在本章中，我们将探讨以下概念：
1. 为什么 JavaScript 对于服务器端开发很重要
2. 浏览器如何使用JavaScript处理I/O
3. Node在服务器端如何处理I/O
4. DIRTy应用程序的含义以及它们为什么适合Node
5. 一些基本Node程序的样例
6. 让我们先关注JavaScript...

**基于 JavaScript 构建**

无论好坏，JavaScript 都是世界上最流行的编程语言。如果你做过任何网络编程，这是不可避免的。 JavaScript，因为网络的纯粹覆盖范围，实现了 Java 的“一次编写，随处运行”的梦想
早在20世纪90年代。
2005 年 Ajax 革命前后，JavaScript 从一个“玩具”语言变成了一个人们用语言来编写真实且重要的程序。 一些值得注意的第一个是 Google 地图和 Gmail，并且今天有许多网络应用程序从 Twitter 到 Facebook 再到 GitHub。
自 2008 年底发布 Google Chrome 以来，由于浏览器供应商（Mozilla、Microsoft、Apple、Opera 和 Google）之间的激烈竞争，改进速度令人难以置信，JavaScript 性能已大幅提升。 这些现代的表现JavaScript 虚拟机正在真正改变您可以构建的应用程序类型。一个引人注目且坦率地说令人兴奋的例子是 jslinux，一台 PC在 JavaScript 中运行的模拟器，您可以在其中加载 Linux 内核，与终端会话，并编译 C 程序，所有这些都在您的浏览器中进行。
Node 使用 V8，即为 Google Chrome 提供支持的虚拟机，用于服务器端编程。 V8 为 Node 带来了巨大的性能提升，因为它省去了中间人，比起执行，更喜欢直接编译为本机机器代码字节码或使用解释器。 由于 Node 在服务器上使用 JavaScript，因此还有其他好处：
1. 开发人员可以用一种语言编写 Web 应用程序，这有助于减少在客户端和服务器开发之间进行上下文切换，并允许客户端和服务器之间的代码共享，例如表单重复使用相同的代码验证或游戏逻辑
2. JSON 是当今非常流行的数据交换格式，并且是 JavaScript 原生的
3. JavaScript 是各种 NoSQL 数据库（例如 CouchDB和 MongoDB），因此与它们交互是很自然的事情（例如，MongoDB 的 shell 和查询语言是 JavaScript； CouchDB的map/reduce是JavaScript）。
4. JavaScript 是一个编译目标，并且已经有许多语言可以编译它。
5. Node 使用一台符合 ECMAScript 标准的虚拟机 (V8)。5 换句话说，您不必等待所有浏览器都赶上来在 Node 中使用新的 JavaScript 语言功能

谁知道 JavaScript 最终会成为一种引人注目的用于编写服务器端应用程序的语言？ 然而，由于其广泛的覆盖范围、性能和其他特征前面提到，Node 已经获得了很大的关注。 JavaScript 只是这个难题的一小部分； Node 使用 JavaScript 的方式更加引人注目。 为了了解 Node 环境，让我们深入了解您最熟悉的 JavaScript 环境：浏览器。