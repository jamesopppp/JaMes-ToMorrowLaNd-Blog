前言
==
最近有个App需要增加保存图片到本地图库的新功能，遗憾的是当时上线的App未添加文件管理及下载的原生功能，所以需要上载一个大版本让用户重新下载才能使用这个功能，具体实现已完成，主要是在构建IOS安装包时遇到一些奇怪的问题。

## 当前环境

* Cordova 8.0
* Android 7.1.4
* Ios 4.5.5

## 遇到难关
首先因为重新rebuild了app的上层文件，安卓真机测试已经正常可以实现功能，但是IOS需要重新构建工程并使用xcode开发者模式上机测试才能确保万无一失，毕竟一套代码双端运行还是要谨慎点的~

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562432019282&di=b4bb3b432b643cb50518ad0d32ff02bf&imgtype=0&src=http%3A%2F%2Fp1.so.qhimgs1.com%2Ft019737d79b12def611.jpg">

可是当我重新构建IOS工程时出现以下的错误:

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-06-02.png">

我整个直接凌乱了，这是报了个找不到模块的错，可是platform_metadata这是什么平台我却无从下手，于是开始请教万能的google与百度，可是网上的说法各有不一，因为之前的工程是可以正常构建IOS的，所以我怀疑起了后来新增的几个插件，其中我留意的上面的报错里一个插件:

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-06-03.png">

没错，就是这个add-swift-support的插件，于是我就联合关键词搜了一下度娘，找到了[这篇文章](http://www.voidcn.com/article/p-wnfgzfjm-but.html):

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-06-04.png">

仔细看了下，遇到的情况几乎一致（泪流满面），原来就是该插件的一个版本错误，于是我一顿操作，remove然后重新add一遍，IOS工程终于成功构建啦~

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562432888483&di=151f52b1f9aa3616cca9d39f83775e4e&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn12%2F559%2Fw690h669%2F20180416%2Faa6e-fzcyxmv3101827.jpg">

## 再遇难关，直接闪退
当我以为稳了，能一路顺风进行调试IOS上的功能了，我又一次遭到了社会的毒打，我发现无论我用模拟器或者真机启动App都是直接闪退!我滴妈呀，我又蒙了。

天真的我又怀疑是插件导致的，会不会是哪个插件不兼容IOS呢？可是我查看了所有正使用的插件都不存在双端兼容问题（当然我不会犯这么低级的错误），我就把插件全卸了，开了个新的工程开始一步一步的排查，重复着枯燥的检查。

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562433237319&di=ee0f5bdd7371e66f8a9acff38e17a768&imgtype=0&src=http%3A%2F%2Fwww.3987.com%2Fuploadfile%2F2018%2F0711%2F20180711115230534.jpg">

## 抓出元凶

去厕所洗了把脸清醒了下再回来，一直徒劳无果的我这次观察了下真机调试时的日志，好像发现了点眉目：

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-06-01.png">

仔细观察另外一个App在启动时第一个启动的插件是hot-code-push热更新程序，在反观这个工程第一个插件都走不到就直接闪退了，于是我怀疑是热更新惹的祸，仔细检查了下，我在构建IOS时的前端上层文件貌似少了这两个玩意：

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-06-05.png">

难道是因为这两个热更新校检文件导致的IOS程序崩溃?可是我的常识告诉我，安卓没生成热更新文件构建App运行也没问题啊，难道苹果就是这么恶心?我立马生成热更新文件然后重新构建App再上真机运行，这一次!App成功运行了!!!我终于长叹了口气!然后开始调试新功能~功能调试也正常!太棒了!

<img style="width:250px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562434107928&di=3861389aa1d75d12c95e24deb4ed6d4a&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn07%2F700%2Fw750h750%2F20180423%2F53df-fznefki0004693.jpg">

总结
==
生产双端App应用一直是我很恐惧的事情，因为不单单要应对各种你无法预料的问题，比如安卓苹果原生的问题，还得保证你的代码可以在双端运行无兼容问题，万一有些问题解决不了可能就无法构建应用了，用户日活可是有10多万的呢!而且目前所在公司也没有比我水平更高更擅长的人了，我只能自己学习解决...不过痛并快乐着吧，大大小小的坑都踩过，越努力越幸运呀，懂得东西多总是没坏处的~

<img style="width:400px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562434607778&di=a759db5334307a29e8c6ce9e7249a50a&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fq_70%2Cc_zoom%2Cw_640%2Fimages%2F20171113%2F886f2de310de49898da9802725c9b567.gif">