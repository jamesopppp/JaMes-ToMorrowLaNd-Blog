前言
==
最近，准备上线的两款Hybrid App测试阶段反馈到部分用户无法请求接口数据，经过各种姿势的测试排查后，确定是系统为Android 9.0及以上会出现此问题，于是本菜鸡就开始了探究原因。

> 原来Google表示，为保证用户数据和设备的安全，针对下一代 Android 系统(Android P) 的应用程序，将要求默认使用加密连接，这意味着 Android P 将禁止 App 使用所有未加密的连接，因此运行 Android P 系统的安卓设备无论是接收或者发送流量，未来都不能明码传输，需要使用下一代(Transport Layer Security)传输层安全协议，而 Android Nougat 和 Oreo 则不受影响。

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562816692&di=42174d212611d305abc74dc2f400b227&imgtype=jpg&er=1&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201808%2F23%2F20180823195436_xvucj.jpeg">

所以说白了，就是Android 9.0后http请求（明文流量）非加密的网络流量请求都会被系统BAN掉，即便应用嵌套了webview，webview也不能使用明文传输~

## 解决方案
抱着求知的渴望发现主要有三种解决方案可以去解决：

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562222107924&di=ed2beae5df452f9002ac9299ceb964fc&imgtype=0&src=http%3A%2F%2Fdmimg.5054399.com%2Fallimg%2Fqidai%2Fshrbqb%2F009.jpg">

### 一、App使用安全加密的传输方式https进行网络请求

毕竟众所周知，Apple官方一直只允许使用https进行数据请求，毕竟安全为首要嘛，这肯定会成为未来的趋势吧我觉得，以后可能都只推荐使用https进行网络请求了。

### 二、App的targetSdkVersion降到27以下

降低sdk版本，27及以下还没有这个新的特性，可以避免这个问题,但我未去使用该方案。

### 三、App工程添加网络配置并应用（我使用的方案）

1. 在工程目录res（cordova8.0+目录: Project xxx/platforms/android/app/src/main/res/xml ）下新增一个叫xml的目录（如果没有的话），然后创建一个xml文件[推荐命名 "network_security_config.xml"]，然后配置以下内容：
```
<?xml version="1.0" encoding="utf-8"?>

<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```

大致意思就是允许使用http请求网络

2. 在AndroidManifest.xml文件下修改application标签属性(引用上一步的配置文件)
```
<application
...
 android:networkSecurityConfig="@xml/network_security_config"
...
/>
```

ok!大功告成，重新打包的App可以正常使用http请求啦~，不方便的地方是每次新建工程都需要重新进行配置！不过总算又踏过一个坑,可喜可贺可喜可贺!

查阅参考以下文章:
> https://blog.csdn.net/qq_29634351/article/details/86654535

> https://blog.csdn.net/Simon_Crystin/article/details/88574248

## 总结
身为一个前端开发工程师（饮水机管理员）不仅要擅长多端开发，还需要了解关于原生的一些知识，现在的前端工程师基本上技能点需要广泛成长才能在现今站得住脚，少抱怨多学习，生活更美好!加油加油~!

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562224735472&di=d00d612ae81527661c5d1928b654820f&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201711%2F28%2F20171128205230_Z5XQH.thumb.700_0.jpeg">