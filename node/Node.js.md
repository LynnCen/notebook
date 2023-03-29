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







# 第一章 基础知识

## 1. 回调

### 定义

就是将一个函数作为参数传递给另外一个函数，并且作为参数是可以被执行的，其本质就是一个高阶函数

```javascript
fs.readFlie("foo",callback)
```

## 2. 同步/异步和阻塞/非阻塞

### 2.1 同步与异步

同步/异步都是描述进程/线程的调用方式

同步：指的是进程/线程发起调用后，一直等待调用返回后才继续执行下一步，这并不代表cpu在这段时间内停止等待，操作系统会切换到其他进程/线程，等到调用返回后再切换回原来的进程/线程。

异步：发起调用后，进程/线程会继续往下执行，当调用返回后，通过某种手段来通知调用者。

### 2.2 阻塞与非阻塞

阻塞与非阻塞都是针对I/O状态而言，阻塞与非阻塞都是同步的。

输入/输出(I/O)：是在内存和外部设备（如磁盘，终端和网络）之间复制数据的过程。

**（1）阻塞IO（blocking I/O**）

对于IO来说通常分为两个阶段，准备数据和返回结果，阻塞型IO在进程发出一个系统调用之后，进程就会一直等待上述两个阶段完成，等到拿到返回结果之后再重新运行。

**（2）非阻塞IO（nonblocking I/O）**

和上面相似，不同的是进程在发出一个系统调用之后，如果数据还没有就绪，就会马上返回上一个结果告诉进程现在还没就绪，和阻塞IO的区别就是用户进程会不断的查询内核状态。这个过程是同步的。

**（3）IO multiplexing/Event Driven**

也称为事件驱动IO，同样是以**轮询**的方式查询内核的执行状态，和非阻塞IO的区别就是一个进程可能会管理多个IO请求，当某个IO调用有了结果之后，就返回对应的结果。

**（4）Asynchronous I/O**

异步IO，当进程发出调用后，内核会马上返回结果，进程会继续做其他事情，直到操作系统返回数据，给用户进程发送一个信号。

总：同步调用会造成调用进程的IO阻塞，异步调用不回造成调用进程的IO阻塞。

### 2.3 单线程和多线程

Node并没有提供多线程的支持，用户编写的代码只能在当前的线程中运行，用于运行代码的事件循环也是单线程运行的。

Node的底层实现并非是单线程的，libuv会通过类似线程池的实现来模拟不同操作系统下的异步调用。

bliuv是一个跨平台的异步IO库，Node的异步调用都是通过libuv来支持的，例如readFile为例，读取文件的系统调用是由libuv来完成的，Node只是负责调用libuv的接口，等数据返回后再执行对应的回调方法。

### 2.4 并行和并发

并行：许多任务同时执行

并发：多个任务轮流执行 执行间隔非常短，看起来像是一起执行

## 3. 事件模型（Event loop）

### 3.1 Node中的事件循环

首先需要明确的是，事件循环同样是运行在单线程环境下，js的事件循环时是依靠浏览器实现的，而Node作为另一种运行时，事件循环是由底层的libuv实现。

| timers （用来处理 setTimeOut()和setInerval()）               |
| ------------------------------------------------------------ |
| I/O callbacks （大多数的回调方法都是在这个阶段执行，除了timers、close、和setImmediate事件的回调） |
| idle,prepare （仅仅在内部使用）                              |
| poll （轮询，不断检查有没有新的IO事件，事件循环可能在这里阻塞） |
| check （用来检查setImmediate()事件的回调)                    |
| close callbacks 处理一些close相关的事件，例如socket.on('close') |

事件循环分为6个不同的阶段，每个阶段都维护着一个回调函数的队列，在不同的阶段，事件循环会处理不同的类型的事件。

**timers**

用来处理定时器相关的回调，当一个定时器超时后，一个事件就会加入到队列中，事件循环会跳转至这个阶段执行相对应的回调函数。

**I/O callbacks** 

除了timers、close、和setImmediate事件的回调，大多数的回调方法都是位于这个阶段执行。

**poll**

poll阶段主要是等待新的事件出现，如果没有，事件循环可能就会在此阻塞。

第一步有定时器任务就执行定时器方法，

第二步 poll队列有任务，就执行。

第三步 poll队列为空，当前代码定义了setImmediate，则转去check阶段去执行setImmediate中的回调方法。

第四步 如果 poll队列为空，且无setImmediate，事件循环可能会进入等待状态，并等到新的事件出现，所以称为轮询。

**check**

check阶段就是为setImmediate方法而设置的

**close**

如果一个socket或者一个句柄被关闭，那么就会产生一个close事件，该事件就会被加入到对应的队列中。close阶段执行完毕，本轮事件循环结束，循环进入下一轮。

### 3.2 process.nextTick

定义一个异步动作，并且让这个动作在事件循环的当前阶段结束后执行。

 process.nextTick并不是事件循环的一部分，但它的回调函数也是由事件循环调用的，该方法的定义的回调会被加入到名为nextTickQueue的队列中，在事件循环的任何阶段，如果nextTickQueue不为空，都会在当前阶段执行完毕之后优先执行nextTickQueue中的回调函数，执行完毕之后，事件循环才会继续往下执行。

### **3.3 setImmediate**

