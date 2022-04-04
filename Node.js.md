[TOC]



# Javascript v8引擎

# 运行node命令



```js
node app.js

nodemon app.js

chmod u+x app.js
```

# 如何退出node项目

### `ctrl-C`

在控制台运行程序时，我们可以通过`ctrl-C`来关闭，如果程序的退出则需要使用下面的命令

### `process.exit()`

process核心模块提供了一个方便的方法，允许您以编程方式退出Node.js程序:`process.exit()`。

当Node.js运行这一行时，进程会立即被强制终止。

这意味着任何挂起的回调，任何仍在发送的网络请求，任何文件系统访问，或写入stdout或stderr的进程——所有这些都将立即被不优雅地终止。

可以传递一个整数，向操作系统发出退出代码的信号

```js
process.exit(1)
```

默认情况下，退出码是0，这意味着成功。不同的退出码有不同的含义，您可能希望在您自己的系统中使用它来使程序与其他程序通信。

### `process.exitCode`

也可以设置`process.exitCode`属性，当程序结束时，Node.js将返回退出代码。

### `SIGKILL`

```js
const express = require('express')

const app = express()

app.get('/', (req, res) => {
  res.send('Hi!')
})

const server = app.listen(3000, () => console.log('Server ready'))

process.on('SIGTERM', () => {
  server.close(() => {
    console.log('Process terminated')
  })
})
```

`SIGKILL`是告诉进程立即终止的信号，理想情况下它的作用类似于process.exit()。

### `SIGTERM`

`SIGTERM`是告诉进程优雅地终止的信号。它是过程管理者如upstart或supervisor等发出的信号。

### `process.kill`

你可以在程序内部的另一个函数中发送这个信号

```js
process.kill(process.pid, 'SIGTERM')
```

# 如何读取node中的环境变量

Node.js的进程核心模块提供了`env`属性，它承载了在进程启动时设置的所有环境变量。

```js
USER_ID=239482 USER_KEY=foobar node app.js
```

*Note:* `process`不需要“require”，它是自动可用的。

```js
process.env.USER_ID // "239482"
process.env.USER_KEY // "foobar"
```

以同样的方式，您可以访问您设置的任何自定义环境变量。

如果您的节点项目中有多个环境变量，您还可以在项目的根目录中创建一个.env文件，然后使用dotenv包在运行时加载它们。

```bash
# .env file
USER_ID="239482"
USER_KEY="foobar"
NODE_ENV="development"
```

```js
require('dotenv').config();
process.env.USER_ID // "239482"
process.env.USER_KEY // "foobar"
process.env.NODE_ENV // "development"
```