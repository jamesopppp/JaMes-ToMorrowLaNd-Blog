前言
==
这次再记录个在使用Cordova时遇到的bug，主要是针对Android端的，如果用户在系统的设置里，设置了特别的字体大小时，界面排版什么的就会随着出现很多无法预料的问题！

<img style="width:160px;" src="http://b-ssl.duitang.com/uploads/item/201710/04/20171004183433_E2YsL.thumb.224_0.jpeg">

# 引起原因

还记得我遇到这问题时是有点懵b的，因为在google的控制台调试并没出现这种问题，讲道理我也不会犯这么大的错误呀，为啥一到手机上就变成这鬼样...当时适配移动端还是用着rem那套，所以应该也不会有这种问题。

<img style="width:140px;" src="http://b-ssl.duitang.com/uploads/item/201710/02/20171002142856_GvWLT.thumb.700_0.jpeg">

后来测试的同事手机我随便看了下其他应用还有系统界面，发现他手机大部分字体界面都比平常的大，原来他设置了系统字体大小！具体设置界面android都大同小异如下：

<img style="width:50%;" src="https://image.jamescathy.top/20190709-01.png">

所以万一用户是老年群体或者近视群体，或多或少都会设置字体大小来达到正常使用，所以这个问题必须想办法解决！

# 解决方案

所以怎么解决这个问题呢，度娘上就看到了类似的问题，有android原生大佬就提出了[修改cordova底层工程文件的一个办法](https://www.cnblogs.com/happymental/p/9755849.html)：

<img style="width:100%;" src="https://image.jamescathy.top/20190709-02.png">

```
<!--source code-->
webView.getSettings().setTextZoom(100)；
```

## 1、找到corodva工程下的视图引擎文件(SystemWebViewEngine.java)：

具体目录在：
```
当前版本：cordova 8.0
/platforms/android/CordovaLib/src/org/apache/cordova/engine/SystemWebViewEngine.java
```

## 2、在initWebViewSettings方法增加对settings变量的参数修改：

<img style="width:100%;" src="https://image.jamescathy.top/20190709-03.png">

```
<!--source code-->
settings.setTextZoom(100);
```

ok至此，简单的修改然后保存，重新编译构建app，真机上修改系统设置的字体大小已经无法对app造成影响。

总结
==
这个问题对于我这个前端菜b来说的确有点摸不着头脑，毕竟也没有太多原生安卓的知识经验，不过还好有大佬分享出解决的办法，不过这是刚接触cordova时遇到的问题，现在记录起来以防自己忘记，毕竟我感觉我的记性还是很差的...

<img style="width:200px;" src="http://n.sinaimg.cn/sinacn17/480/w240h240/20180506/0467-hacuuvu0491775.gif">