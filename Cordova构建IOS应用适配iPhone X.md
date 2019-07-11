前言
==
随着时代进步，可能很多人都使用上了iPhone X及以上的IOS机型，那么我们构建的IOS应用就必须做到对这种刘海屏的适配，5555！每次有各种奇葩的机型屏幕出现，遭殃的就是我们程序猿，这次我们就记录下在Cordova上如何做到适配吧！

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562826899161&di=1f4f3fe162148122db0137b1e2081b09&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201804%2F16%2F20180416051736_jkuir.jpeg">

# 解决方法

<img style="width:400px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562827010313&di=23f53fad647a77eb77400202f394ba90&imgtype=0&src=http%3A%2F%2Fupload.chinaz.com%2F2018%2F0503%2F201805031022165633.jpg">

## 一、更新插件
首先需要更新一些Cordova插件以达到最新版本，因为最新版本才能保证有适配iPhone X的特性能力，列如：
* cordova-plugin-splashscreen
* cordova-plugin-statusbar
如果有用到别的webview容器也需要更新

```
cordova plugin remove cordova-plugin-splashscreen//移除插件
cordova plugin add cordova-plugin-splashscreen//下载最新的插件
cordova plugin remove cordova-plugin-statusbar//移除插件
cordova plugin add cordova-plugin-statusbar//下载最新的插件，cordova-plugin-statusbar插件版本为2.3.0才能适配iPhoneX
```

## 二、更新启动屏启动图 [重要]
推荐使用Launch Storyboard Image，具体可在[官方文档](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/index.html)查阅~

```
<splash src="res/screen/ios/Default@2x~universal~anyany.png" />
<!--一步到位的设置2732 × 2732的启动图即可适配全部尺寸-->
```

如果设置config.xml无效，可以去xcode中找到Launch Images进行设置。

## 三、更新HTML viewport meta
开发移动端的应该知道这个视图属性，但这次要添加一个ios11新增的属性：**viewport-fit=cover**，具体完整如下：
```
<meta name="viewport" content="initial-scale=1, width=device-width, height=device-height, maximum-scale=1, minimum-scale=1, user-scalable=no, viewport-fit=cover">
```

## 四、使用CSS适配Safe-Area
Safe-Area就是iPhone X等全面屏衍生出来的产物，虽然我们已经初步实现内容全面屏化，但是因为顶部还有一部分区域是会被屏幕刘海以及底部的虚拟Home所遮挡。

这里苹果提供了Css的常量constant(safe-area-inset-top)/env(safe-area-inset-*)以方便我们使容器主体UI能做到适配，一般会使用在padding属性上：

```
// 不一定是加在body上
body {
/* Status bar height on iOS 11.0 */
padding-top: constant(safe-area-inset-top);
padding-bottom: constant(safe-area-inset-bottom);
/* Status bar height on iOS 11+ */
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-bottom);
// 记得添加背景色,否则空百区域将为白色
backgroup-color: #000;
}
```

# 总结
其实实现起来也挺简单，但注意最重要的是启动图必须配置全面屏的尺寸，还有在开发时也要注意页面布局，尽量采用自适应方式去布局，以免适配时造成需要重构布局的结果，好啦~又增加了点经验！加油！

<img style="width:300px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562828723214&di=b566fabfa3a6145f77744f3b21e07197&imgtype=0&src=http%3A%2F%2Fimg.tukexw.com%2Fimg%2F95d89fc656c957d7.jpg">