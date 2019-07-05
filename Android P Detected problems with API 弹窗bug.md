前言
==
最近上线的App，开始有个别用户反映每次启动App时会弹出一个英文弹窗，听到这有点奇怪的问题，我立马怀疑是不是跟安卓派（android pie 安卓P 即 android 9.0系统代号）的新特性有关系?

<img style="width:400px;" src="https://image.jamescathy.top/2019-07-04-1.png">

## 找到原因
果不其然，查阅大量文档文章后大概了解了这个情况发生的原因：
> 自从手机系统升级到Android 9.0以后，打开APP开始出现以上提示，出现这种情况的原因是：
Android P 后Google限制了开发者调用非官方公开API 方法或接口，也就是说，你用反射直接调用源码就会有这样的提示弹窗出现，非 SDK 接口指的是 Android 系统内部使用、并未提供在 SDK 中的接口，开发者可能通过 Java 反射、JNI 等技术来调用这些接口。但是，这么做是很危险的：非 SDK 接口没有任何公开文档，必须查看源代码才能理解其行为逻辑。

我开发的Hybrid App是使用Apche Cordova构建底层的框架，我猜或许这类框架在底层构建上存在反射调用源码的地方，也或许是因为我调用了个别插件导致的，那没办法咯，只能想想如何消灭这个弹框的弹出了。

<img style="width:250px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562237738834&di=d1e24de9e3558e337d51058cbb50adaf&imgtype=0&src=http%3A%2F%2Fi-4.yxdown.com%2F2017%2F10%2F14%2F5b410f8e-a7cd-4e16-9a96-30e680ddd41a.jpg">

## 解决方案
查阅了部分安卓大佬的文章后，终于找到一篇有讲到针对Cordova出现该情况的解决方案（简直美滋滋）：

<img style="width:150px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562237878459&di=9585bf2eaafdd8af588f3f2317ee65cd&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201803%2F25%2F20180325093152_t5PdF.thumb.700_0.jpeg">

### 1、找到Cordova Android工程下的驱动源文件:
<img style="width:400px;" src="https://image.jamescathy.top/2019-07-04-2.png">

### 2、导入所需代码
<img style="width:60%;" src="https://image.jamescathy.top/2019-07-04-3.png">

```
import java.lang.reflect.Method;
import java.lang.reflect.Field;
```

### 3、添加禁止弹窗方法并调用
<img style="width:100%;" src="https://image.jamescathy.top/2019-07-04-4.png">

```
public void onCreate(Bundle savedInstanceState){
    ... //忽略代码块
    disableAPIDialog();
}

private void disableAPIDialog(){
    try {
        Class clazz = Class.forName("android.app.ActivityThread");
        Method currentActivityThread = clazz.getDeclaredMethod("currentActivityThread");
        currentActivityThread.setAccessible(true);
        Object activityThread = currentActivityThread.invoke(null);
        Field mHiddenApiWarningShown = clazz.getDeclaredField("mHiddenApiWarningShown");
        mHiddenApiWarningShown.setAccessible(true);
        mHiddenApiWarningShown.setBoolean(activityThread, true);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

ok，至此重新构建安装包就可以解决这个问题啦！非常感谢这位大佬给予的解决方法!~

参考文章：
> https://blog.csdn.net/qq_41590018/article/details/87699136  @好名字打不过备注

总结
==
其实前端开发工程师利用现有的前端知识及技能去开发App肯定是不够的，我们还需要补充学习很多关于安卓及IOS的原生知识才能安稳的睡觉，更何况在目前的公司里，大大小小的App都是我独自开发的，更需要付出更多的时间去学习，甚至还有恶心的Ios安卓兼容问题，必须写出双端兼容的代码！加油冲呀~James!

<img style="width:240px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562238011307&di=58b64818b59c76634bbfcce57393eaf0&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn17%2F480%2Fw240h240%2F20180506%2F5b27-hacuuvu0491282.gif">