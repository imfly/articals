# 使用 Google Fonts 为网页添加美观字体

原文： http://www.ibm.com/developerworks/cn/web/1505_zhangyan_googlefont/

在线字体提供丰富多样的字体样式，能使页面更美观，更具吸引力。Google Fonts 就是一个开源的在线字体库，使用起来简单快速。

[张 彦](#authorN10026), 软件工程师, IBM

*   [![-](//www.ibm.com/i/c.gif)](#toggle)<span class="ibm-twisty-head">内容</span>

    <div class="ibm-twisty-body">

    *   [前言](#1.前言 |outline)
    *   [什么是安全字体](#2.什么是安全字体 |outline)
    *   [@font-face 标签简介](#3.@font-face 标签简介 |outline)
    *   [如何使用 Google Fonts API](#4.如何使用 GoogleFontsAPI|outline)
    *   [结束语](#7.结束语 |outline)
    *   [参考资料](#resources)
    *   [评论](#icomments)

## 前言

文字是网页中很重要的组成部分。为文字选择一个合适的字体，能够更好的展现一个网站的个性，表达所要传递的信息，同时吸引用户来产生兴趣。

说到字体，我们首先会想到 CSS 里面的 font，例如：

```
<html>
<head>
    <style>
        p { font-family: Arial, Helvetica, sans-serif; }
    </style>
</head>
<body>
    <p>some text</p>
</body>
</html>
```

在这段 HTML 代码中为<p>标签定义了字体，当浏览器解析<p>some text</p>标签时，首先会在系统中查找 Arial 这个字体，如果找不到，就找 Helvetica 字体，如果还是找不到，就会查找浏览器默认的 sans-serif(非衬线)字体，最后把文字渲染出来。

## 什么是安全字体

安全字体这个概念，也许很多人都不是很熟悉，我们先举个例子：

```
font-family: Arial, Helvetica, sans-serif;
```

这个对字体（font-family）的定义就是一个安全字体。每种操作系统都有自己默认安装的字体，浏览器只能正常显示操作系统中安装了的字体。而不同的操作系统默认安装的字体不完全相同，有的甚至名称都不一样，在这种情况下，我们必须定义安全字体，使字体在所有的浏览器中都能够正常显示。

在上面这个 font-family 的定义中，我们选择 Arial 作为首选字体（注：Arial 字体是最常用的 sans serif 字体，也是 Windows 的默认字体，当字体很小是不容易阅读），但是，苹果系统中没有这个字体，所以我们选择 Helvetica（和 Arial 很相似）作为第二备选字体，最后我们选择 sans-serif 作为第三备选字体，如果在一个既没有 Arial 也没有 Helvetica 的系统里，那么浏览器会使用默认的 sans-serif 字体来渲染文字。这样，我们很大程度上保证了使用不同操作系统的访问者都能看到相同（至少是类似）的页面文字。

除了 Arial，常见的安全字体还有：

*   Verdana 字体，它是微软公司的核心字体之一，专门为屏幕显示而开发的。它的应用广泛，宽度大而易于阅读，是显示器中最清晰的字体。CSS 写法：font-family: Verdana, Geneva, sans-serif;
*   Times New Roman 字体，它是最常用的 serif 字体，是浏览器默认的字体。小号字的易读性也很差。CSS 写法：font-family: 'Times New Roman', Times, serif;

有兴趣的读者可以通过这个链接来查阅常用的安全字体。[http://www.w3schools.com/cssref/css_websafe_fonts.asp](http://www.w3schools.com/cssref/css_websafe_fonts.asp)

在网页开发中，应该尽量使用安全字体，也就是高度通用的字体，这样，访问者才能流畅的阅读网页的所有内容。

但是，网页设计师一定不会满足于使用这些安全字体，如何才能使用漂亮的字体，并能在普通用户的浏览器中被正确的渲染出来？答案是：使用@font-face 方案。

## @font-face 标签简介

@font-face 被列为了 CSS3 的一项新特性，其实它并不是什么新鲜技术，它最早出现在 CSS2 的规范定义中，但是在 CSS2.1 中又被删除，现在被正式列入 CSS3。目前主流的浏览器（IE 4+/Firefox 3.5+/Chrome 1+/Safari 3.1+/Opera 10+）都能够支持这个属性，所以不用担心会有浏览器兼容性问题。

@font-face 允许您在网页中使用电脑中没有安装的字体，完全摆脱安全字体的限制。只需将字体包安装在服务器上，当用户加载网页时，字体包会自动下载到用户机器上，保证字体能够正确渲染。

随着@font-face 的不断流行，产生了许多新的字体格式图标集，称为网络字体。Google Fonts API 就是基于@font-face 的特性开发的一套优秀的网络字体库。

网络字体的优点有很多：

1.  使用的是真正的文本，而不是图片，放大和缩小都不会影响渲染效果，用户体验好；
2.  可以被搜索引擎辨认；
3.  不像图片每次需要重新生成，添加删除更方便。

## 如何使用 Google Fonts API

Google Fonts 提供了超过 600 种的高质量的字体，所有的浏览器都兼容，无需引入 JavaScript，简单易用，更重要的是，免费。（虽然，暂时不支持中文字体，因为中文字体库实在是太大了）。

现在来看下，如何在网页中使用 Google Fonts。

### 挑选字体

登录[Google Fonts （谷歌字体官方网站）](http://www.google.com/fonts/)。（图 1）

##### 图 1.Google Fonts 主页

![Google Fonts 主页](image003.jpg)

在主页中，您可以直接浏览所有的字体，可以按单个词语来查看，或者按句子段落来查看整体效果，还可以调整字体大小。如果您对字体分类比较熟悉，就可以使用左边的搜索条件对字体作筛选。找到您喜欢的字体之后，点击"Add to Collection"按钮，然后在网页下方的"Collection"中即可找到您添加的所有字体。

### 使用选中的字体来测试您的文字

在上一页面（图 1）的"Collection"中，点击"review"。在预览页面中（图 2），输入您的测试文字，来查看效果。另外在"review"页面上，您还可以调整其它的与字体相关的样式，比如字体大小，间隔，变换，等等。在图 2 中，我们选择测试的是"Condiment"字体

##### 图 2.Google Fonts 预览页面

![Google Fonts 预览页面](image005.jpg)

### 在您的网页中添加字体链接。

如果确认使用该字体， 在网页下方的"Collection"中，点击"Use"按钮，在接下来的页面中，您将看到详细的说明，包括字体链接和如何将字体添加到您的网页中。

有三种方式来添加字体链接：

*   Standard 方式：

```
<link rel='stylesheet' type='text/css'
href='http://fonts.googleapis.com/css?family=Condiment'>
```

*   @import 方式：

```
@import url(http://fonts.googleapis.com/css?family=Condiment);
```

*   JavaScript 方式：

    （通过添加动态脚本并执行来导入字体，代码省略）

下一步定义在那个标签上使用该字体，例如：在`<class="myheader">`标签上使用，

.myheader {font-family: 'Condiment', cursive;}

大功告成，您现在可以打开您的网页欣赏一下了(图 3)。

##### 图 3.测试页面效果

![测试页面效果](image007.jpg)

##### 清单 1.测试页面代码

```
<html>
          <head>
                     <link rel="stylesheet" type="text/css"           
                               href='http://fonts.googleapis.com/css?family=Condiment'>
                    <style>
                             .myheader {
                                        font-family: Condiment, cursive;
                                        font-size: 48px;
                                        text-align: center;
                               }
                    </style>
          </head>
          <body>
                     <div class="myheader"> Test my own text !</div>
          </body>
</html>
```

### 优化字体包加载

如果您不是大范围的在网页中使用 Google 字体，只是在标题或 logo 里使用，那么可以在 url 里添加 text 参数，来限制加载的字体包的大小，最高能缩减 90%左右的大小，以此来节约下载流量。例如：http://fonts.googleapis.com/css?family= Condiment'&text=Hello

这样，您只会下载 h，e，l，o 这四个字母的字体，大大缩小了字体包的大小。

### 下载字体包

您还可以把字体包下载并安装到本地，这样您就可以在本地使用这些字体，比如在 Notepad，Microsoft Office 里。

方法为：在图 3 中，点击下载按钮![](image009.jpg)， 选择"Download the font families in your Collection as a zip file"

### 高级应用 API

Google Fonts 还开放了一些接口（称为 Developer API），用于获取字体库的信息数据。

比如实时获取字体库实际可用的字体及其相关信息：

[`https://www.googleapis.com/webfonts/v1/webfonts?key=`](https://www.googleapis.com/webfonts/v1/webfonts?key=YOUR-API-KEY)[`_YOUR-API-KEY_`](https://www.googleapis.com/webfonts/v1/webfonts?key=YOUR-API-KEY)

这个请求的返回结果是一个 JSON 类型的数据，包括了每种字体的名称，样式种类（比如 regular，italic），版本，修改时间，包含的样式包的请求地址，等等。

请注意，在 URL 里面有一个 key，这个 key 是和您的 web 应用工程相联系的，只有注册过的 web 应用才能成功调用 Developer API。我们必须要在 Google Cloud Console 注册之后，才能获取这个 key。

关于这个 Developer API， 可以参阅这个[链接](https://developers.google.com/fonts/docs/developer_api)。

## 结束语

Google Fonts 很强大，但是也碰到一些加载的问题，最好还是字体声明的时候，在最后添加一个安全字体，来保证万无一失。另外的一个建议是，在网页中要适量的使用花样的字体，让整个页面保持干净简洁。

## 参考资料

### 学习

*   Google Fonts [开发入门](https://developers.google.com/fonts/docs/getting_started)
*   [Google Font 字体介绍](http://www.google.com/fonts/earlyaccess)
*   W3School 网站上关于[CSS 字体属性介绍](http://www.w3school.com.cn/css/css_font.asp)
*   W3School 网站上关于[CSS 字体系列介绍](http://www.w3school.com.cn/css/css_font-family.asp)
*   W3School 网站上关于[font-face 的介绍](http://www.w3school.com.cn/css3/css3_font.asp)
*   IBM 在线的[Web development 技术系列文章](http://www.ibm.com/developerworks/cn/web/)
*   [Google Fonts 官方网站](http://www.google.com/fonts/)
*   [Google 网页字体讨论小组](https://groups.google.com/forum/?fromgroups=#!forum/early-access-fonts)
*   [developerWorks Web development 专区](http://www.ibm.com/developerworks/cn/web/)：通过专门关于 Web 技术的文章和教程，扩展您在网站开发方面的技能。
*   [developerWorks Ajax 资源中心](http://www.ibm.com/developerworks/cn/ajax/)：这是有关 Ajax 编程模型信息的一站式中心，包括很多文档、教程、论坛、blog、wiki 和新闻。任何 Ajax 的新信息都能在这里找到。
*   [developerWorks Web 2.0 资源中心](http://www.ibm.com/developerworks/cn/web20/)，这是有关 Web 2.0 相关信息的一站式中心，包括大量 Web 2.0 技术文章、教程、下载和相关技术资源。您还可以通过 [Web 2.0 新手入门](http://www.ibm.com/developerworks/cn/web20/newto/) 栏目，迅速了解 Web 2.0 的相关概念。
*   查看 [HTML5 专题](http://www.ibm.com/developerworks/cn/web/lp/html5/)，了解更多和 HTML5 相关的知识和动向。
