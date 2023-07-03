

# Next获取数据

 **Static Generation** （静态渲染）

 **Server-side Rendering**（服务端渲染）

介绍：Next.js 是 React 服务端渲染应用框架. 用于构建 [SEO](https://so.csdn.net/so/search?q=SEO&spm=1001.2101.3001.7020) 友好的 SPA 应用.

支持两种预渲染方式, **静态生成(getStaticProps )**和**服务器端渲染(**getServerSideProps**)**(ssr).
基于页面的路由系统, 路由零配置
自动代码拆分. 优化页面加载速度.
支持静态导出, 可将应用导出为静态网站.
内置 CSS-in-JS 库 styled-jsx
方案成熟, 可用于生产环境, 世界许多公司都在使用
应用部署简单, 拥有专属部署环境 Vercel, 也可以部署在其他环境

**基于页面的路由系统：**

无需我们定义路由配置，页面直接在pages文件夹中的react组件，页面地址和文件夹地址是一一对应的关系。

页面与页面之间通过Link组件跳转。

**静态资源访问：**

应用程序根目录中的 public 文件夹用于提供静态资源.
通过以下形式进行访问.
public/images/1.jpg -> /images/1.jpg
public/css/base.css -> /css/base.css

**修改页面的Title：**

通过 Head 组件修改元数据，指定title标签。

**next中添加样式**：

内置css-in-js

支持css模块，CSS 模块约定样式文件的名称必须为"组件文件名称.module.css

**全局样式文件**：

在 pages 文件夹中新建 _app.js 文件并加入 global.css.

**预渲染**

- Client-side Rendering (CSR)
- Static Generation (SSG)
- Server-side Rendering (SSR)

预渲染是指数据和HTML的拼接在服务器端提前完成，以使 SEO 更加友好，带来更好的用户体验。

预渲染的两种形式：静态生成和服务端渲染，区别在于生成 HTML 的时机不同.

静态生成是在**构建时**生成 HTML. 以后的每个请求都**共用**构建时生成好的 HTML.

服务器端渲染是在**请求时**生成 HTML. 每个请求都会**重新**生成 HTML.

静态生成一次构建, 反复使用, 访问速度快. 因为页面都是事先生成好的.（推荐）

服务器端渲染访问速度不如静态生成快, 但是由于每次请求都会重新渲染, 所以**适用数据频繁更新的页面或页面内容随请求变化而变化的页面**