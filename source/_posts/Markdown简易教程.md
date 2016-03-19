title: "Markdown简易教程"
date: 2015-07-12 11:01:00
categories: 技术
tags:
- Markdown
---
![Markdown](http://7xj51c.com1.z0.glb.clouddn.com/markdown.jpg)

> **目录**
1. [Markdown简介](#1)
2. [Markdown基本语法](#2)
	+ 2.1 [标题](#2.1)
	+ 2.2 [列表](#2.2)
	+ 2.3 [链接](#2.3)
	+ 2.4 [图片](#2.4)
	+ 2.5 [代码块](#2.5)
	+ 2.6 [引用](#2.6)
	+ 2.7 [粗体和斜体](#2.7)
	+ 2.8 [表格](#2.8)
	+ 2.9 [分隔符](#2.9)
	+ 2.10 [特殊字符插入](#2.10)
3. [Markdown中页内跳转实现](#3)
4. [Markdown编辑器推荐](#4)
5. [Markdown使用小技巧](#5)

## 1. Markdown简介 <span id="1"></span>

Markdown 是一种「电子邮件」风格的「标记语言」，它的目标就是实现「易读易写」，现在Markdown已经成为一种适用于网络的书写语言。
总结Markdown的优点就是:
+ 纯文本，所以兼容性强，可以用所有的文本编辑器打开
+ 然你更专注于文字而不是排版
+ 格式转换方便，Markdown的文本可以很轻松地转换为html语言
+ Markdown标记语法有极强的可读性

本博客所使用的Hexo（博客搭建软件）默认采用的就是Markdown语言作为博文编辑语言，博主编辑好内容后就可以通过Hexo将博文发布到博客网站上。
<!--more-->

## 2. Markdown基本语法 <span id="2"></span>

Markdown标记语言的语法非常简单，只需要多使用就会熟能生巧，作为初学者也可以点击[这个网站][MarkdownOnlineWebsite]在线学习编辑Markdown文档，在这个网站中左边是Markdown语言编辑的文本，右边则实时解析出对应的网页页面，十分方便。

### 2.1 标题 <span id="2.1"></span>

Markdown支持两种标题的语法，类Setext和类atxs形式。
类Setext形式是用底线的形式，利用=（最高阶标题）和-（第二阶标题），例如:
>This is an Head 1
>\================
>This is an Head 2
> \----------------
>
>实际效果
>This is an Head 1
>================
>This is an Head 2
>----------------

任何数量的 = 和 - 都可以有效果。

类 Atx 形式则是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶，例如:
>\#这是H1
>\##这是H2
>\###这是H3
>\####这是H4
>\#####这是H5
>\######这是H6
>
>显示效果
>#这是H1
>##这是H2
>###这是H3
>####这是H4
>#####这是H5
>######这是H6

### 2.2 列表 <span id="2.2"></span>

Markdown支持有序列表和无序列表。
无序列表可以使用加号“+”、“-”或者“*”来作为列表标记:
>\* Red
>\* Green
>\* Blue

等同于:
>\+ Red
>\+ Green
>\+ Blue

也等同于:
>\- Red
>\- Green
>\- Blue

上面三种无序列表的显示效果为:
>* Red
>* Green
>* Blue

有序列表则使用数字接着一个英文句点:
>1. Red
>2. Green
>3. Blue
>
>显示效果就如上所示

### 2.3 链接 <span id="2.3"></span>

Markdown支持两种方式的链接语法: 行内式和参考式。
不管是行内式还是参考式，链接文字都是用[方括号]进行标记。
要建立一个**行内式**的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，例如:
>This is a link of baidu website [baidu]\(www.baidu.com)
>
>显示效果

This is a link of baidu website [baidu](http://www.baidu.com)

**参考式**的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记，例如:
>This is an [example]\[id] reference-style link.
>
>在定义了id之后，需要在文中的末尾定义id对应的链接，例如:
>\[id]: www.baidu.com
>
>显示效果

This is an [example][id] reference-style link.

### 2.4 图片 <span id="2.4"></span>

Markdown使用一种和链接很相似的语法来标记图片，同样也允许两种样式: 行内式和参考式。
**行内式**格式的图片语法如下所示:
>![AltText]\(url/of/image)

详细的叙述如下:
* 一个惊叹号“！”
* 接着一个方括号，里面放上图片的替代文字
* 接着一个普通的括号，里面放上图片的网址

**参考式**的图片语法，则如下所示:
>![AltText]\[id]

「id」是图片参考的名称，图片参考的定义方式和参考式链接中的id定义方式类似，如下所示:
>[id]: url/to/image

### 2.5 代码块 <span id="2.5"></span>

Markdown中用标记符号“```”包围的区块表示代码区块，并且可以在标记符号“```”之后添加上对应程序语言的名称,实现对应代码区块内的关键字高亮，例如:
>\```java
>import java.lang.*
>
>void main()
>{
>	printlf("Hello World");
>}
>```
>显示效果
>

```java
import java.lang.*

void main()
{
	printlf("Hello World");
}
```

### 2.6 引用 <span id="2.6"></span>

Markdown引用是使用类似email中用“>”的引用方式，具体的使用方法就是在引用的段落的开头加上引用的标记符号“>”，例如:
>\>这句话就是引用的！
>
>显示效果
>
>这句话就是引用的！

并且，在引用的区块内同样可以使用Markdown中的其他语法，例如标题、列表、代码区块等

### 2.7 粗体和斜体 <span id="2.7"></span>

Markdown使用“\*”或者“\_”作为标记强调字词的符号，被“\*”或者“\_”包围的字词会解析为表示强调的粗体和斜体。
其中，用一个“\*”包围的字词表示斜体
>\*你好*
>
>显示效果
>
>*你好*

用两个“\*”包围的字词表示粗体
>\*\*你好\*\*
>
>显示效果
>
>**你好**

### 2.8 表格 <span id="2.8"></span>

Markdown中用“|”、“-”和“:”标记符号来构造表格，例如:

>\|    Tables    |      Are      |     Cool    |
>\|--------------|:-------------:|------------:|
>\|    col 3 is  | right-aligned |       $1600 |
>\|    col 2 is  |    centered   |          $12|
>\|     hello    |    how much   |           $5|
>
>显示效果
>
>|    Tables    |      Are      |     Cool    |
>|--------------|:-------------:|------------:|
>|    col 3 is  | right-aligned |       $1600 |
>|    col 2 is  |    centered   |          $12|
>|     hello    |    how much   |           $5|

**总结**:
1. 第二行是设定对应列的对齐方式
2. 通过在标记符号"|"之间设置“:”的位置来实现对齐方式
3. 如果标记符号为“|:-----:|”，则表示居中对齐
4. 如果标记符号为“|------:|”，则表示右对齐
5. 如果标记符号为"|:------|"，则表示左对齐
6. 如果标记符号为“|-------|”，则表示的是默认对齐方式，默认的对齐方式是左对齐

### 2.9 分隔符 <span id="2.9"></span>

Markdown中使用三个以上的"*"、“-”和“_”来建立一个分隔符，例如:
>\*\*\*
>***
>\-\-\-
>------------------------------------------
>
>\_\_\_
>__________________________________________

### 2.10 特殊字符插入 <span id="2.10"></span>

Markdown中可以通过反斜杠"\"来插入一些在语法中有其他意义的符号。
Markdown支持以下这些符号前面加上反斜杠来帮助插入普通的符号:
>\\    反斜杠
>\`    反引号
>\*    星号
>\_    底线
>\{}   花括号
>\()   括弧
>\#    井字号
>\+    加号
>\-    减号
>\.    英文句号
>\!    惊叹号

## 3. Markdown中实现页内跳转 <span id="3"></span>

在写一篇很长的文章时，如果没有一个目录，不仅写作者有时头绪会很乱，而且读者在读整篇文章时也会很困难，因此有一个可以跳转目录就会变得很方便。
由于Markdown标记语言是兼容html语言的，所以在这里可以使用html中的锚来实现页内跳转。
实现方式:
1. 在需要进行跳转的地方用Markdown语言设置跳转的标签:[点击跳转](#1)
2. 在跳转到的地方设置一个锚:< span id="1"></ span>（注意: 这里为了能显示html的标签，因此在< 和span之间加了一个空格，在实际应用时则需要把这个空格去除）。


## 4. Markdown编辑器推荐 <span id="4"></span>

Linux 平台:
* [haroopad][haroopad_url]

Windows平台:
* [Markpad][markpad_url]

在线Markdown编辑网站:
* [Cmd Markdown][cmd_markdown_url]

## 5. Markdown使用小技巧 <span id="5"></span>
1. 在Markdown中输入空格字符。
在Markdown中默认对文本中的空格字符是忽略的，但是有时我们为了达到缩进美观的要求时，我们可以通过输入
>& emsp;

	来实现空格字符的输入。

[MarkdownOnlineWebsite]: https://www.zybuluo.com/mdeditor
[id]: http://www.baidu.com
[haroopad_url]: http://pad.haroopress.com
[cmd_markdown_url]: https://www.zybuluo.com/mdeditor
[markpad_url]: http://code52.org/DownmarkerWPF



