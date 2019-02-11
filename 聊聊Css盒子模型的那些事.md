## 前言

盒子模型是面试时的最常遇到的考点，因为这是css布局中最核心的概念，只有理解这个重要的知识点才能更迅速的实现页面基础排版，我觉得这更像是一种布局思维，开始工作时还吃过亏，今天就给大家科普一下这个知识点。


## 一、css盒子模型是什么

![box](https://www.runoob.com/images/box-model.gif)

css盒子模型又称框模型(Box Model)，所有的HTML元素都可以看作盒子，盒子的组成由内而外分别是由：
1. <span style="color:#75b7ff;">Content(内容)</span> - 盒子的内容，显示文本和图像。 
2. <span style="color:#75b7ff;">Padding(内边距)</span> - 清除内容周围的区域，内边距是透明的。
3. <span style="color:#75b7ff;">Border(边框)</span> - 围绕在内边距和内容外的边框。
4. <span style="color:#75b7ff;">Margin(外边距)</span> - 清除边框外的区域，外边距是透明的。



## 二、详细深入盒子模型
现代的浏览器一般都有两种渲染模式：标准模式和怪异模式。在标准模式下，浏览器按照HTML与CSS标准对文档进行解析和渲染；而在怪异模式下，浏览器则按照旧有的非标准的实现方式对文档进行解析和渲染。这样的话，对于旧有的网页，浏览器启动怪异模式，就能够使得旧网页正常显示；对于新的网页，则可以启动标准模式，使得新网页能够使用HTML与CSS的标准特性。

可能这么说大家还是对这个概念还是很模糊，于是我跑了个示例demo，神秘代码如下:

    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>css盒子模型</title>
    </head>
    <style>
        body {
            padding-top: 300px;
            text-align: center;
        }
    
        div {
            margin: auto;
        }
    
        .father {
            box-sizing:border-box;//怪异盒子
            width: 200px;
            height: 200px;
            background-color: red;
            padding: 30px;
        }
    
        .child {
            width: 200px;
            height: 200px;
            background-color: #469af5;
        }
    </style>
    
    <body>
        <div class="father">
            <div class="child"></div>
        </div>
    </body>
    
    </html>    //可直接粘贴运行查看效果

大家可以看到父盒子(father)的样式宽度为200px,可是设置了padding:30px这个样式后：

宽度计算却变为<span style="color:#FF6666;">宽度(200px)</span>+<span style="color:#FF6666;">左内边距(30px)</span>+<span style="color:#FF6666;">右内边距(30px)</span>=<span style="color:#FF6666;">总宽度(260px)</span>。即<span style="color:#FF6666;">总宽度=width(content)+ margin+ padding+border</span>,如下图:

![demo1](https://image.jamescathy.top/boxModel-1.png)

以上示例用的是默认盒子模型，也就是<span style="color:#FF6666;">标准模型</span>，除此之外还有<span style="color:#FF6666;">IE模型</span>，也俗称"<span style="color:#FF6666;">怪异盒子</span>"，设置怪异盒子样式后效果如下：

![demo2](https://image.jamescathy.top/boxModel-2.png)

父盒子(father)的总宽度计算模式变为：<span style="color:#FF6666;">总宽度=width(content + border + padding)+ margin</span>。所以导致父盒子的实际content宽高只有140px，子盒子(width:200px)自然而然就被父盒子无情的挤出来了...

<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1525862111333&di=f79ee5b566b2e2049bc645ca7e6bf394&imgtype=0&src=http%3A%2F%2Fimg2.jiemian.com%2F101%2Foriginal%2F20170930%2F150676527189819600_a580xH.jpg" width = "200" height = "200" alt="pig1"/>

## 三、如何修改盒子模式
既然盒子模型如此怪异，且不符合w3c的标准，那么我们平常开发时就要尽量避免使用Quirks Mode(怪异模式)，应使用Standards Mode(标准模式)，另外修改盒子模式可以通过css样式更改：
    
    box-sizing:content-box;
    //标准模式
    
    box-sizing:border-box;
    //怪异模式
    
    box-sizing:inherit;
    //继承父元素的属性值

## 四、Say Something
    
另外啰嗦一下，某些浏览器环境下的块级元素有些会自带margin或者padding属性值，比如body默认就有magin，如果不知道可能会影响开发，所以我建议一般最好全局性的去除一些css属性，有利于以后的布局开发。

例如这样：
    
    *{
        margin:0;
        padding:0;
    }

但是这样做好像还是不够<span style="color:#FF6666;">专业(bi ge)</span>，所以我们还可以使用并集选择器，区别性对待想初始化的元素:

    body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td
    {
    margin:0;
    padding:0;
    }

可是这么做好像还是不够<span style="color:#FF6666;">优雅(bi ge)</span>，所以一般我都会使用这种方式，没错!就是引入现成的样式表!

<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1525866394121&di=84e47964b5d33e5fbcba58ef8154e667&imgtype=0&src=http%3A%2F%2Fdingyue.nosdn.127.net%2FWJKHCh8w0eFJPxyB7sORKhXu%3DOY29z8l3iCcplHyGiz171524986385260.jpg" width = "200" height = "200" alt="pig2"/>

一般使用以下2种样式表：

> 1. <span style="color:#FF6666;">Reset.css</span> (真丶初始化，默认样式通通清空)
>
> 2. <span style="color:#FF6666;">Normalize.css </span>(和平解决，只去除不该存在的默认样式)

我一般会用Normalize.css，also，如果在下的修为够高，建议根据项目以及你的理解去新建一份适用的样式表，或者你跟我一样是<span style="color:#FF6666;">懒癌患者</span>，那么用还是不用，用这个还是那个这个问题就见仁见智啦。

## 总结
觉得这篇文章非常偏向萌新，希望能帮助到大家，因为我也只是参与工作不到一年，所以可能水平略渣，我会继续更新更多自己在工作实践中遇到的经历，分享到此结束，下回再见~！


<div style="margin-top:50px;">
