上次初步了解了下 Node.js ，这次我们记录下 Node.js 的环境配置，环境配置大家都不陌生吧，由于 Node 的版本迭代速度很快，所以我们可能需要在不同版本之间切换，这里就用到下面所说的 NVM了。

## 安装包方式（不推荐）
最简单的方式可能是使用安装包的方式安装 Node，从[官方网站](https://nodejs.org/zh-cn/)就可以找到下载链接，下载完成后只需要不断Next选择就可以完成自动化安装，但是我们身为 Geek(码农)，怎么能这么随便呢，而且这种安装方式有很多不足的地方，爱折腾的我们肯定不能坐以待毙啦~

<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1552277884507&di=6fb457bae7e1a4faed6246a161fc2a2b&imgtype=0&src=http%3A%2F%2Fwww.chinairn.com%2FUserFiles%2Fimage%2F20180416%2F20180416151427_5244.jpg" style="width: 300px;">

## 更新 Node 版本
为什么不推荐使用安装包方式安装 Node 呢，Node 版本迭代非常快，导致一些常见问题：
* 以前版本安装的很多全局的工具包需要重新安装
* 无法回滚到之前的版本
* 无法在多个版本之间切换（有时需要在特定版本使用Node）

如果有一天我想体验新的版本，我必须重新下载最新的安装包，然后覆盖安装，非常麻烦。

## NVM 的方式

### Mac OS：
具体可以参考 Node 的 [Github Readme](https://github.com/creationix/nvm) 文档详细了解。

- 使用终端
- 输入：

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
source ~/.nvm/nvm.sh
```
- 安装以后，nvm 的执行脚本，每次使用前都要激活，建议将其加入~/.bashrc文件(如果使用的是Bash)，激活后，就可以安装指定版本的 Node 啦~

- 也可以使用 Homebrew 安装（更方便，维护更简单）
```
brew install nvm
```

### Window：
windows 系统下有用go语言开发了个叫 [nvm-windows](https://github.com/coreybutler/nvm-windows) 的工具，方便 windows 用户去安装使用 NVM,具体可以参考 Github 文档，现在可以直接安装setup安装程序，过程也是非常简单的。

<img style="margin:20px 0;" src="https://image.jamescathy.top/nvm-windows.png">

截止现在最新 release 版本是1.1.7，主要安装资源有以下这些：
* nvm-noinstall.zip：绿色免安装版，需要配置环境变量之类的。
* nvm-setup.zip：setup安装，无需配置，简单方便。(推荐)
* source code(zip)：zip压缩的源码。
* source code(tar.gz)：tar.gz压缩的源码，一般用于Linux系统下。

安装完成后，通过终端控制台输入 nvm，检查是否安装成功：

<img style="margin:20px 0;" src="https://image.jamescathy.top/nvm-command.png?new">

有哪些可用命令都可以在里面看到，使用非常简单，安装 Node后，Npm也会一并安装，也一样可以使用 npm -v 查看版本，这样我们就可以随意切换 Node 版本啦~

## 总结
简单的安装 Nvm 后可以让我们更方便的管理 Node 版本，当然配置程序运行环境是我们必备的技能，无论是什么语言，都可能需要特定的环境配置，互联网时代，我们要多利用网上资源学习这些知识，不要对这些感到恐惧，多尝试大不了重装电脑的事~haha

<img style="width: 450px;" src="http://5b0988e595225.cdn.sohucs.com/images/20180707/91daf4a33a724fea8510d5013c337c71.gif">