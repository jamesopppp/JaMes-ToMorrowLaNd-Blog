Node.js 算是目前大多数前端工程师技术栈中必备的一项了，使用 Javascript 参与后端开发可能是很多人之前都想象不到的事情，想想都令人兴奋，一起努力开始学习吧~

## Node.js 简介

<img style="margin: 10px 0;" src="https://image.jamescathy.top/Node_logo.jpeg" width = "350" alt="Node"/>

Node.js 是一个<span style="color:#EE9A00;">基于 Chrome V8 引擎的 JavaScript 运行环境(平台)</span>。 
Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。

以上是一段来自Node.js官网的简介，我们还可以了解到：

1. Node 就是 Javascript 语言在服务端的运行环境。（很多人包括我曾以为 Node 是一门语言或者框架）

2. Javascript 语言通过 Node 在服务器运行，在这个意义上，Node 有点像 Javascript 虚拟机。

3. Node 提供了大量工具库，使得 Javascript 语言与操作系统互动(比如读写文件、新建子进程)，在这个意义上，Node 又是 Javascript 的工具库。

<img style="margin: 50px 0 20px 0;" src="https://image.jamescathy.top/Node_powered.jpeg" width = "300" alt="Node"/>

Node.js 的诞生其实很奇妙，值得一提的是 Node.js 之父 Ryan Dahl，据他回忆当时本来想采用 Ruby，但是 Ruby 的虚拟机效率不行，是 Node 选择了 Javascript，不是 Javascript 发展出来了 Node。

<img style="margin: 10px 0;" src="https://image.jamescathy.top/chrome_logo.jpeg" width = "240" alt="Chrome"/>

Node 内部采用的 V8 引擎本来只是 Google Chrome 浏览器内核里的 Javascript 语言解释器，因为 V8 引擎执行Javascript的速度非常快，性能非常好。Ryan Dahl 这个鬼才就把它搬到了服务端上，通过自行开发的 libuv 库，调用操作系统资源，并且加以封装拓展，Node 对一些特殊用例进行优化，提供替代的API，使得 V8 在非浏览器环境下运行得更好。

而且 Node.js 的包管理器是 Npm，全球最大的开源库生态系统，Node 本来并没有太多的功能性 API，所以有很多第三方人员开发出来很多 Package。

<img style="margin: 20px 0;" src="https://image.jamescathy.top/npm_javascript.png" width = "340" alt="Chrome"/>

## Node.js 的诞生

* 2008年左右，随着 AJAX 的逐渐普及，Web开发逐渐走向复杂化，系统化。

* 2009年2月，Ryan Dahl在博客上宣布准备基于 V8 创建一个轻量级的Web服务器并提供一套库。

* 2009年5月，Ryan Dahl在GitHub上发布了最初版本的部分Node包，随后几个月里，有人开始使用Node开发应用。 

* 2009年11月和2010年4月，两届JSConf大会都安排了Node.js的讲座。

* 2010年年底，Node获得云计算服务商Joyent资助，创始人Ryan Dahl加入Joyent全职负责Node的发展。

* 2011年7月，Node在微软的支持下发布Windows版本。

Ryan Dahl 从宣布着手开发到实现发布仅仅才用了几个月，不得不让人佩服其实力啊。

<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1549954029699&di=d9810614631e00c96dce43f2c775af65&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201710%2F04%2F20171004131829_tQ2HJ.thumb.700_0.jpeg" width = "200" style="margin: 10px 0;" alt="cool"/>

## Node.js 的实现

* Node 内部采用 Google Chrome 的 V8 引擎，作为 Javascript 语言解释器。

* 通过自行开发的 libuv库，调用操作系统资源。

<img style="margin: 10px 0;" src="https://image.jamescathy.top/Node_pic1.png" width = "500" alt="Node"/>

V8 引擎本身使用了一些最新的编译技术。这使得用 Javascript 这类脚本语言编写出来的代码运行速度获得了极大提升，又节省了开发成本。对性能的苛求是 Node 的一个关键因素。 Javascript 是一个事件驱动语言，Node 利用了这个优点，编写出可扩展性高的服务器。Node 采用了一个称为“事件循环(event loop）”的架构，使得编写可扩展性高的服务器变得既容易又安全。提高服务器性能的技巧有多种多样。Node 选择了一种既能提高性能，又能减低开发复杂度的架构。这是一个非常重要的特性。并发编程通常很复杂且布满地雷。Node 绕过了这些，但仍提供很好的性能。

## Deno 并不是下一代 Node.js

近日，因为 Node 自身也有很多诟病， Node 之父 Ryan Dahl 发布新的开源项目 Deno，从官方介绍来看，可以认为它是下一代 Node，使用 Go 语言代替 C++ 重新编写跨平台底层内核驱动，上层仍然使用 V8 引擎，最终提供一个安全的 TypeScript 运行时。

其实 Deno 大部分特性都是针对目前 Node 的痛点而来的，包括无 package.json、依赖的引入和更新方式，针对的就是被广泛吐槽的过大的 node_modules。

#### Deno仓库的issue狂欢

但是在发布没多久后，Deno 的 github 仓库 issue开启了狂欢模式...

<img style="margin: 10px 0 30px 0;" src="https://image.jamescathy.top/deno_issue.jpg" width = "340" alt="issue"/>

贺老 @johnhax 在微博中写到：

> 一帮把issue列表当成聊天室的傻逼，中国开发者的脸都被丢光了。
> 
> 虽然我们一贯很讨厌各种 IT 培训班，但是从今天开始，只要你们做一件事情——确保你们的学员学会 github 上的基本礼仪，你们也算有功德了。

后续大部分issue都被删除了，ry也关闭了issue，狂欢的参与者们也陆续被封号禁言了。

#### 目前 Deno 只是一个 Demo。

既然是 Node.js 之父的新作，在讨论中自然离不开 Node.js。而作者很皮的回复到：

> The main difference is that Node works and Deno does not work : )
>
> 最大的区别就是：Node 可以工作，而 Deno 不行 : )

#### 初学者应该学习 Node.js 还是 Deno？
对于这个问题，Ryan Dahl 的回答干净利落：

> Use Node. Deno is a prototype / experiment.
> 
> 使用 Node。Deno 只是一个原型或实验性产品。

Deno 的目标是兼容浏览器。原生支持 Typescript，不兼容 Node.js 的生态。

所以，Deno 不是要取代 Node.js，也不是下一代 Node.js，也不是要放弃 npm 重建 Node 生态，deno 目前是要拥抱浏览器生态。

引用：
> https://studygolang.com/articles/13101 
> 
> Deno 并不是下一代 Node.js


## 总结

Node.js 为前端圈带来了更多的可能性，现在的前端轮子造的飞起，前端工程师的职责可不是简简单单的画画页面而已，还要开发App端PC客户端等等，只有不断学习才能跟得上时代的步伐，一起加油成长吧！

<img style="margin: 10px 0;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1549954235343&di=065394e25c22ac8eed2ede605495357f&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201801%2F30%2F20180130223326_isajb.jpg" width = "240" alt="learn"/>