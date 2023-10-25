# ServOS 原理

**2022.07.04**

**在了解这个框架之前，需要大家先了解下 RequireJS 和 Inversify 两个库的基本使用，知道的同学可以直接跳过这两部分。**

**这里来写一个最小可用的 ServOS 框架来帮助大家来理解下原理，这里就叫它 SOS 把。。。 源码**[在这](http://gitlab.alibaba-inc.com/wb-wzs728893/SOS)，感兴趣的同学可以拉下来跑一下。

## RequireJS

[RequireJS](https://requirejs.org) 是一个 amd 模块加载器，模块的定义通过 `<span class="ne-text">define</span>` 方法，模块的加载通过 `<span class="ne-text">require</span>` 方法。模块的定义是异步的，正如 amd 的全称 ** Asynchronous Module Definition** 表示的一样，当你定义的模块所依赖的所有模块都被定义完成了，你的模块才算定义完成，才可以使用 `<span class="ne-text">require</span>` 方法加载你的模块。

**比如，**

```javascript
define("antd", ["moment"], (moment) => ({}));
// 此时 antd 模块还未定义，你通过 require(["antd"], (antd) => {})
// 会报错，因为 antd 模块还不存在，接下来定义 moment 模块
define("moment", [], () => ({}));
// 因为 moment 不依赖任何模块，所以当你定义完成 moment 模块的时候
// 你的 antd 模块也就定义完成了，看到没，这就是异步的模块定义
```

**在 SOS 框架里，我们可以定义两个函数来定义和加载一个模块，比如有那么个关于一个模块的 JSON 对象的描述，**

```javascript
{
  "bundleName": "moment",
  "modulePath": "moment.min.js",
  "packageName": "moment",
  "url": "https://cdnjs.cloudflare.com/ajax/libs/moment.js/",
  "version": "2.29.3"
}
```

**我们就可以利用 RequireJS 来加载这个模块，通过往 RequireJS 的 paths 里加入一对 KVP，K 是模块名，V 是模块的 URL 地址，这样你就可以通过 **`<span class="ne-text">require([K])</span>` 的形式加载模块了，

```javascript
class ModuleService {
  registerModule(name, url) {
    globalThis.require.config({
      paths: {
        [name]: url,
      },
    });
  }

  loadModule(name) {
    return new Promise((resolve, reject) => {
      globalThis.require(
        [name],
        (module) => {
          resolve(module);
        },
        (err) => {
          reject(`${name} module load failed`);
        }
      );
    });
  }
}
```

**使用如下，**

```javascript
const moduleService = new ModuleService();

moduleService.registerModule(
  "moment",
  "https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.3/moment.min"
); // 注意不需要文件后缀这里

moduleService.loadModule("moment").then((moment) => console.log(moment));
```

## Inversify

**如果你知道这个控制反转/依赖注入的概念的话，可以直接跳过看下一小节。**

**它是一个 IOC（控制反转/依赖注入）的库，简单来说它提供用一个类实例管理多个类实例，以及类被初始化的时候帮你自动被注入一些你想要的其他实例的功能。**

**你可以初始化一个 **`<span class="ne-text">Container</span>` 实例，然后通过 `<span class="ne-text">bind</span>` 方法往 `<span class="ne-text">Container</span>` 实例上绑定其他类，然后你可以通过 `<span class="ne-text">get</span>` 方法获取我们绑定的其他类的实例，

```javascript
// 创建一个容器，默认是 Transient 的 Scope，每次调用 `get` 方法返回的都是一个新的类实例，这里使用 单例 模式
const container = new Container({ defaultScope: "Singleton" });

// 我们将 ModuleService 这个类绑定到 “ModuleService” 这个 ID 上
container.bind("ModuleServiceID").to(ModuleService);

// 我们通过 “ModuleService” 这个 ID 就可以获取到这个 ModuleService 类的实例了，当然我们要在这个类的上面加一句 @injectable 才行
const moduleService = container.get("ModuleService");

// 我们可以使用这个类去加载一些已经定义好的模块
moduleService.loadModule("moment").then((moment) => console.log(moment));
```

**上面只是讲了一个容器实例管理一堆其他实例的例子，正常情况下一个 service id 只能绑定一个类，但是你可以给这个 id 加一个限制条件（constraint），也就是在 **`<span class="ne-text">.to</span>` 方法调用完，再调用 `<span class="ne-text">.when</span>` 方法添加一个限制条件，这样你可以用 `<span class="ne-text">container.getAll(id)</span>` 的方法获取所有的类实例，这在 SOS 中可以用来做 Tab 类的插件。

**当然我们也可以通过依赖注入的方式获取到类实例，**

```javascript
@injectable
class ModuleService {...}

@injectable
class MyClass {
   constructor(@inject("ModuleServiceID") moduleService) {
	 this.moduleService = moduleService;
  }
}

container.bind("myClass").to(MyClass);
const myClass = myContainer.get("myClass");
myClass.moduleService.loadModule("moment").then(moment => console.log(moment));

// 通过使用 @inject装饰器（decorator）你在初始化一个类实例的时候，声明的依赖会自动被注入到你的实例里去，也就是所谓的依赖注入
```

**Inversify 就是这样通过一个 service id 绑定一个 service 的形式给你提供服务，默认它会帮你实例化这个 service，但是你也可以给这个 service id 绑定一个常量，使用 **`<span class="ne-text">.toConstantValue</span>` 方法，而 service id 官方推荐使用 Symbol，详见[官方文档](https://github.com/inversify/InversifyJS)。

## SOS

![](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/290311/1657266629699-0af3df3c-a73d-41aa-b46b-6d6eccec2736.png)

### 入口

**首先来看下我们工作台页面的源码，核心部分就下面两行，**

```html
<script src="/g_config.js"></script>
<script>
  require(["@ali/servos-core"], () => {
    servos.bootstrap();
  });
</script>
```

**第一行就是我们的 **`<span class="ne-text">g_config</span>` 文件，来看看里面都有啥，

```javascript
requirejs.config({
  paths: {
    "@ali/servos-core": "https://g.alicdn.com/servos/core/1.4.5/core/index",
  },
});
requirejs(["@ali/servos-core"], ({ BootstrapConfig }) => {
  const config = {
	...
  };
  globalThis[BootstrapConfig] = config;
});
```

**上面两行就是加载了我们的 servos 模块，并用模块里导出的 BootstrapConfig Symbol 将 config 对象用该 Symbol 作为 key 加到我们的 window 对象上，所以，如果你想要在 3.0 工作台拿到我们的配置信息的话，这样就可以了，**

```javascript
import { BootstrapConfig } from "@ali/servos-core";
const config = globalThis[BootstrapConfig];
```

**仔细看下，你会发现它其实用的是 Symbol 而不是 Symbol.for 定义了这个变量，**

```javascript
export const BootstrapConfig = Symbol("servos.core.BootstrapConfig");
```

### SOS 初始化（bootstrap）

**想看 Servos 源码的同学，可以直接 **`<span class="ne-text">npm install @ali/servos-core</span>` 然后在 `<span class="ne-text">node_modules</span>` 里就可以看到了，几乎是没有做任何语法转换看起来也是非常直观的，里面的 decorator 的部分做了转换，使用的是 TypeScript 的 decorator，和 EcmaScript 提案中的 decorator 还是有点不同的，感兴趣的同学可以了解下。

**---- 核心模块**

**接着上面，在执行完 **`<span class="ne-text">g_config</span>` 文件之后，我们就能拿到一个 servos 的实例了，因为在导出的 servos 的 `<span class="ne-text">index.js</span>` 文件里，导出了 `<span class="ne-text">servos.js</span>` 文件，里面定义了 Servos 的类，定义完成后实例化了这个类并将其写到了 window 上，

```javascript
// index.js
// 导出所有模块，比如 Servos 的实例化就是在这里执行的

// servos.js
// Servos 实例化的时候会实例化一个 IOC 的容器用来管理其他实例
export const servos = new Servos();
globalThis.servos = servos;
```

**我们有了 servos 实例之后，下一步就是 **`<span class="ne-text">servos.bootstrap();</span>` 进行相关服务的初始化了，`<span class="ne-text">index.js</span>` 里导出的 `<span class="ne-text">CoreModule.js</span>` 里导出了一个 **ServosCoreModule** 变量，里面定义了很多内置的服务，当 bootstrap 执行到 `<span class="ne-text">ServosCoreModule.registry(binding);</span>` 这一步的时候，servos 就开始往容器上绑定服务了，下面来讲下一些比较核心的服务吧，

- **DefaultModuleService（模块的注册和加载）**
- **DefaultBundleService（注册 bundle 到 RequireJS config）**
- **DefaultLibraryService（加载一些需要 preload 的模块）**
- **DefaultExtensionService（将 extension 注册到容器上）**

**每一个服务绑定到容器上之后，一般还没什么用，当 bootstrap 执行到 **`<span class="ne-text">CoreStartupTask</span>` 这个任务的时候，`<span class="ne-text">await serviceManager.startAllServices()</span>` 这里才开始真正做一些事情，它会遍历我们刚才注册的服务的实例身上的 `<span class="ne-text">start</span>` 方法，来看下上面这些服务的 `<span class="ne-text">start</span>` 方法都做了啥，

**DefaultModuleService** 这个类和我们第一小节里面声明的 ** ModuleService** 这个类一样，你可以将其绑定到 container 上，然后使用对应的 service id 去获取这个类实例，如果你遇到需要注册模块或者加载模块的时候，参考下第二小节介绍 Inversify 的时候的例子。

**DefaultBundleService** 这个类会将 `<span class="ne-text">g_config.bundle.list</span>` 里的每一个 bundle 注册到 RequireJS 配置中去，bundle 的 manifest 的 packageName 作为模块名，URL 需要拼接，和我们在第一小节介绍 RequireJS 的时候注册模块的时候一样。

** DefaultLibraryService** 这个类会将 `<span class="ne-text">g_config.lib.list</span>` 里的每一个 lib 注册到 RequreJS 配置中去，并且设置了一个 callback，当对应的 lib 模块被加载的时候，同时加载该 lib 指定的资源列表，也就是 manifest 里的 `<span class="ne-text">resources</span>` 字段。如果 lib 需要预加载（manifest 里的 preload 字段）的话，那么这个时候会加载这个 lib 模块，并且加载其指定的 resources。

** DefaultExtensionService** 这个类会将 `<span class="ne-text">g_config.extension.list</span>` 里的每一项绑定到容器上，通过 `<span class="ne-text">bind(implements字段).toDynamicValue(() => extension).when(request => request.target.matchesNamedTag(extension.fullName))</span>` 这种带 constraint 的方式，因为我们的扩展（extension） 在工作台上都是 `<span class="ne-text">com.xixikf.workbench.Plugin</span>` 这个扩展点（service id），所以我们很容易就可以通过 `<span class="ne-text">servos.container.getAll(Symbol.for('com.xixikf.workbench.Plugin'))</span>` 的形式拿到所有的 extension 了（比如，信息查询，语音转文本这些 extension）。这里因为工作台里都是用 Symbol 作为 service id 的，所以这里 `<span class="ne-text">.getAll</span>` 的时候使用了 Symbol，然后我们就可以利用这些 extension 自己写一个插件容器了，不一定要是 Tabs 这种形式也行。注意一点，这里绑定的其实不是 extension 的类，只是绑定了个 manifest。

** DefaultApplicationService** 这个类会对 `<span class="ne-text">g_config.app.list</span>` 里的每一个 app 并不会做什么，如果你只是问绑定的话。不过这个类后面会用到。

**到此为止基本上 **`<span class="ne-text">g_config</span>` 里的所有 list 相关的数据都用到了， **bundle** **，** **lib** **，** **app** **，** **extension** **。**

**---- 加载默认应用**

**接下来 bootstrap 进入到了加载默认应用 **`<span class="ne-text">DefaultAppStartupTask</span>` 这一步了，

```javascript
const app = await this.applicationService.launchDefaultApplication();
const target = document.getElementById("mount-point");
this.reactRenderService.render(app, target);
```

`<span class="ne-text">launchDefaultApplication</span>` 方法底层就是调用了 `<span class="ne-text">ModuleService</span>` 服务提供的 loadModule 方法来加载模块，默认的应用是在 `<span class="ne-text">g_config.app.default</span>` 这里指定的，默认一般是，`<span class="ne-text">com.xixikf.workbench.ShellApp</span>`，bundle 名是 `<span class="ne-text">com.xixikf.workbench</span>`，entry point 是 `<span class="ne-text">ShellApp</span>`，这样你就可以在 app list 里找到对应的 manifest 了，这样模块加载完你就可以拿到这个 entry point 对应的类了，然后通过用 manifest 构建出来的 fullName 作为 service id 将这个类绑定起来，fullName 的话，`<span class="ne-text">${manifest.bundleName}.${manifest.name || manifest.entryPoint}</span>` 长这样。然后再通过 `<span class="ne-text">container.get(fullName)</span>` 函数来获取我们的类实例，然后调用该实例身上 `<span class="ne-text">didLoad</span>` 等的一些生命周期函数，这样就拿到了一个实例了，一般就是一个 React 组件实例，下一步就是调用 `<span class="ne-text">this.reactRenderService.render(app, target);</span>` 进行组件渲染了。

`<span class="ne-text">this.reactRenderService</span>` 这个东西，其实就是 CoreModule 里的 `<span class="ne-text">DefaultReactRenderService</span>` 类实例，但是这个类依赖的是 `<span class="ne-text">Symbol.for('servos.core.rendering.ReactRenderer')</span>` 这个 service id 对应的服务。像这种和 UI 相关的肯定不是写在 servos 框架里的，那在哪里？

**对，在 servos-ui 这个库里，当你加在默认应用的时候，也就是我们的工作台壳应用，它依赖 **`<span class="ne-text">servos-ui</span>` 这个库，所以我们的壳应用被加载后我们 `<span class="ne-text">servos-ui</span>` 这个库也就被夹在了，这个库被加载后会在 `<span class="ne-text">Symbol.for('servos.core.rendering.ReactRenderer')</span>` 这个 service id 上绑定一个对象，一般将 `<span class="ne-text">ReactDOM</span>` 作为常量进行绑定就可以了，它身上有一个 render 方法。

```javascript
export const ServosUIModule = new ServosModule((binding) => {
  binding.registerExtensionImpl(ReactRenderer, ReactDOMRenderer);
});
export default ServosUIModule;
```

**不过它实际上就是干了下面这件事情，本质上一样，**

```javascript
container
  .bind(Symbol.for("servos.core.rendering.ReactRenderer"))
  .toConstantValue({ render: ReactDOM.render });
```

**在 Servos 里，如果你想要在 container 上绑定一些服务的话，你需要这样写，**

```javascript
export const ServosUIModule = new ServosModule((binding) => {
  binding.registerExtensionImpl(ReactRenderer, ReactDOMRenderer);
});
export default ServosUIModule;
```

**注意两个点，一个是使用 **`<span class="ne-text">ServosModule</span>` 这个类，还有一个是实例化的类要使用 ESM 的 default 默认导出。

**Servos 在 ModuleService 实例化的时候，使用 RequireJS 的 onResourceLoad 函数，在每个模块加载完成的时候会去调用下该模块暴露出来的 default 方法，大致的实现其实就是这样，**

```javascript
globalThis.requirejs.onResourceLoad = (context, map) => {
  // 每个模块可以导出一个默认模块，用来往容器上绑定一些服务
  // 比如 reactRenderer
  // 也就是所谓的 隐式的注册机制
  const module = context.defined[map.name];
  if (module.default) {
    // TODO: 我这里一股脑儿的调用 default 函数了，可以给 default 函数加个标识
    module.default(this.container);
  }
};
```

**这一块重点可以看下 **`<span class="ne-text">RemoteApplication</span>` 这个类。

### 插件容器

**对于 extension 这里补充几点，虽然你可以通过 **`<span class="ne-text">servos.container.getAll(Symbol.for('com.xixikf.workbench.Plugin'))</span>` 这种方式获取到所有的 extension，然后遍历进行渲染，但是呢，工作台（workbench）提供的 Tabs 插件容器对 extension 进行了过滤（因为工作台有个插件管理的页面，坐席可以决定是否展示某个 extension 以及对 extension 进行排序），所以不会展示所有的 extension，你可以看下 `<span class="ne-text">TabsPluginContainer</span>` 组件，里面通过 `<span class="ne-text">usePlugins</span>` 这个 hooks，最终会调用 `<span class="ne-text">RuntimeInstalledPluginsByApp</span>` 这个接口，这个接口需要你传一个 appName 作为入参，比如热线（`<span class="ne-text">com.xixikf.hotline.desk.HotlineDeskApp</span>`），然后接口就会将你开发的所有的 extension 里 manifest 里的 supports 数组里有 `<span class="ne-text">com.xixikf.hotline.desk.HotlineDeskApp</span>` 这个值的 extension 全拉出来，

```javascript
// extension manifest 示例
{
  "bundleName": "com.xixikf.hotline.desk",
  "displayName": "语音转文本",
  "entryPoint": "HotlineTouchChatPanel",
  "implements": "com.xixikf.workbench.Plugin",
  "name": "HotlineTouchChatPanel",
  "props": {
    "supports": ["com.xixikf.hotline.desk.HotlineDeskApp", "com.xixikf.train.TrainApp"]
  },
  "resources": [
    {
      "type": "css",
      "url": "index.css"
    }
  ]
}
```

![](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/290311/1657266668801-0dec22bc-4612-4fb2-b3d5-19fda0de267a.png)

**根据 app 支持的 slots 进行分类，这样使用插件容器的时候给一个 **`<span class="ne-text">slot</span>` props 就能进一步的对上面的返回结果进行过滤了。

```javascript
// 这个是热线工作台这个应用的 manifest
{
  "bundleName": "com.xixikf.hotline.desk",
  "description": "",
  "displayName": "热线客服",
  "entryPoint": "HotlineDeskApp",
  "extensionPoints": [
    {
      "allowMultiple": true,
      "dynamic": false,
      "name": "subject.SubjectProvider"
    }
  ],
  "name": "HotlineDeskApp",
  "props": {
    "slots": [
      {
        "displayName": "热线信息插件区",
        "name": "HotlineInformationPlugins",
        "type": "plugin"
      },
      {
        "displayName": "热线助手插件区",
        "name": "HotlineAssistantPlugins",
        "type": "plugin"
      }
    ]
  },
  "resources": [
    {
      "type": "css",
      "url": "index.css"
    }
  ],
  "routePath": "hotline-desk",
  "uuid": "com.xixikf.hotline.desk.HotlineDeskApp"
}
```

**你可以在热线 app 这个仓库里看到这样的用法，**

```javascript
<PluginContainer slot="HotlineInformationPlugins" />
```

**这个 **`<span class="ne-text">PluginContainer</span>` 插件容器组件会根据传过来的 slot props 对 extension 进行二次过滤。

**看到这里你应该对于\*\***扩展点**和**扩展**有了个大概的理解，它们就是 IOC 框架里的 **service id** 和 **service\*\* 的关系。

**---- 其他**

**如果有讲的不对的或者不好的，欢迎留言区评论 👏**

## 用例

### 1. 微服务框架

**ServsOS 也支持 2.0 这样的微应用管理机制，首先加载的默认应用会作为一个和 yodajs 容器一样的壳应用，提供路由，状态管理等能力，并且默认对路由做了缓存。**

```javascript
<Routes basename={basePath || "/"}>
  <Route path="/*" element={<UniContainer />} />
  <Route
    path="/"
    element={
      <NavigateHome homePath={routingConfig?.homePath || "/app-center"} />
    }
  />
</Routes>
```

```javascript
// UniContainer 通过 display 属性对页面进行了缓存
{
  routes.map((route) => {
    const active = route === current;
    return route.app.hasLoaded || route === current ? (
      <div
        key={route.path}
        className={cn(styles.container)}
        style={{ display: active ? "flex" : "none" }}
      >
        <ErrorBoundary errorDisplay={ErrorDisplay}>
          <ApplicationContainer app={route.app} active={active} />
        </ErrorBoundary>
      </div>
    ) : null;
  });
}
```

**同时因为 servos 在 bootstrap 之后会往 RequireJS 中添加配置，所以开发的插件，应用等也可以在打包的时候将 **`<span class="ne-text">React</span>`，`<span class="ne-text">antd</span>` 等常用模块给 external 出去，Vite 里可以通过 rollupOptions 来进行 build 的配置。

## TODO

1. **manifest 里的 ** **allowMultiple** **，\*\***autoLoad** 属性什么用，好像没在代码里看到，**dynamic\*\* 看到了不过还不知道什么用。
2. **为什么要使用这种设计模式，利好点在哪里，一开始使用的时候感觉理解成本挺高的。**
3. **在 **[Inversify](https://github.com/inversify/InversifyJS/blob/772ea8ef53b17ac00a35df45d00bfc2f1ca53d07/wiki/symbols_as_id.md) 里，用 Symbol.for 和用字符串有啥区别，除非用 `<span class="ne-text">Symbol("xxx")</span>` 才不会造成命名冲突。但是我发现用 `<span class="ne-text">.to</span>` 绑定的时候，service id 用同一个字符串两次会报错，但是用 `<span class="ne-text">Symbol.for</span>` 就不会。
4. **动态菜单 3.0。我记得将下面的配置给到后端就可以了添加一个菜单了，这个还没来得及看具体的实现。**

```javascript
// 将这个给了后端就行了
// 菜单加载的时候的就会加载我们的组件了
binding.registerExtensionImpl(SystemSettingsImplProvider, Keywords, {
  tags: { code: "com.xixikf.workbench.ShellApp.Keywords" },
});
```

## 问题记录

1. **开发 app 的时候需要将导出的类用 **`<span class="ne-text">@injectable</span>` 进行修饰，并且需要声明 `<span class="ne-text">didLoad</span>` 类方法，extension 没有这个限制。

## 参考

1. [https://requirejs.org/docs/api.html](https://requirejs.org/docs/api.html#packages)
2. [https://cdnjs.com/libraries/require.js](https://cdnjs.com/libraries/require.js)
3. [How symbols helps to avoid name collision in object in js?](https://stackoverflow.com/questions/70743687/how-symbols-helps-to-avoid-name-collision-in-object-in-js)
4. [Module federation](https://webpack.js.org/concepts/module-federation)
5. [3.0 插件开发 - 小雨](https://yuque.antfin.com/docs/share/6b4ead57-3c16-48b4-a622-84163c2e3044?#%20%E3%80%8A3.0%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91%E3%80%8B)
6. [3.0 应用开发系列 - 方炎](https://yuque.antfin.com/docs/share/eac30694-f184-4f9c-a729-6b7794016e14?#%20%E3%80%8A%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E3%80%8B)
7. [Servos 文档](https://doc.alibaba-inc.com/xixikf/docs/intro)
8. [https://blog.greenroots.info/javascript-why-reflect-apis](https://blog.greenroots.info/javascript-why-reflect-apis)
9. [Decorator &amp; Reflect Metadata &amp; InversifyJS](https://juejin.cn/post/6844903759877783559#heading-32)
10. [https://jkchao.github.io/typescript-book-chinese/tips/metadata.html#controller-与-get-的实现](https://jkchao.github.io/typescript-book-chinese/tips/metadata.html#controller-%E4%B8%8E-get-%E7%9A%84%E5%AE%9E%E7%8E%B0)
11. [Decorators &amp; metadata reflection in TypeScript: From Novice to Expert (Part IV)](http://blog.wolksoftware.com/decorators-metadata-reflection-in-typescript-from-novice-to-expert-part-4)
12. [Metaprogramming - medium](https://medium.com/jspoint/introduction-to-reflect-metadata-package-and-its-ecmascript-proposal-8798405d7d88)
13. [why-reflect-metadata-suc-s-5fal](https://dev.to/svehla/why-reflect-metadata-suc-s-5fal)
