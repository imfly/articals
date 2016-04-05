#### Web 中文字体应用指南

在 Web 上应用字体是一项基本技术，同时也是一门艺术。对于英文字体来说可选择的范围实在是太广泛了，合理的使用它们将会为你的网站增色不少。关于英文字体的使用和搭配技巧，在这里不做赘述，只推荐一套非常好的视频：[Fundamentals of Design _by CodeSchool_](http://www.codeschool.com/courses/fundamentals-of-design)

而真正的挑战在于中文字体，由于中文字体组成的特殊性导致其体积过于庞大，除了操作系统内置的字体之外，我们很难在网站上应用其他的字体。在可选性很差的前提之下，如何正确的使用中文字体呢？

首先，以下的字体声明都是很糟糕的，切忌使用：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="err">"宋体"</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="nt">Arial</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="nt">Helvetica</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">;</span>

<span class="o">...</span>
</pre>

接下来，我们一步一步来说明如何定义好的字体声明。

#### 中文字体也有英文名称

很多开发者忽略了这一点：尽管我们在操作系统中常常看到`宋体`、`微软雅黑`、`华文细黑`这样的字体名称，但实际上这只是字体的显示名称，而不是字体文件的名称。虽然说在大多数情况下直接使用显示名称也有效，但有些用户却工作在一些很极端的情况下，这会导致你的字体声明无效。

比如说，用户安装了中文版的操作系统（这意味着系统有中文字体），但是却切换到了以英文为主要语言——这种情况在那些希望加强英语锻炼的中文用户当中是很常见的。这时候，操作系统很有可能无法按照显示名称找到正确的字体，所以我们要记住的第一件事情就是： **同时声明中文字体的字体名称（英文）和显示名称（中文）**，就像这样：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="nt">STXihei</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">;</span>
</pre>

#### 永远不要忘记声明英文字体，并且英文字体应该在中文字体之前

记住这个事实：绝大部分中文字体里包含英文字母（但是基本上都很丑），而英文字体里不包含中文字符。

在网页里中/英文混排是很常见的，你绝对不会喜欢用中文字体显示英文的效果，所以一定不要忘了先声明英文字体：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Georgia</span><span class="o">,</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="nt">Arial</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">;</span>
</pre>

另外还有一个好习惯，就是在最后补充英文字体族的名称。字体族大体上分为两类：非衬线和衬线，它们之间的区别和使用规则请见本文开始介绍的视频。一般来说，你应该这么做：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Georgia</span><span class="o">,</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="nt">serif</span><span class="o">;</span>

<span class="nt">font-family</span><span class="o">:</span> <span class="nt">Arial</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">,</span> <span class="nt">sans-serif</span><span class="o">;</span>
</pre>

请注意：以上两句声明中的`宋体`和`微软雅黑`不应该调换（尽管调换了也不会发生错误），这是因为从字体的式样来看，`微软雅黑`是非衬线的，而`宋体`才是衬线的。然而中文并不像英文那样严格区分字体族，所以这一点在实际应用当中并不那么重要。

#### 别忘了照顾不同的操作系统

作为一个 Web 开发者，你理应对 Windows, Mac OS, Linux 家族等常用操作系统里的系统字体有足够的了解，特别是中文。在这里，我们假设目标网站要同时给予 windows 用户和 mac 用户最好的字体体验，于是我们可以这样声明：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Helvetica</span><span class="o">,</span> <span class="nt">Tahoma</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">,</span> <span class="nt">STXihei</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">,</span> <span class="nt">sans-serif</span><span class="o">;</span>
</pre>

这句声明都做到哪些事情呢？让我们一一说明（括号内代表其对应的目标操作系统）：

1.  对于英文字符，首先查找`Helvetica`(Mac)，然后查找`Tahoma`(Win)，都找不到就用`Arial`(Mac&Win)；若是以上三者都缺失，则使用当前默认的`sans-serif`字体(操作系统或浏览器指定)；
2.  对于中文字体，我们已经了解其规则了。`华文细黑`(Mac)，`微软雅黑`(Win)是这两个平台的默认中文字体。

#### 注意向下兼容

到此为止，我们的字体声明已经很不错了——如果你不必考虑还在使用旧版本操作系统的用户的话。遗憾地是，中文市场还有大量的用户在使用 Windows XP，`宋体`才是他们的主要中文字体。为了照顾到这些用户，你可以为`微软雅黑`增加一个 fallback：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Helvetica</span><span class="o">,</span> <span class="nt">Tahoma</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">,</span> <span class="nt">STXihei</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="nt">Heiti</span><span class="o">,</span> <span class="err">"黑体"</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">,</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="nt">sans-serif</span><span class="o">;</span>
</pre>

同样地，你看到我们也为 Mac 系统使用了`黑体`作为 fallback。

#### 其他

#### 不加双引号可以吗？

可以。有些英文字体的名称多于两个单词，因为单词中间有空格所以需要用 `""` 包裹起来。中文字体很特别，按照英文的角度来看，像`微软雅黑`究竟算是一个词还是四个词呢？没关系，好在中文字体的名称里没有空格，所以 `""` 不加也没什么大碍。

不过，谁都不能保证在任何操作系统/浏览器环境下都是如此，若是发生了奇怪的事情，不妨加上双引号试试看。

#### 可以默认显示某种字体吗？比如`微软雅黑`

你可能注意到了，在我们最后的字体声明里，`华文细黑`是默认字体（如果你的系统上安装了声明里所有的中文字体的话），为什么我要先声明 Mac 系统的字体呢？

按理来说，大多数网站的主要目标市场还是 Windows 用户的，所以理论上这个才是合理的声明：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Helvetica</span><span class="o">,</span> <span class="nt">Tahoma</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">,</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="nt">STXihei</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="nt">Heiti</span><span class="o">,</span> <span class="err">"黑体"</span><span class="o">,</span> <span class="nt">sans-serif</span><span class="o">;</span>
</pre>

但实际上却并非如此。在中文字体的用户群体里，很大一部分拥有 Mac 的人都同时安装了 Win 下常用的中文字体（这得归功于 Office for Mac）；但极少有 Win 用户去安装 Mac 下的中文字体。

因此，把 Mac 用字体声明在前面几乎不会对 Win 用户产生什么影响（因为他们压根没有！），倒是用来做 fallback 的`黑体`可能会取代`微软雅黑`的位置，所以更保险的做法或许是这样：

<pre class="highlight css"><span class="nt">font-family</span><span class="o">:</span> <span class="nt">Helvetica</span><span class="o">,</span> <span class="nt">Tahoma</span><span class="o">,</span> <span class="nt">Arial</span><span class="o">,</span> <span class="nt">STXihei</span><span class="o">,</span> <span class="err">"华文细黑"</span><span class="o">,</span> <span class="err">"</span><span class="nt">Microsoft</span> <span class="nt">YaHei</span><span class="err">"</span><span class="o">,</span> <span class="err">"微软雅黑"</span><span class="o">,</span> <span class="nt">SimSun</span><span class="o">,</span> <span class="err">"宋体"</span><span class="o">,</span> <span class="nt">Heiti</span><span class="o">,</span> <span class="err">"黑体"</span><span class="o">,</span> <span class="nt">sans-serif</span><span class="o">;</span>
</pre>

但无论如何请不要把`微软雅黑`放在中文字体的最前面，作为史上最丑陋的中文字体之一，`微软雅黑`实在不是什么好的选择，请照顾一下被 Mac 宠坏的用户吧，谢谢！（仅代表个人观点）

BTW，如果你也像我一样不喜欢呆头呆脑的`微软雅黑`，那就干脆把它删了吧~

到此为止，虽然在我们的示例代码里没有包含 Linux 家族的例子，不过相信你也明白该怎么做了吧。

#### 一点补充

鉴于一些人对`微软雅黑`的排位产生异议，我不妨把上文的解决方案再延伸一步。事实是这样子的：

1.  把`微软雅黑`放前面，会导致安装了`微软雅黑`字体的 Mac 用户不得不面对`微软雅黑`，而在 Mac 下比`微软雅黑`优雅得多的中文字体比比皆是；

2.  把 Mac 下的字体放前面，也会对 Windows 用户造成差不多的困惑，毕竟`微软雅黑`是 Windows 平台下显示效果最好的字体（目前为止）；

1 和 2，哪一种出现的概率更大一些？我想这是一个不需要计算就能知道的答案吧？

但是——的确还有另外两个因素在纠结着：

1.  不少 Windows 用户因为各种原因关闭了 `ClearType`，在此情形下`微软雅黑`将会惨不忍睹！但是 Mac 的字体也不是好的选择，真正的胜出者？猜对了，`宋体`。

2.  绝大部分 Mac 下的黑体在 Windows 下模糊不清，而`微软雅黑`虽然丑但在 Mac 下至少能看。（间接体现了两个平台的字体渲染技术的差距）

所以在实践中，真正接近“万无一失”的方案需要考虑以下几点：

1.  利用 UA 判断为不同的平台加载不一样的字体声明；

2.  除非有特别的原因，否则尽量保持正文用`宋体`，标题和其他可以放大些的地方用`微软雅黑`（针对 Windows）；

3.  Mac 下的`冬青体`效果极佳，但是该字体在 Mac OS X 10.6 以前是没有的，所以谨慎考虑你的用户群体，或者使用`华文黑体`系列做 fallback；

最后，我不想再和任何人争论字体的优劣，本文的目的是介绍使用方法而不是字体选择。“美”或“丑”向来都是很主观的事情，只因为我是作者，所以我免不了会有倾向性，然而我也相信你自己会有正确的判断，和我较真没有任何实际意义。


----------------

原文：https://ruby-china.org/topics/14005