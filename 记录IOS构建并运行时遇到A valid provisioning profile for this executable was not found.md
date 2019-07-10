前言
==
这次主要记录一次在尝试真机调试时遇到的IOS构建问题，还以为是证书的问题，可是尝试了网上很多办法都没解决，最后原来只要设置一下构建系统的模式就可以了（惊呆）!

<img style="width:200px;" src="http://b-ssl.duitang.com/uploads/item/201710/26/20171026201046_CHzYZ.thumb.700_0.jpeg">

# xcode版本
<img style="width:400px;" src="https://image.jamescathy.top/20190710-02.png">

# 起因
<img style="width:500px;" src="https://image.jamescathy.top/20190710-03.png">

看这图上的提示，说的是找不到有效的可执行配置文件，可是我用的是免证书真机调试，跟证书应该扯不上关系，网上大部分的回答都是检查provisioning profile有没有正常导入，或者全部删除重新导入之类的回答，然后对我并没什么用...

# 解决问题

后来看到[一篇文章](https://blog.csdn.net/mobo123/article/details/83652486)是说切换默认的构建模式即可解决问题，我半信半疑的尝试了下，居然就成了！

<img style="width:400px;" src="https://image.jamescathy.top/20190710-01.png">

具体在xcode的 file=>Project Settings，将第一项的Build System切换为Legacy Build System再重新运行构建就没问题了，猜测是因为版本的问题，具体就没深究了。

总结
==
好吧，苹果还是比较"恶心"的，各种莫名其妙的问题，这个报错的图打死都想不到这样设置就能解决了，幸好网上还是有很多前车之鉴的，不过这些坑遇过了，下次就不会那么手忙脚乱了，哈哈哈!

<img style="width:400px;" src="http://b-ssl.duitang.com/uploads/item/201812/07/20181207001325_zdgms.thumb.700_0.gif">