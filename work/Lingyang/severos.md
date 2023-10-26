# 原理补充

ServOS 原理

2022.07.04

在了解这个框架之前，需要大家先了解下 RequireJS 和 Inversify 两个库的基本使用，知道的同学可以直接跳过这两部分。

这里来写一个最小可用的 ServOS 框架来帮助大家来理解下原理，这里就叫它 SOS 把。。。 源码[在这](http://gitlab.alibaba-inc.com/wb-wzs728893/SOS)，感兴趣的同学可以拉下来跑一下。

RequireJS

[RequireJS](https://requirejs.org/) 是一个 amd 模块加载器，模块的定义通过 define 方法，模块的加载通过 require 方法。模块的定义是异步的，正如 amd 的全称 Asynchronous Module Definition 表示的一样，当你定义的模块所依赖的所有模块都被定义完成了，你的模块才算定义完成，才可以使用 require 方法加载你的模块。

比如，

在 SOS 框架里，我们可以定义两个函数来定义和加载一个模块，比如有那么个关于一个模块的 JSON 对象的描述，

我们就可以利用 RequireJS 来加载这个模块，通过往 RequireJS 的 paths 里加入一对 KVP，K 是模块名，V 是模块的 URL 地址，这样你就可以通过 require([K]) 的形式加载模块了，

使用如下，

Inversify

如果你知道这个控制反转/依赖注入的概念的话，可以直接跳过看下一小节。

它是一个 IOC（控制反转/依赖注入）的库，简单来说它提供用一个类实例管理多个类实例，以及类被初始化的时候帮你自动被注入一些你想要的其他实例的功能。

你可以初始化一个 Container 实例，然后通过 bind 方法往 Container 实例上绑定其他类，然后你可以通过 get 方法获取我们绑定的其他类的实例，

上面只是讲了一个容器实例管理一堆其他实例的例子，正常情况下一个 service id 只能绑定一个类，但是你可以给这个 id 加一个限制条件（constraint），也就是在 .to 方法调用完，再调用 .when 方法添加一个限制条件，这样你可以用 container.getAll(id) 的方法获取所有的类实例，这在 SOS 中可以用来做 Tab 类的插件。

当然我们也可以通过依赖注入的方式获取到类实例，

Inversify 就是这样通过一个 service id 绑定一个 service 的形式给你提供服务，默认它会帮你实例化这个 service，但是你也可以给这个 service id 绑定一个常量，使用 .toConstantValue 方法，而 service id 官方推荐使用 Symbol，详见[官方文档](https://github.com/inversify/InversifyJS)。

SOS

![](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/290311/1657266629699-0af3df3c-a73d-41aa-b46b-6d6eccec2736.png)

入口

首先来看下我们工作台页面的源码，核心部分就下面两行，

第一行就是我们的 g_config 文件，来看看里面都有啥，

上面两行就是加载了我们的 servos 模块，并用模块里导出的 BootstrapConfig Symbol 将 config 对象用该 Symbol 作为 key 加到我们的 window 对象上，所以，如果你想要在 3.0 工作台拿到我们的配置信息的话，这样就可以了，

仔细看下，你会发现它其实用的是 Symbol 而不是 Symbol.for 定义了这个变量，

SOS 初始化（bootstrap）

想看 Servos 源码的同学，可以直接 npm install @ali/servos-core 然后在 node_modules 里就可以看到了，几乎是没有做任何语法转换看起来也是非常直观的，里面的 decorator 的部分做了转换，使用的是 TypeScript 的 decorator，和 EcmaScript 提案中的 decorator 还是有点不同的，感兴趣的同学可以了解下。

---- 核心模块

接着上面，在执行完 g_config 文件之后，我们就能拿到一个 servos 的实例了，因为在导出的 servos 的 index.js 文件里，导出了 servos.js 文件，里面定义了 Servos 的类，定义完成后实例化了这个类并将其写到了 window 上，

我们有了 servos 实例之后，下一步就是 servos.bootstrap(); 进行相关服务的初始化了，index.js 里导出的 CoreModule.js 里导出了一个 ServosCoreModule 变量，里面定义了很多内置的服务，当 bootstrap 执行到 ServosCoreModule.registry(binding); 这一步的时候，servos 就开始往容器上绑定服务了，下面来讲下一些比较核心的服务吧，

●DefaultModuleService（模块的注册和加载）
●DefaultBundleService（注册 bundle 到 RequireJS config）
●DefaultLibraryService（加载一些需要 preload 的模块）
●DefaultExtensionService（将 extension 注册到容器上）

每一个服务绑定到容器上之后，一般还没什么用，当 bootstrap 执行到 CoreStartupTask 这个任务的时候，await serviceManager.startAllServices() 这里才开始真正做一些事情，它会遍历我们刚才注册的服务的实例身上的 start 方法，来看下上面这些服务的 start 方法都做了啥，

DefaultModuleService 这个类和我们第一小节里面声明的 ModuleService 这个类一样，你可以将其绑定到 container 上，然后使用对应的 service id 去获取这个类实例，如果你遇到需要注册模块或者加载模块的时候，参考下第二小节介绍 Inversify 的时候的例子。

DefaultBundleService 这个类会将 g_config.bundle.list 里的每一个 bundle 注册到 RequireJS 配置中去，bundle 的 manifest 的 packageName 作为模块名，URL 需要拼接，和我们在第一小节介绍 RequireJS 的时候注册模块的时候一样。

DefaultLibraryService 这个类会将 g_config.lib.list 里的每一个 lib 注册到 RequreJS 配置中去，并且设置了一个 callback，当对应的 lib 模块被加载的时候，同时加载该 lib 指定的资源列表，也就是 manifest 里的 resources 字段。如果 lib 需要预加载（manifest 里的 preload 字段）的话，那么这个时候会加载这个 lib 模块，并且加载其指定的 resources。

DefaultExtensionService 这个类会将 g_config.extension.list 里的每一项绑定到容器上，通过 bind(implements 字段).toDynamicValue(() => extension).when(request => request.target.matchesNamedTag(extension.fullName)) 这种带 constraint 的方式，因为我们的扩展（extension） 在工作台上都是 com.xixikf.workbench.Plugin 这个扩展点（service id），所以我们很容易就可以通过 servos.container.getAll(Symbol.for('com.xixikf.workbench.Plugin')) 的形式拿到所有的 extension 了（比如，信息查询，语音转文本这些 extension）。这里因为工作台里都是用 Symbol 作为 service id 的，所以这里 .getAll 的时候使用了 Symbol，然后我们就可以利用这些 extension 自己写一个插件容器了，不一定要是 Tabs 这种形式也行。注意一点，这里绑定的其实不是 extension 的类，只是绑定了个 manifest。

DefaultApplicationService 这个类会对 g_config.app.list 里的每一个 app 并不会做什么，如果你只是问绑定的话。不过这个类后面会用到。

到此为止基本上 g_config 里的所有 list 相关的数据都用到了，bundle，lib，app，extension。

---- 加载默认应用

接下来 bootstrap 进入到了加载默认应用 DefaultAppStartupTask 这一步了，

launchDefaultApplication 方法底层就是调用了 ModuleService 服务提供的 loadModule 方法来加载模块，默认的应用是在 g_config.app.default 这里指定的，默认一般是，com.xixikf.workbench.ShellApp，bundle 名是 com.xixikf.workbench，entry point 是 ShellApp，这样你就可以在 app list 里找到对应的 manifest 了，这样模块加载完你就可以拿到这个 entry point 对应的类了，然后通过用 manifest 构建出来的 fullName 作为 service id 将这个类绑定起来，fullName 的话，${manifest.bundleName}.${manifest.name || manifest.entryPoint} 长这样。然后再通过 container.get(fullName) 函数来获取我们的类实例，然后调用该实例身上 didLoad 等的一些生命周期函数，这样就拿到了一个实例了，一般就是一个 React 组件实例，下一步就是调用 this.reactRenderService.render(app, target); 进行组件渲染了。

this.reactRenderService 这个东西，其实就是 CoreModule 里的 DefaultReactRenderService 类实例，但是这个类依赖的是 Symbol.for('servos.core.rendering.ReactRenderer') 这个 service id 对应的服务。像这种和 UI 相关的肯定不是写在 servos 框架里的，那在哪里？

对，在 servos-ui 这个库里，当你加在默认应用的时候，也就是我们的工作台壳应用，它依赖 servos-ui 这个库，所以我们的壳应用被加载后我们 servos-ui 这个库也就被夹在了，这个库被加载后会在 Symbol.for('servos.core.rendering.ReactRenderer') 这个 service id 上绑定一个对象，一般将 ReactDOM 作为常量进行绑定就可以了，它身上有一个 render 方法。

不过它实际上就是干了下面这件事情，本质上一样，

在 Servos 里，如果你想要在 container 上绑定一些服务的话，你需要这样写，

注意两个点，一个是使用 ServosModule 这个类，还有一个是实例化的类要使用 ESM 的 default 默认导出。

Servos 在 ModuleService 实例化的时候，使用 RequireJS 的 onResourceLoad 函数，在每个模块加载完成的时候会去调用下该模块暴露出来的 default 方法，大致的实现其实就是这样，

这一块重点可以看下 RemoteApplication 这个类。

插件容器

对于 extension 这里补充几点，虽然你可以通过 servos.container.getAll(Symbol.for('com.xixikf.workbench.Plugin')) 这种方式获取到所有的 extension，然后遍历进行渲染，但是呢，工作台（workbench）提供的 Tabs 插件容器对 extension 进行了过滤（因为工作台有个插件管理的页面，坐席可以决定是否展示某个 extension 以及对 extension 进行排序），所以不会展示所有的 extension，你可以看下 TabsPluginContainer 组件，里面通过 usePlugins 这个 hooks，最终会调用 RuntimeInstalledPluginsByApp 这个接口，这个接口需要你传一个 appName 作为入参，比如热线（com.xixikf.hotline.desk.HotlineDeskApp），然后接口就会将你开发的所有的 extension 里 manifest 里的 supports 数组里有 com.xixikf.hotline.desk.HotlineDeskApp 这个值的 extension 全拉出来，

![](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/290311/1657266668801-0dec22bc-4612-4fb2-b3d5-19fda0de267a.png)

根据 app 支持的 slots 进行分类，这样使用插件容器的时候给一个 slot props 就能进一步的对上面的返回结果进行过滤了。

你可以在热线 app 这个仓库里看到这样的用法，

这个 PluginContainer 插件容器组件会根据传过来的 slot props 对 extension 进行二次过滤。

看到这里你应该对于扩展点和扩展有了个大概的理解，它们就是 IOC 框架里的 service id 和 service 的关系。

---- 其他

如果有讲的不对的或者不好的，欢迎留言区评论 👏

用例

1. 微服务框架

ServsOS 也支持 2.0 这样的微应用管理机制，首先加载的默认应用会作为一个和 yodajs 容器一样的壳应用，提供路由，状态管理等能力，并且默认对路由做了缓存。

同时因为 servos 在 bootstrap 之后会往 RequireJS 中添加配置，所以开发的插件，应用等也可以在打包的时候将 React，antd 等常用模块给 external 出去，Vite 里可以通过 rollupOptions 来进行 build 的配置。

TODO

1manifest 里的 allowMultiple，autoLoad 属性什么用，好像没在代码里看到，dynamic 看到了不过还不知道什么用。
2 为什么要使用这种设计模式，利好点在哪里，一开始使用的时候感觉理解成本挺高的。
3 在 [Inversify](https://github.com/inversify/InversifyJS/blob/772ea8ef53b17ac00a35df45d00bfc2f1ca53d07/wiki/symbols_as_id.md) 里，用 Symbol.for 和用字符串有啥区别，除非用 Symbol("xxx") 才不会造成命名冲突。但是我发现用 .to 绑定的时候，service id 用同一个字符串两次会报错，但是用 Symbol.for 就不会。
4 动态菜单 3.0。我记得将下面的配置给到后端就可以了添加一个菜单了，这个还没来得及看具体的实现。

**JavaScript**

复制代码

1

2

3

4

5

**// 将这个给了后端就行了**

**// 菜单加载的时候的就会加载我们的组件了**

**binding**.**registerExtensionImpl**(**SystemSettingsImplProvider**, **Keywords**, **{**

    **tags**: **{** **code**: **'com.xixikf.workbench.ShellApp.Keywords'** **}**,

**}**)**;**

问题记录

1 开发 app 的时候需要将导出的类用 @injectable 进行修饰，并且需要声明 didLoad 类方法，extension 没有这个限制。

参考

1[https://requirejs.org/docs/api.html](https://requirejs.org/docs/api.html#packages)
2[https://cdnjs.com/libraries/require.js](https://cdnjs.com/libraries/require.js)
3[How symbols helps to avoid name collision in object in js?](https://stackoverflow.com/questions/70743687/how-symbols-helps-to-avoid-name-collision-in-object-in-js)
4[Module federation](https://webpack.js.org/concepts/module-federation)
5[3.0 插件开发 - 小雨](https://yuque.antfin.com/docs/share/6b4ead57-3c16-48b4-a622-84163c2e3044?#%20%E3%80%8A3.0%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91%E3%80%8B)
6[3.0 应用开发系列 - 方炎](https://yuque.antfin.com/docs/share/eac30694-f184-4f9c-a729-6b7794016e14?#%20%E3%80%8A%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E3%80%8B)
7[Servos 文档](https://doc.alibaba-inc.com/xixikf/docs/intro)
8[https://blog.greenroots.info/javascript-why-reflect-apis](https://blog.greenroots.info/javascript-why-reflect-apis)
9[Decorator &amp; Reflect Metadata &amp; InversifyJS](https://juejin.cn/post/6844903759877783559#heading-32)
10[https://jkchao.github.io/typescript-book-chinese/tips/metadata.html#controller-与-get-的实现](https://jkchao.github.io/typescript-book-chinese/tips/metadata.html#controller-%E4%B8%8E-get-%E7%9A%84%E5%AE%9E%E7%8E%B0)
11[Decorators &amp; metadata reflection in TypeScript: From Novice to Expert (Part IV)](http://blog.wolksoftware.com/decorators-metadata-reflection-in-typescript-from-novice-to-expert-part-4)
12[Metaprogramming - medium](https://medium.com/jspoint/introduction-to-reflect-metadata-package-and-its-ecmascript-proposal-8798405d7d88)
13[why-reflect-metadata-suc-s-5fal](https://dev.to/svehla/why-reflect-metadata-suc-s-5fal)

若有收获，就点个赞吧