setImmediate的事件会在当前事件循环的结尾触发，对应的回调方法会在当前事件循环末尾执行。



# 第二章 常用模块

## 1. Module

### 1.1 commonJS

### 1.2 AMD

### 1.3 reuire

运行机制

重复引用

缓存策略

隐患

### 1.4 模块化与作用域

控制台中的this

脚本中的this

node中的作用域种类

## 2. Buffer

### 2.1 概述



### 2.2 Buffer的构建与转换

### 2.3 Buffer的拼接



## 3. File system

### 3.1 常见api



## 4. HTTP服务

### 4.1 创建http服务

### 4.2 处理http请求

1. method

2. header

3. request body

### 4.3 Response 对象

1. 设置status code

2. 设置 response header
3. response bodey

4. response end

### 4.4 上传数据

1. 提交表单
2. 使用post上传文件

### 4.5 http客户端服务

反向代理

### 4.6 创建代理服务器



### 4.7 TCP服务

1. TCP和Socket
2. 创建TCP服务器

### 4.8 更安全的传输方式—SSL

##### 什么是SSL

SSL协议及其继任者TLS协议是为网络通讯提供安全及数据完整性得一种安全协议。在传输层对网络连接进行加密。SSL与HTTP结合成HTTPS，与webSocket结合成WSS。

##### SSL原理

不同的SSL握手存在差异，分三种：

只验证服务器

验证服务器和客户端

恢复原有会话

只验证服务器：

（1）客户端发送Client hello消息，该消息包括SSL版本信息，一个随机数，一个session id（用来避免后续请求的握手）和浏览器支持的密码套件cipher suite（由加密算法名称组成的字符串）

（2）服务器确定本次通信使用的SSL版本和其他信息，发送Server Hello给客户端，里面的内容包含服务器支持的SSL版本，一个伪随机数以及服务器的密码套件

（3）服务器发送CA证书给客户端

（4）服务器发送Server Hello done。

（5）客户端验证服务器证书的合法性，利用证书中的公钥加密作为Client Key Exchange 的消息发送给服务器

（6）SSL客户端发送Change Cipher Spec消息，该消息属于SSL密码变化协议

（7）客户端计算历史消息的hash值，然后使用服务器公钥加密后发送给服务器，服务器进行同样的操作，然后两个值相同表示密钥交换成功。

（8）服务器发送Change Cipher Spec消息。

（9）服务器计算历史消息的hash，通过交换后的密钥加密，将其作为finished消息发送给客户端，客户端利用交换后的密钥解密，如果本地历史消息相同就证明服务器身份。握手结束。

##### 对称加密和非对称加密

对称加密：即信息的发送方和接受方使用同一个密钥去加密和解密数据，它的最大优势是加/解密速度更快，适合大量数据进行加密，但密钥管理困难。

非对称加密：又称为公钥加密，它需要使用不同的密钥来分为进行加密和解密操作，一个公开发布，即公开密钥，另一个由用户自己保存，即私用密钥。信息发送者用公开密钥去加密，而信息接收者用私用密钥解密。加密和解密速度比对称密钥加密慢很多。

##### 关于CA

用来验证公钥的正确性。一个CA证书包括用户的信息，CA签发机构信息，用户公钥，有效日期，CA签发机构签名，CA签发机构的公钥。

### 4.9 创建HTTPS服务

HTTPS+SSL = HTTPS 

首先创建公钥、私钥以及证书，可以通过openSSL这一工具来完成。

### 4.10 WebSocket

webSocket可以看作是HTTP协议的升级版，同样是基于TCP协议的应用层协议，主要是为了弥补HTTP协议的无持久化和无状态等缺陷。

在http1.1中 增加了Connection：Keep-alive，当服务器收到带有这一字段的头部请求就会保持TCP不断开，同时也会在response中增加这一字段。可以避免多次的TCP握手带来的性能浪费。

在http协议中，服务器无法主动向客户端发送数据。

websocket可以实现浏览器与服务器的全双工通信。

### 4.11 Stream

##### stream的种类

Readable 可读流

Writable 可写流

Duplex 即可读 又可写

Transform 操作写入数据，然后读取结果，通常用于输入数据和输出数据不要求匹配的场景。

fs.createReadstream() 创建一个可读流。

pipr()相当于在可读流和可写流之间架起了桥梁，使得数据可以通过管道由可读流进入可写流。

##### ReadLine

提供了按行读取Stream中数据的功能

##### 自定义Stream

### 4.12 Events

Node的Events模块定义了一个类EventEmitter。

##### 事件和监听器



### 4.13 多进程服务

##### child_process

node是单线程运行，如果线程奔溃导致进程也会随之退出，单进程无法充分利用多核cpu

创建子进程的方法 fork spawn exec execFile  spawn是最为基础的

##### 进程通信

childProcess模块定义了send方法，用于进程通信。worker.on(‘message“，callback)这个方法中监听。

##### Cluster

可以看作是做了封装的child_process模块，可以共享一个socket连接，可以实现简单的负载均衡。

### 4.14 Process对象

process是一个全局对象，无需声明即可访问，每个node进程都有独立的process对象。

##### 环境变量

**process.env**

##### 方法和事件

process.on('event',callback)

### 4.15 Timer

setTimeout

setInterval

定时器中的this：

在js中，this指向window

在node，this指向timeout类，如需更改，使用call或者bind