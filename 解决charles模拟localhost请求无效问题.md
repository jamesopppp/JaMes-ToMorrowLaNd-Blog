前言
==
最近在使用React自己玩玩项目，使用charles来抓取本地发起的请求并模拟响应，但是无论怎么折腾都代理不了本地的localhost请求，这里总结下怎么成功解决的~

<img style="width:240px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562910951335&di=385cda310961162ccae9bd7d78c1d1ed&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201711%2F19%2F20171119155002_kmrdh.jpeg">

# 起因
刚开始我一直检查不到原因，监控列表里什么请求都能看到，而且桌面的确存在list.json，就是看不到发起的localhost请求，请求也不能正常返回：

<img style="width:500px;" src="https://image.jamescathy.top/20190712-01.png">

<img style="width:600px;" src="https://image.jamescathy.top/20190712-03.png">

我以为是我设置Map Local有误，但是仔细检查也没什么不对的地方：

<img style="width:500px;" src="https://image.jamescathy.top/20190712-02.png">

# 解决问题
后来查阅了[一篇V2EX上的文章](https://www.v2ex.com/t/534350)，其中有人回答了个解决方法，并且我也测试顺利可以解决：

<img style="width:500px;" src="https://image.jamescathy.top/20190712-04.png">

使用localhost.charlesproxy.com代替localhost域名发起请求即可解决。

<img style="width:400px;" src="https://image.jamescathy.top/20190712-05.png">

总结
==
如果嫌麻烦，使用charles进行模拟接口请求真的是挺方便的，只需要简单的设置，并且增加对应的json文件就能满足简单的需求，较复杂的项目可以使用mockjs之类的来进行接口mock。

<img style="width:400px;" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1562910747661&di=f0eaef8c51d15397515850b9e8af945e&imgtype=0&src=http%3A%2F%2F8.pic.9ht.com%2Fthumb%2Fup%2F2018-4%2F201841611424320420_600_566.jpg">