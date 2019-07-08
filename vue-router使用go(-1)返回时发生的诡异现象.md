前言
===
最近在使用vue-router时发现一个诡异的现象，大体就是一些页面中使用了我自己封装的header组件，里面的返回图标事件实际调用了router.go(-1)这个api，但是比较小概率的触发了一个现象：点击返回时路由的url发生了变化，无论调用多少次，页面视图还是没有任何改变！但是如果使用其他api，例如push、replace之类的却正常发生url及视图改变...

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562567563856&di=b014aa261926db81647ba666c480c8fa&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201712%2F31%2F20171231104624_RnfvH.thumb.224_0.jpeg">

# 当前环境

* vue： 2.6.6
* vue-router： 3.0.1
* webpack： 4.28.4
* ruouter mode： hash

# 探究原因
发生这个现象，我其实开始是有点摸不着头脑的，因为这从没遇到过，而且触发几率非常低，我也只是偶尔发现了这个现象，这个现象到底是哪里影响进而发生的呢？我又开始了我的网上冲浪之旅~

<img style="width:200px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562582805112&di=c1e9ab34645012228226ac8c58bb91ee&imgtype=0&src=http%3A%2F%2Fdmimg.5054399.com%2Fallimg%2Fqidai%2Fshrbqb%2F009.jpg">

## Github上的issue

我第一时间跑到了vue-router的github上寻找相关的issues，不过好像一直找不到相关的问题，我就好奇难道这个问题还没有被广泛的发现？后来终于看到了[一个23天前被关闭的issue](https://github.com/vuejs/vue-router/issues/2812)：

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-08-01.png">

第一个issue发起的格式不对，于是这位开发者又发出了一个issue：

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-08-02.png">

仔细看这位老铁的描述，貌似情况应该跟我差不多，但是他给出的链接我没看懂啥意思，版本信息与我也差不多的样子，维护者回答了下，意思估计是描述的不清晰且提供了个没用的链接，还有让提起的issue使用英文书写不然看不懂啊哈哈哈hhhh~所以还是没有什么帮助。

## 另外一篇相同情况的文章

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-08-03.png">

依然是相同情况，但是还是看不到问题发生原因之类的内容~确实很捉急，而且经过大量测试，如果把路由模式改为History，貌似就不会出现这个问题了...

# 找到解决方案

终于在博客园看到了一位大佬也遇到了一样的问题，而且他的说法也验证我之前的测试，History模式的路由的确能规避这个诡异的现象，并且他也给出了个hack的方法可以暂时解决这个问题，大概总结下解决方案：

[关于vue-router中点击浏览器前进后退地址栏路由变了但是页面没跳转](https://www.cnblogs.com/mmzuo-798/p/10260327.html)

## 1、将路由模式改为History模式

一般vue-router默认的router mode默认都为hash，如果想避免这个情况可以直接修改路由模式为History，但是！别高兴的太早，一般想使用这个模式，还得后端去做相应的支持，否则会出现一些刷新的问题，具体可以看[vue-router的官方文档](https://router.vuejs.org/zh/guide/essentials/history-mode.html)有详细介绍，那我不用还有个原因是因为，我这个项目最后会产出静态文件然后构建App，使用History做路由会导致页面丢失。

## 2、hash模式中使用h5新增的onhashchange事件做hack处理

这个事件名字顾名思义就是当hash值改变时触发，并提供两个回调参数，分别是newURL（页面新URL）和oldURL（页面旧的URL），而且兼容性也非常不错：

<img style="width:100%;" src="https://image.jamescathy.top/2019-07-08-04.png">

关于该事件的具体详情可以上MDN文档看看：[window.onhashchange](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/onhashchange)

具体实现代码如下：
```
// 检测浏览器路由改变页面不刷新问题,hash模式的工作原理是hashchange事件
window.addEventListener(
  'hashchange',
  () => {
    const currentPath = window.location.hash.slice(1)
    if (this.$route.path !== currentPath) {
      this.$router.push(currentPath)
    }
  },
  false
)

Vue可在App.vue的生命钩子中挂载监听
```

意思就是当浏览器hash值变更时，获取浏览器location的hash与当前路由的hash做比对，不相同的话就调用router.push浏览器location的hash值达到跳转。

# 延伸发现
虽然最后还是不知道引起该现象的问题所在，但好在暂时解决了，但其实我发现，调用router.push并不会触发onhashchange事件，但是调用router.go()却能正常触发，但好在我也仅仅只需要router.go(-1)能正常运作而已，具体原因：
> vue-router 底层调用的是history.pushState和history.replaceState, 这2个api对url的更新不限于hash, 不会触发hashchange

<img style="width:150px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563177651&di=8a4680d8e0a774faab9097975cccf9d3&imgtype=jpg&er=1&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201710%2F14%2F20171014200946_5vCuF.thumb.224_0.png">

总结
===
至于这个诡异的bug我还会继续关注，但按照目前网上搜索，貌似遇到这个情况的人并不多，我也考虑向官方提个issue，庆幸的是这个问题暂时性的解决了~加油加油！

<img style="width:220px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562582918740&di=208b591f9ab41edeb5043b6c496faae4&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201711%2F23%2F20171123223333_ncXrE.thumb.224_0.jpeg">