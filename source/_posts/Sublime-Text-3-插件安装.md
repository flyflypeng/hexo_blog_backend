---
title: Sublime-Text-3-插件安装
date: 2016-03-11 00:00:45
tags:
- Sublime
- 常用软件
- Linux
---

## Package Control 简介

Sublime Text 3 之所以强大在于它有许许多多的功能非常丰富的插件，可以适用不同的编程语言的需要。

而 Package Control 就是安装并管理这些插件的工具。在安装好 Package Control 之后，我们就可以通过它来安装其他一系列的插件了。

## Package Control 安装

Package Control 的安装方法也非常简单，官方安装文档：[Package Control INSTALLATION][1]。

如果是 Sublime Text 3 的话，只需要将下面的内容复制到 Sublime 的控制台中（Sublime 的控制台可以通过快捷键 “Ctrl + `”来打开）

```python
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

## 插件安装

这样安装完了之后，我们就可以打开菜单栏中的 "Preferences" 看到下拉菜单中最后一行就是 "Package Control"，如下所示：
![Sublime_Package_Control_Installation][2]

<!--more-->

OK，到现在为止，我们就可以通过 Package Control 工具来安装其他的插件了。

具体的方法如下：
1. 使用快捷键 "Ctrl + Shift + P" 打开命令搜索框。
2. 在文本框中输入 "Package Control: "，然后就会弹出选项让我们去选择安装插件还是卸载插件等等的一系列操作。
3. 如果是安装插件，则选择"Package Control: Install Package"

**注意**
在这里可能会存在一个问题，那就是 Sublime 无法连接到 PackageControl 网站上，无法安装插件。因此，这个问题的解决方法，可以参考下面的解决方案：
[解决sublimeText3无法安装插件问题 -- There are no packages available for installation][3]

解决了这个问题之后，我们就可以愉快地在搜索框中输入自己想要的插件名称了。

## 常用插件列表

### 1. Alignment
代码对齐插件

### 2. C Improved
更加人性化的C语言着色方案

### 3. DocBlockr
自动生成大块的注释，并且可以用tab在不同内容之间切换

### 4. SublimeAStyleFormatter
国人做的Astyle Sublime版，蛮不错的。安装完成之后，下面这个配置一定要打开，即保存自动格式，保存之后就会对代码进行自动按格式对齐。
![保存自动格式化配置][4]
### 5. CTags
代码标签生成器，它支持函数的跳转，用来阅读源码非常方便。安装完 CTags 插件之后还要在 Ubuntu 中安装一个 ctags 的工具。

插件官方地址：[CTags][8]

### 6. SublimeCodeIntel
代码自动补全插件。它支持以下语言的自动提示功能：

>JavaScript, Mason, XBL, XUL, RHTML, SCSS, Python, HTML, Ruby, Python3, XML, Sass, XSLT, Django, HTML5, Perl, CSS, Twig, Less, Smarty, Node.js, Tcl, TemplateToolkit, PHP

插件官方地址：[SublimeCodeIntel][7]

### 7. CHeaders
该插件可以帮助你自动填充/提示 C/C++ 的头文件。 使用方法：该插件在 Linux 系统中会默认从以下两个路径去寻找头文件：

```shell
/usr/include/
/usr/local/include 
```

如果你想再添加一些头文件的查找路径，可以将头文件路径添加到设置文件中的 "PATHS_HEADERS" 参数中
    
```shell
{
     "PATHS_HEADERS" : ["~/Desktop/project/include"],
}
```

为了能够使编写 C++ 代码时，我们还可以添加系统中 gcc-4.8 中有关 C++ 的相关头文件，改完之后的配置文件如下：

```shell
{
    // Path to your c headers files...
    // example linux: 
    //      /home/leoxnidas/include/
    //      ~/include/
    //      $HOME/include/
    // example window: 
    //      C:\
    //      %HOME% <= Same => C:\
    "PATHS_HEADERS" : 
    [ 
        "/usr/include/c++/",
        "/usr/include/c++/4.8/",
        "/usr/include/boost/",
        "/usr/lib/gcc/x86_64-linux-gnu/4.8/include"
    ]
}
```

如果有时插件不能在输入头文件时智能提示相关的头文件，这可能说明插件工作不正常了，所有就需要重新加载插件，我们只需要使用快捷键 "Ctrl + Shift + P" 来打开命令行输入框，并在框中输入以下的命令：

```shell
CHeaders: Load Plugin
```

如下图所示：
![Sublime_CHeader][12]


[github 插件地址][11]

### 8. SideBarFolders
这个插件可以用来管理 Sublime 中打开的文件夹，如下图所示：
![SideBarFolders][6]

### 9. SideBarEnhancements
SideBarEnhancements 是一款很实用的右键菜单增强插件；在安装该插件前，在Sublime Text左侧FOLDERS栏中点击右键，只有寥寥几个简单的功能；安装了就相当于给其丰了大胸一般。

### 10. ConvertToUTF8
支持将编辑的文件保存成各种不同的编码格式，例如: GB2312, GBK, BIG5, EUC-KR, EUC-JP 等

### 11. BracketHighlighter
用于匹配括号，引号和html标签。

### 12. MarkDown Editing
SublimeText不仅仅是能够查看和编辑 Markdown 文件，但它会视它们为格式很糟糕的纯文本。这个插件通过适当的颜色高亮和其它功能来更好地完成这些任务。

### 13. Markdown Preview
Markdown Preview较常用的功能是preview in browser和Export HTML in Sublime Text，前者可以在浏览器看到预览效果，后者可将markdown保存为html文件。
使用方法：按Ctrl + Shift + P
输入mp 后回车(Markdown Preview: current file in browser)
此时就可以在浏览器里看到刚才编辑的文档了

### 14. Emmet 
Emmet 插件对于前端开发者来说可谓大大提高了他们的工作效率，它可以自动地补全 HTML 和 CSS 代码中的闭合标签。
[插件官方地址][9]

### 15. SublimeClang
SublimeClang 是 Sublime Text 中唯一的 C/C++ 代码自动补全插件，功能强大，并且自带语法检查功能。但是这个插件在 Sublime Text 3 中已经停止更新了，所以在 Sublime  Text 3 中就不能使用 Package Control 工具进行下载使用了，只能自己直接从 github 上的仓库中下载下来手动安装。有关安装的步骤，请见另一篇文章。

[点滴记录——Ubuntu 14.04中安装Sublime Text 3并使用SublimeClang插件][10]


## 参考链接
[如何优雅地使用Sublime Text3][5]


[1]: https://packagecontrol.io/installation
[2]: http://7xj51c.com1.z0.glb.clouddn.com/sublime_package_control_install.png
[3]: http://blog.csdn.net/zhyh1986/article/details/40678263
[4]: http://7xj51c.com1.z0.glb.clouddn.com/sublimeAStyleFormatter.png
[5]: http://www.jianshu.com/p/3cb5c6f2421c
[6]: http://7xj51c.com1.z0.glb.clouddn.com/SideBarFolders.png
[7]: http://sublimecodeintel.github.io/SublimeCodeIntel/
[8]: https://github.com/SublimeText/CTags
[9]: https://packagecontrol.io/packages/Emmet
[10]: http://blog.csdn.net/cywosp/article/details/32721011
[11]: https://github.com/leoxnidas/CHeaders
[12]: http://7xj51c.com1.z0.glb.clouddn.com/sublime_CHeaders.png

