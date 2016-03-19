title: "Python学习笔记（3）"
date: 2015-07-23 16:34:35
categories: 技术
tags:
- Python
- 学习
- 笔记
---

![python_logo][python_logo_url]

>### 目录
>1. [模块](#1)
>	1.1 [模块使用](#1.1)
>	1.2 [安装第三方模块](#1.2)
>	1.3 [使用\_\_future\_\_](#1.3)
>2. [面向对象编程](#2)
>	2.1 [类和实例](#2.1)
>	2.2 [访问权限](#2.2)
>	2.3 [继承和多态](#2.3)
>	2.4 [获取对象信息](#2.4)
>3. [面向对象高级编程](#3)
>	3.1 [使用\_\_slots\_\_](#3.1)
>	3.2 [使用@property](#3.2)
>	3.3 [多重继承](#3.3)
>	3.4 [定制类](#3.4)





## 1. 模块 <span id="1"></span>

为了编写可维护的代码，我们把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。在Python中，一个.py文件就称之为一个模块（Module）。

使用模块的好处？

最大的好处是大大提高了代码的可维护性。其次，编写代码不必从零开始。当一个模块编写完毕，就可以被其他地方引用。我们在编写程序的时候，也经常引用其他模块，包括Python内置的模块和来自第三方的模块。

使用模块还可以避免函数名和变量名冲突。相同名字的函数名和变量名可以存在于不同的模块中，因此，我们在编写模块时，不必担心模块名会与其他模块冲突。但是，尽量不要与Python中的内置函数名冲突。[Python内置函数][Python_built_in_function_url]。

<!--more-->

但是你也许会想到 ，如果两个人编写了相同的模块名，那么模块名冲突了怎么办？Python为了避免模块名冲突，引入了包(Package)。

举个例子，一个abc.py的文件就是一个名字叫abc的模块，一个xyz.py的文件就是一个名字叫xyz的模块。

现在，假设我们的abc和xyz这两个模块名字与其他模块冲突了，于是我们可以通过包来组织模块，避免冲突。方法是选择一个顶层包名，比如mycompany，按照如下目录存放:
![module1][module1_url]

引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，abc.py模块的名字就变成了mycompany.abc，类似的，xyz.py的模块名变成了mycompany.xyz。

**请注意，在每一个包目录下都会有有一个叫做\_\_init\_\_.py的文件，这个文件必须存在，否则Python就把这个目录当成一个普通的目录，而不是一个包。\_\_init\_\_.py可以是空文件，也可以有Python代码，因为__init__.py本身就是一个模块，而它的模块名就是mycompany。**

类似的，可以有多层目录，组成多级层次的包结构。比如，下面的目录结构:
![module2][module2_url]

文件www.py的模块名就是mycompany.web.www，两个文件utils.py的模块名分别是mycompany.utils和mycompany.web.utils。

mycompany.web也是一个模块，请指出该模块对应的.py文件。
mycompany.web模块对应的文件就是mycompany.web目录下的\_\_init\_\_.py文件。


### 1.1 模块使用 <span id="1.1"></span>

下面我们就以Python中的内建模块sys为例，编写一个hello模块，由此来讲解Python中模块文件的编写规范。
```python
#!/usr/bin/env python
#-*-coding:utf-8-*-

'a test module'

__author__ = 'woshijpf'

import sys

def test():
	argv = sys.argv
	if len(argv) == 1:
		print 'Hello World!'
	elif len(argv) == 2:
		print 'Hello %s' % (argv[1])
	else:
		print 'Too many arguments!'

if __name__ == '__main__':
	test()
```
第1行和第2行是标准注释，第1行注释可以使得该hello.py文件可以直接在Mac/Linux/Unix上运行，而第2行注释则表示本.py文件采用utf-8编码格式编码。
第4行的字符串是一个字符串，表示模块的文档注释，任何模块文件中第一个字符串都被当做是模块文档注释字符串。
第6行使用\_\_author\_\_变量把作者的信息写进去，然后如果其他人引用你的模块时，就可以瞻仰你的大明明了。
以上就是Python模板的标准模板文件。
从后面开始就是真正的代码部分。

使用sys模块的第一步就是导入sys模块，
```python
import sys
```
导入sys模块后，我们就有了变量sys指向该模块，利用sys这个变量，就可以访问sys模块的所有功能。

sys模块中有一个argv的变量，用list存储了命令行的所有参数。argv变量至少拥有一个元素，因为第一个参数永远都是.py文件的文件名

运行python hello.py获得的sys.argv就是['hello.py']；

运行python hello.py Michael获得的sys.argv就是['hello.py', 'Michael]。

注意上面hello模块中最后两句代码:
```python
if __name__ == '__main__':
	test()
```
当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量\_\_name\_\_置为\_\_main\_\_，而如果在其他地方导入该hello模块时，if判断将失败，因此，这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

下面，我们就可以通过命令行来查看一下运行的效果:
```bash
$ python hello.py
$ Hello World!
$ python hello.py 'Bob'
$ Hello Bob
```
如果启动Python交互环境，然后导入hello模块:
```Python
python
Python 2.7.6 (default, Mar 22 2014, 22:59:56) 
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
```
倒入时，没有打印‘Hello World’，这是因为导入hello模块时，并没有执行test()函数，当执行hello中的test模块时 ，就能够打印处‘Hello World！’
```python
>>> hello.test()
Hello World!
```

#### 别名
当我们导入模块时，我们不一定要使用模块文件名作为导入后的模块名，我们可以给这个模块重新起个名字。比如Python标准库一般会提供StringIO和cStringIO两个库，这两个库的接口和功能是一样的，但是cStringIO是C写的，速度更快，所以，你会经常看到这样的写法:
```python
try:
	import cStringIO as StringIO
except ImportError:
	import StringIO
```
这样就可以优先导入cStringIO。如果有些平台不提供cStringIO，还可以降级使用StringIO。导入cStringIO时，用import ... as ...指定了别名StringIO，因此，后续代码引用StringIO即可正常工作。

#### 作用域
在模块中，我们可以定义很多的函数和变量，但是有时我们不想模块内有的函数和变量被别人直接使用，而是仅仅在模块内部进行使用，在Python中我们可以通过前缀"\_"来实现。


正常的函数和变量名是公开的（public），可以被直接引用，比如: abc，x123，PI等；

类似\_\_xxx\_\_这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的\_\_author\_\_，\_\_name\_\_就是特殊变量，hello模块定义的文档注释也可以用特殊变量\_\_doc\_\_访问，我们自己的变量一般不要用这种变量名；

类似\_\_xxx和\_xxx的变量或者函数名是非公开的(private)，不应该被直接引用，比如\_abc,\_\_abc。

private函数或变量不应该被别人引用，那它们有什么用呢？请看例子:
```python
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```
我们在模块里公开greeting()函数，而把内部逻辑用private函数隐藏起来了，这样，调用greeting()函数不用关心内部的private函数细节，这也是一种非常有用的代码封装和抽象的方法，即:
外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。

**这里的作用域的思想就是使用了面向对象编程中的抽象和封装的特点，就是把具体的实现的细节可以封装起来，对外只提供规定的接口。**

### 1.2 安装第三方模块 <span id="1.2"></span>

在Python中安装第三方模块是通过setuptools工具进行的。Python中封装了两个setuptools的包管理工具: easy_install 和 pip。目前官方推荐使用的是pip。

现在，我们就来安装第一个第三方库PIL(Python Imaging Library)，这是Python下非常强大的图像处理工具库。一般说来，第三方库都会在Python官方的[pypi.python.org][pypi_url]网站注册，要安装一个第三方库，必须先知道该库的名称，可以在官网或者pypi上搜索，比如Python Imaging Library的名称叫PIL，因此，安装Python Imaging Library的命令就是: 
```bash
$ pip install PIL
```
耐心等待并下载完成后，就可以使用PIL库了。

在安装PIL时，我遇到这样一个问题：
```bash
Could not find any downloads that satisfy the requirement PIL
  Some externally hosted files were ignored (use --allow-external PIL to allow).
Cleaning up...
No distributions at all found for PIL
Storing debug log for failure in /home/woshijpf/.pip/pip.log
```
我的系统环境:
> Ubuntu 14.04 64位
> Python 2.7.6

解决办法:
```bash
$ sudo pip install PIL --allow-external PIL --allow-unverified PIL
```
安装成功后的提示界面:
![PIL_install_pic][PIL_install_pic_url]

有了PIL之后，处理图像就变得非常简单，下面就以一个缩小图片为例子来介绍一下PIL的用法:
```python
>>> import Image
>>> im = Image.open('test.png')
>>> print im.format, im.size, im.mode
PNG (400, 300) RGB
>>> im.thumbnail((200, 100))
>>> IOError: decoder jpeg not available
```
但是当执行到"im.thumbnail((200,100))"时就出现了错误“IOError: decoder jpeg not available”，通过Google之后，找到了原因是因为PIL在不同的linux系统中可能存在一些bug，而且现在Python的图像处理库更多人选择使用**pillow**。pillow图像处理库是基于PIL而实现的一个图像处理库，是现在Python环境中流行的图像处理库。

所以在安装pillow之前，我们首先需要卸载掉PIL，使用如下命令:
```bash
$ sudo pip uninstall PIL
```
然后我们再安装pillow
```bash
$ sudo pip install pillow
```
安装成功后的提示界面，如下所示:
![pillow_install][pillow_install_url]

然后，我们在调用Image模块来进行图像处理:
```python
>>> import Image
>>> im = Image.open('robot_cat.jpg')
>>> print im.format,im.size,im.mode
JPEG (550, 550) RGB
>>> im.thumbnail((200,200))
>>> im.save('thumb.jpg','JPEG')
```
最后，我们进入文件夹中，可以查看一下是否生成了"thumb.jpg"的压缩后的图片:
![thumb_jpg][thumb_jpg_url]

到此，经过我们的测试，说明pillow模块已经安装成功了。

在Python中除了我们已经安装好了的pillow库，还有许多常用的第三方库，例如: MySQL的驱动: MySQL-python，用于科学计算的NumPy库: numpy，用于生成文本的模板工具Jinja2，等等

#### 模块的搜索路径

当我们试图加载一个模块时，Python会到指定路径下去寻找对应的.py文件，如果找不到就会报错。
```python
>>> import mymodule
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named mymodule
```

默认情况下，Python会搜索当前工作目录、所有已安装的内置模块和第三方模块，搜索路径存放在sys模块中的path变量中:
```python
>>> import sys
>>> sys.path
['', '/usr/lib/python2.7', '/usr/lib/python2.7/plat-x86_64-linux-gnu', '/usr/lib/python2.7/lib-tk', '/usr/lib/python2.7/lib-old', '/usr/lib/python2.7/lib-dynload', '/usr/local/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages/PILcompat', '/usr/lib/python2.7/dist-packages/gtk-2.0', '/usr/lib/pymodules/python2.7', '/usr/lib/python2.7/dist-packages/ubuntu-sso-client']
```
如果我们要添加自己的模块搜索目录，我们可以有以下两种方法:

第一种，直接些该sys.path变量，添加要搜索的目录:
```python
>>> import sys
>>> sys.path.append('/Users/michael/my_py_scripts')
```
但是这种方法是在运行时修改，运行完之后就失效了。

第二种，设置环境变量PYTHONPATH，该环境变量中的内容会被自动添加到模块搜索路径中。设置方式与设置Path环境变量类似。注意只需要添加你自己的搜索路径，Python自己本身的搜索路径不受影响。

设置Linux系统中的环境变量的一个方法就是修改/etc/profile文件，在文件中添加想要添加的环境变量，然后保存后通过命令"source /etc/profile"来使其生效。下面就举个例子:
```bash
mkdir python_module
sudo gedit /etc/profile
```
然后在**/etc/profile**文件末尾添加上如下的语句:
```
export PYTHONPATH=/home/woshijpf/python_module
```
保存后退出，接着就在命令行中输入下面的语句:
```bash
source /etc/profile
```
最后，我们进入Python的交互环境，导入sys模块，查看sys模块中的path变量是否发生了变化:
```python
>>> import sys
>>> sys.path
['', '/home/woshijpf/python_module', '/usr/lib/python2.7', '/usr/lib/python2.7/plat-x86_64-linux-gnu', '/usr/lib/python2.7/lib-tk', '/usr/lib/python2.7/lib-old', '/usr/lib/python2.7/lib-dynload', '/usr/local/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages/PILcompat', '/usr/lib/python2.7/dist-packages/gtk-2.0', '/usr/lib/pymodules/python2.7', '/usr/lib/python2.7/dist-packages/ubuntu-sso-client']
```
从上面的结果我们可以看到，我们手动添加的Python模块目录的确被添加到了sys模块中的path变量中了。

### 1.3 使用\_\_future\_\_ <span id="1.3"></span>

Python的每个新版本都会增加一些新的功能，或者对原来的功能作一些改动。有些改动是不兼容旧版本的，也就是在当前版本运行正常的代码，到下一个版本运行就可能不正常了。

从Python 2.7到Python 3.x就有不兼容的一些改动，比如2.x里的字符串用'xxx'表示str，Unicode字符串用u'xxx'表示unicode，而在3.x中，所有字符串都被视为unicode，因此，写u'xxx'和'xxx'是完全一致的，而在2.x中以'xxx'表示的str就必须写成b'xxx'，以此表示“二进制字符串”。

Python提供了__future__模块，把下一个新版本的特性导入到当前版本，于是我们就可以在当前版本中测试一些新版本的特性。举例说明如下:
为了适应Python 3.x的新的字符串的表示方法，在2.7版本的代码中，可以通过unicode_literals来使用Python 3.x的新的语法:
```python
# still running on python 2.7

from __future__ import unicode_literals

print '\'xxx\' is unicode?',isinstance('xxx',unicode)
print 'u\'xxx\' is unicode?', isinstance(u'xxx', unicode)
print '\'xxx\' is str?', isinstance('xxx', str)
print 'b\'xxx\' is str?', isinstance(b'xxx', str)
```

注意到上面的代码仍在Python2.7的运行，但结果显示去掉前缀u的'a string'仍是一个unicode，而加上前缀b的b'a string'才变成了str:
```bash
$ python futur_module.py
'xxx' is unicode? True
u'xxx' is unicode? True
'xxx' is str? False
b'xxx' is str? True
```

## 2. 面向对象编程 <span id="2"></span>

面向对象编程--Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过过程把函数细分为子函数，即把大块函数切割成小块函数来降低系统的复杂度，

在Python中所有的数据类型都可以视为对象，当然也可以自定义对象。自定义对象数据类型就是面向对象中的累的概念(class)。

### 2.1 类和实例 <span id="2.1"></span>

面向对象的设计思想是从自然界中来的，因为在自然界中，类(class)和实例(instance)的概念是很自然的。Class是一种抽象概念，比如我们定义一个Class--Student，它指的是学生的概念，而实例(instance)则是一个个具体的Student，比如‘Tom’，‘Peter’是两个具体的Student。

面向对象中最重要的概念就是类（class）和实例（instance），必须牢记类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但是各自的数据可能不同。

#### 类的定义
Python中类的定义是通过关键字**class**实现的，例如，定义一个学生类Student:
```python
 class Student(object):
 	pass
```
class后面紧跟着的是类名，即Student，类名通常是大写开头的单词，紧接着是(object)，表示该类从那个类继承而来，而object是所有类的基类。

创建好类之后，我们可以通过类名+()的形式来构造一个类的实例，例如:

```python
>>> s = Student()
>>> s
<__main__.Student object at 0x7f0871045890>
>>> Student
<class '__main__.Student'>
```
可以看到变量s是一个指向类Student的对象，后面的’0x7f0871045890‘表示的是对象在内存中的地址。

由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的\_\_init\_\_方法（这个\_\_init\_\_方法类似于C++中类的构造函数），在创建实例的时候，就把name，score等属性绑上去:

```python
class Student(object):
	def __init__(self,name,score):
		self.name = name
		self.score = score
```

注意到\_\_init\_\_方法的第一个参数永远都是self，表示创建的实例本身（这个self类似与Java中的this指针），因此，在\_\_init\_\_方法内部，就可以把各种属性绑定到self上，因为self就是指向创建的实例本身。

有了\_\_init\_\_方法，在创建实例的时候，就不能传入空的参数了，必须传入与\_\_init\_\_方法匹配的参数，但self不用传，Python解释器会将实例变量传进去。
```python
>>> import student_class
>>> s = student_class.Student('Tom','60')
>>> s.name
'Tom'
>>> s.score
'60'
```

和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数和关键字参数。


#### 数据封装

面向对象编程的一个重要特点就是数据封装。在上面的Student类中，每个实例就拥有各种的name和score这些数据。我们可以通过函数来访问这些数据，比如打印一个学生的成绩:
```python
>>> def print_score(std):
...     print '%s:%s'%(std.name,std.score)
... 
>>> print_score(s)
Tom:60
```
但是，既然Student实例本身就拥有这些数据，要访问这些数据，就没有必要从外面的函数去访问，可以直接在Student类的内部定义访问数据的函数，这样，就把“数据”给封装起来了。这些封装数据的函数是和Student类本身是关联起来的，我们称之为类的方法:

```python
class Student(object):
	def __init__(self,name,score):
		self.name = name
		self.score = score

	def print_score(self):
		print '%s:%s' % (self.name,self.score)
```

要定义一个方法，除了第一个参数是self外，其他和普通函数一样。要调用一个方法，只需要在实例变量上直接调用，除了self不用传递，其他参数正常传入:
```python
>>> import student_class
>>> s = student_class.Student('Tom','60')
>>> s.print_score()
Tom:60
```

和静态语言不同，**Python允许对实例变量绑定任何数据**，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同:
```python
>>> from student_class import Student
>>> p = Student('Peter',90)
>>> b = Student('Bob',59)
>>> p.age = 20
>>> p.age
20
>>> b.age
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'
```

### 2.2 访问权限 <span id="2.2"></span>
在class内部，可以有属性和方法，而外部代码可以通过直接调用实例变量的方法来操作数据。

如果要让内部属性不被外部访问，可以把属性的名称加上两个下划线"\_\_"，在Python中，实例的变量如果以“\_\_”开头，就变成了一个私有变量(private)，只有类内部的方法能够方法，外部的函数不能访问，所以我们可以将Student类修改成下面这样:
```python
#!/usr/bin/env python
#-*- coding:utf-8 -*-

'a example of class: Student , propety is private'

__author__ = 'woshijpf'


class Student(object):
	def __init__(self,name,score):
		self.__name = name
		self.__score = score

	def print_score(self):
		print '%s:%s' % (self.__name,self.__score)
```
改完之后，对于外部的函数，我们就无法再访问到Student实例变量\_\_name和实例的\_\_score。
```python
>>> import student_class_private
>>> b = student_class_private.Student('Bob',40)
>>> b.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```
这样就确保了外部代码不能够随意地修改对象内部的状态，这样通过访问限制的保护，代码就会更加健壮。

如果外部代码要获取name和score，那么可以给Student类增加get_name和get_score的方法来获取:
```python
	def get_name(self):
		return self.__name

	def get_score(self):
		return self.__name
```
如果又要允许外部修改score，那么可以给Student类增加set_score方法:
```python
	def set_score(self,score):
		if score >= 0 & score <=100:
			self.__score = score
		else:
			raise VauleError('bad score')
```

下面我们就来测试以下，Student类中各个方法

```python
>>> from student_class_private import Student
>>> b = Student('Bob',40)
>>> b.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
>>> b.get_name()
'Bob'
>>> b.get_score()
40
>>> b.set_score(100)
>>> b.get_score()
100
```

需要注意的是，在Python中，变量名类似\_\_xxx\_\_的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用\_\_name\_\_、\_\_score\_\_这样的变量名。

有些时候，你会看到以一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。

双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问\_\_name是因为Python解释器对外把\_\_name变量改成了\_Student\_\_name，所以，仍然可以通过\_Student\_\_name来访问\_\_name变量:
```python
>>> b._Student__name
'Bob'
```
但是强烈建议你不要这么干，因为不同版本的Python解释器可能会把\_\_name改成不同的变量名。

###2.3 继承和多态 <span id="2.3"></span>
在面向对象程序设计中，当我们定义一个class的时候，可以从某个现有的class继承，新的class称为子类(Subclass)，而被继承的class称为基类、父类或者超类(BaseClass,SuperClass)。

例如，我们已经编写了一个名为**Animal**的class，有一个run()方法可以直接打印:
```python
class Animal(object):
	def run(self):
    	print 'Animal is running...'
```
当我们需要编写Dog和Cat类似，就可以直接从Animal类继承:
```python
class Dog(Animal):
	pass

class Cat(Animal):
	pass
```
继承有什么好处？最大的好处就是子类能够获得父类的全部功能。由于Animal实现了run()方法，因此，Dog和Cat作为它的子类，什么事也不干就自动获得了run()方法:
```python
dog = Dog()
dog.run()

cat = Cat()
cat.run()
```
运行的结果如下:
```bash
Animal is running...
Animal is running...
```

从上面的运行结果可以看到，无论是Dog()还是Cat()，它们在运行run()方法的时候，显示的结果都是‘Animal is running...’，但是通常符合逻辑的做法是分别显示‘Dog is running...’和‘Cat is running...’,以此我们需要对代码进行改进:
```python
class Dog(Animal):
	def run(self):
		print 'Dog is runnning...'

class Cat(Animal):
	def run(self):
		print 'Cat is runnning...'
```
再次运行，结果为:
```bash
Dog is runnning...
Cat is runnning...
```

当子类和父类都存在相同的方法run()时，子类的run()方法会覆盖父类的run()方法，在代码运行时，总会调用子类的run()方法。这样我们就获得了继承的另外一个好处就是: 多态。

在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类，例如:
```python
>>> a = Animal()
>>> isinstance(a,Dog)
False
>>> b = Dog()
>>> isinstance(b,Animal)
True
```
要理解多态的好处，我们还需要编写一个函数，这个函数接受一个Animal类型的变量:
```python
>>> def run_twice(animal):
...     animal.run()
...     animal.run()
```
当我们传入Animal的实例时，run_twice()就打印出:
```python
>>> run_twice(Animal())
Animal is running...
Animal is running...
```
当我们传入Dog的实例是，run_twice()就打印出:
```python
>>> run_twice(Dog())
Dog is runnning...
Dog is runnning...
```

多态的好处就是，当我们需要传入Dog、Cat......时，我们只需要接收Animal类型就可以了，因为Dog、Cat......都是Animal类型，然后，按照Animal类型进行操作即可。由于Animal类型有run()方法，因此，传入的任意类型，只要是Animal类或者子类，就会自动调用实际类型的run()方法，这就是多态的意思。

对于一个变量，我们只需要知道它是Animal类型，无需确切地知道它的子类型，就可以放心地调用run()方法，而具体调用的run()方法是作用在Animal、Dog、Cat还是Tortoise对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种Animal的子类时，只要确保run()方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则:

对扩展开放: 允许新增Animal子类；

对修改封闭: 不需要修改依赖Animal类型的run_twice()等函数。

### 2.4 获取对象信息 <span id="2.4"></span>

当我们拿到一个对象的引用时，我们该通过什么方法知道这个对象的另外类型呢？

#### 使用type()
首先，判断对象类型可以使用type()函数:
基本类型都可以用type()判断:
```python
>>> type(123)
<type 'int'>
>>> type('Hello')
<type 'str'>
>>> type(None)
<type 'NoneType'>
```
如果一个变量指向一个函数或者类，也可以通过type()函数来判断:
```python
>>> type(abs)
<type 'builtin_function_or_method'>
>>> type(a)
<class 'inherit_polymorphic_example.Animal'>
>>> type(b)
<class 'inherit_polymorphic_example.Dog'>
```
但是type()函数返回值是什么类型呢？

type()函数返回的是函数参数的类型值常量，Python把每种type类型都定义好了常量，放在type模块里，使用之前需要导入:
```python
>>> import types
>>> type('abc') == types.StringType
True
>>> type(1234) == types.IntType
True
>>> type(u'abc') == types.UnicodeType
True
>>> type(str) == types.TypeType
True
```
最后，注意到有一种类型叫做"TypeType"，所有类型本身的类型就是TypeType，比如:
```python
>>> type(int) == type(str) == types.TypeType
True
```

#### 使用instance()

对于class的继承关系来说，使用type()就很不方便。我们要判断class的类型，可以使用instance()函数。
我们回顾上面的例子，如果继承关系是:
>object -> Animal -> Dog

那么，isintance()就可以告诉我们，一个对新啊该是否是某种类型。我们首先创建2种类型的对象:
```python
>>> a = Animal()
>>> d = Dog()
```
然后，判断:
```python
>>> isinstance(d,Dog)
True
>>> isinstance(d,Animal)
True
```
d虽然是Dog类型，但是由于Dog是从Animal继承而来的，所以，d也还是Animal类型。换句话说，isinstance()判断的是一个对象是否是该类本身，或者位于该类型的父继承链上。

而且能用type()判断的基本类型也能够通过isinstance()判断:
```python
>>> isinstance('a',str)
True
>>> isinstance(u'a',unicode)
True
>>> isinstance('a',unicode)
False
```

并且还可以判断一个变量是否是某些类型中的一种，例如:
```python
>>> isinstance(123,(int,str))
True
>>> isinstance(123,(float,str))
False
```

#### 使用dir()

如果要获得一个对象的所有属性和方法，可以使用dir()函数，它返回一个包含字符串的list，比如，获得一个str对象的所有属性和方法:
```python
>>> dir('ABC')
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count', 'decode', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```
类似\_\_xxx\_\_的属性和方法在Python中都是有特殊用途的，比如\_\_len\_\_方法返回长度。在Python中，如果你调用len（)函数试图获取一个对象的长度，实际上，在len()函数内部，它自动取调用该对象的\_\_len\_\_()方法，所以，下面的代码是等价的:
```python
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```
我们自己写的类，如果也想用len(object)的话，那么就得在类中自己实现一个\_\_len\_\_()方法:
```python
>>> class MyObject(object):
...     def __len__(self):
...         return 100
... 
>>> obj = MyObject()
>>> len(obj)
100
```
仅仅把属性和方法列出来是不够的，配合getattr()、setattr()以及hasattr()，我们可以直接操作一个对象的状态:
```python
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
... 
>>> obj = MyObject()
```
紧接着就可以测试该对象的属性:
```python
>>> hasattr(obj,'x')
True
>>> obj.x
9
>>> hasattr(obj,'y')
False
>>> setattr(obj,'y',19)
>>> hasattr(obj,'y')
True
>>> getattr(obj,'y')
19
>>> obj.y
19
```
也可以获得对象的方法:
```python
>>> hasattr(obj,'power')
True
>>> getattr(obj,'power')
<bound method MyObject.power of <__main__.MyObject object at 0x7f4c398a4e10>>
>>> fn = getattr(obj,'power')
>>> fn()
81
```

对于hasattr()等方法，它们的主要用法是在不确定对象是否有该方法时，进行判断用，例如:
```python
def readImage(fp):
	if hasattr(fp,'read'):
    	return read(fp)
    return None
```
假设我们从文件流fp中读取图像，我们首先要判断该fp对象是否存在read方法，如果存在，则该对象是一个流，如果不存在，则无法读取。

但是请注意，在在Python这类动态语言中，有read()方法，不代表该fp对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要read()方法返回的是有效的图像数据，就不影响读取图像的功能。

## 3. 面向对象高级编程 <span id="3"></span>

### 3.1 使用\_\_slots\_\_ <span id="3.1"></span>

正常情况下，当我们定义了一个class，创建一个class的实例后，我们就可以给该实例绑定任何属性和方法，这就是动态语言的灵活性。先定义class:
```python
>>> class Student(object):
...     pass
```
然后，尝试给实例绑定一个属性:
```python
>>> s = Student()
>>> s.name='Michael'  #动态地给实例绑定一个属性
>>> print s.name
Michael
```
同样，我们也可以给实例绑定一个方法:
```python
>>> def set_age(self,age):  #定义一个函数作为实例方法
...     self.age = age
... 
>>> from types import MethodType
>>> s.set_age = MethodType(set_age,s,Student)  #给实例绑定一个方法
>>> s.set_age(25)  #调用实例方法
>>> s.age #测试结果
25
```
但是，给一个实例绑定的方法，对另外一个实例是不起作用的:
```python
>>> s2 = Student()
>>> s2.set_age(25)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```
为了给所有的实例都绑定该方法，可以给class绑定方法:
```python
>>> def set_score(self,score):
...     self.score = score
... 
>>> Student.set_score = MethodType(set_score,None,Student)
>>> 
```
给class绑定方法后，所有实例均可调用:
```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(59)
>>> s2.score
59
```

通常情况下，上面的set_score方法可以直接定义在class中，但是动态绑定允许我们在程序运行过程中给class加上功能，这在静态语言中是很难实现的。

#### 使用\_\_slots\_\_
但是如果我们想限制class的属性怎么办？比如，只允许对Student类的实例动态地绑定name和age属性，不能够再绑定其他属性。

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的\_\_slots\_\_变量，来限制该class能添加的属性:
```python
>>> class Student(object):
...     __slots__ = ('name','age')  #使用tuple来定义允许绑定的属性名称
... 
```
现在，我们就来测试一下:
```python
>>> s = Student()    #新建一个Student类的实例
>>> s.name = 'Tom'   #绑定属性name
>>> s.age = 20       #绑定属性age
>>> s.score = 99     #绑定属性score
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```
由于‘score’没有被放到特殊变量\_\_slots\_\_中，所以不能绑定score属性，试图绑定score将会得到AttributeError的错误。
\_\_slots\_\_
使用\_\_slots\_\_时一定要注意，\_\_slots\_\_只对当前类起作用，对继承类是不起作用的:
```python
>>> class GraduateStudent(Student):
...     pass
... 
>>> g = GraduateStudent()
>>> g.score = 100
>>> g.score
100
```
除非在子类中也定义__slots__，这样，子类允许定义的属性就是自身的__slots__加上父类的__slots__。

### 3.2 使用@property <span id="3.2"></span>

在绑定属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，导致可以把成绩随便改:
```python
>>> s = Student()
s.score = 9999
```
这显然不合逻辑。为了限制score的范围，可以通过在class的定义中一个set_score()方法来设置成绩，再通过一个get_score()来获取成绩，这样，在set_score()方法里，就可以检查参数了。

但是上面的这种方法略显复杂，没有直接用属性这么简单直接。

因此，Python中就出现了既能检查参数，又可以用类似属性这样简单的方式来访问变量。

Python中内置的@property装饰器就是负责把一个方法变成属性调用:
```python
>>> class Student(object):
...     @property
...     def score(self):
...         return self.__score
...     @score.setter
...     def score(self,value):
...         if not isinstance(value,int):
...             raise VauleError('Score must be an integer!')
...         if vaule < 0 or value > 100:
...				raisr VauleError('score must between 0 ~ 100!')
...         self.__score = value

```
@property的实现比较复杂，我们先考察如何使用。把一个getter方法变成属性，只需要加上@property就可以了，此时，@property本身又创建了另一个装饰器@score.setter，负责把一个setter方法变成属性赋值，于是，我们就拥有一个可控的属性操作:
```python
>>> s = Student()
>>> s.score = 60   #实际转换为s.set_score(60)
>>> s.score        #实际转换为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```
注意到这个神奇的@property，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。

还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性:
```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2014 - self._birth
```
上面的属性中birth属性是一个可读可写的属性，而age就是一个只读属性。

**总之,@property广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行一个必要的检查，这样，程序运行时就减少了出错的可能。**


### 3.3 多重继承 <span id="3.3"></span>

继承是面向对象编程的一个重要的方式，因为通过继承，子类就可以扩展父类的功能。而多继承就是一个子类可以同时继承多个父类，这样就可以同时拥有多个父类的不同功能了。

例如，我们现在要实现下面4种动物:
* Dog - 狗狗
* Bat - 蝙蝠
* Parrot - 鹦鹉
* Ostrich - 鸵鸟

如果按照哺乳动物和鸟类分类，我们可以设计处这样的类层次:
![animal_classification][animal_classification_url]

但是如果按照“能跑”和“能飞”来归类，我们就可以设计出下面这样的层次:
![animal_classification_2][animal_classification_2_url]

如果按照哺乳动物和鸟类进行分类，我们的代码实现就是下面这样:
```python
#大类
class Mammal(Animal):
	pass

class Bird(Animal):
	pass

#各种动物
class Dog(Mammal):
	pass

class Bat(Mammal):
	pass

class Parrot(Bird):
	pass

class Ostrich(Bird):
	pass
```
现在，我们要给动物加上Runnable和Flyable的功能，只需要预先定义好Runnable和Flyable类:
```python
class Runnable(object):
	def run(self):
		print 'Running...'

class Flyable(object):
	def fly(self):
		print 'Flying...'
```
对于需要Runnable功能的动物，就多继承一个Runnable，例如，Dog:
```python
class Dog(Mammal,Runnable):
	pass
```
对于需要Flyable功能的动物，就可以多继承一个Flyable，例如，Bat:
```python
class Bat(Mammal,Flyable):
	pass
```

#### Mixin
在设计类的继承关系时，通常，主线都是单一继承下来的，例如，Ostrich继承自Bird。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让Ostrich除了继承自Bird外，再同时继承Runnable。这种设计通常称之为Mixin。

Mixin的目的就是给一个类增加功能，这样，在设计类的时候，我们优先考虑通过多重继承组合多个Mixin的功能，而不是设计多层次的复杂的继承关系。

Python自带的很多库也使用了Mixin。举个例子，Python自带了TCPServer和UDPServer这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由ForkingMixin和ThreadingMixin提供。通过组合，我们就可以创造出合适的服务来。

比如，编写一个多进程模式的TCP服务，定义如下:
```python
class MyTCPServer(TCPServer,ForkingMixin):
	pass
```
编写一个多线程模式的UDP服务，定义如下:
```python
class MyUDPServer(UDPServer,ThreadingMixin):
	pass
```
### 3.4 定制类 <span id="3.4"></span>

看到类似\_\_slots\_\_这种形如\_\_xxx\_\_的变量或者函数名就要注意，这些在Python中有着特殊的用途。

\_\_slots\_\_我们已经知道怎么用了，\_\_len\_\_()方法我们也知道是为了能够让class作用于len()函数。

除此之外，Python的class中还有许多这样有特殊用途的函数，可以帮助我们定制类。

#### \_\_str\_\_
我们先定义一个Student类，打印一个实例:
```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print Student('Michael')
<__main__.Student object at 0x109afb190>
```
打印出一堆<__main__.Student object at 0x109afb190>，不好看。

怎么才能打印好看呢？只需要定义好\_\_str\_\_()方法，返回一个好看的字符串就可以了:
```python
>>> class Student(object):
...     def __init__(self,name):
...             self.name = name
...     def __str__(self):
...             return 'student object (name:%s)' % self.name
... 
>>> print Student('Tom')
student object (name:Tom)
```

如果我们直接输入变量而不用print，打印出来的实例还是不好看:
```python
>>> s = Student('Tom')
>>> s
<__main__.Student object at 0x7f4c398b3550>
```

这是因为直接显示变量的不是\_\_str\_\_()，而是\_\_repr\_\_()，两者的区别就是\_\_str\_\_()返回用户看到的字符串，而\_\_repr\_\_()返回程序开发者看到的字符串，也就是说，\_\_repr\_\_()是为调试服务的。

解决方法就是在类中定义一个\_\_repr\_\_()。但是通常\_\_str\_\_()和\_\_repr\_\_()的代码是一样的，所以这里可以有一个偷懒的写法:
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```

#### \_\_iter\_\_

如果一个类想被for...in循环，类似list或tuple那样，就必须实现一个\_\_iter\_\_()方法，该方法返回一个迭代对象，然后Python的for循环就会不断调用该迭代对象的next()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。

我们以斐波那契数列为例，写一个Fib类，可以作用于for循环:
```python
class Fib(object):
	def __init__(self):
		self.a = 0       #c初始化两个初值a,b
		self.b = 1

	def __iter__(self):
		return self     #实例本省就是迭代对象，所以就返回了自己

	def next(self):
		tmp = self.a
		self.a = self.b
		self.b = tmp + self.b   #计算下一个值
		if self.a > 20:			   #退出循环的条件
			raise StopIteration()
		return self.a      #返回下一个值
```
现在，我们就把Fib实例作用于for循环:
```python
>>> for n in Fib():
...     print n
... 
1
1
2
3
5
8
13
```
#### \_\_getitem\_\_

Fib实例虽然能作用于for循环，看起来和list有点像，但是，把它当成list来使用还是不行，比如，取第5个元素:
```python
>>> Fib()[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Fib' object does not support indexing
```
要表现得像list那样按照下标取出元素，需要实现\_\_getitem\_\_()方法:
```python
	def __getitem__(self,n):
		a,b = 1,1
		for x in range(n):
			a,b = b,a+b
		return a
```
现在就可以通过下标来访问数列中的任意一项了，下标从0开始:
```python
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[4]
5
>>> f[5]
8
```
但是list中有一个神奇的切片方法，但是对于Fib()却报错，这是因为\_\_getitem\_\_传入的参数可能是一个int，也有可能是一个切片对象，所以需要判断:
```python

	def __getitem__(self,n):
		if isinstance(n,int):
			a,b = 1,1
			for x in range(n):
				a,b = b,a+b
			return a
		if isinstance(n,slice):
			start = n.start
			stop = n.stop
			a,b = 1,1
			L = []
			for x in range(stop):
				if x >= start:
					L.append(a)
				a,b = b,a+b
			return L
```
现在我们就来测试一下Fib的切片功能:
```python
>>> f = Fib()
>>> f[0:5]
[1, 1, 2, 3, 5]
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[4]
5
```
但是上述的切片功能还没有对step参数进行处理，也还没有对负数进行处理，所以还需要实现一个\_\_getitem\_\_()还需要许多工作要做。

此外，如果把对象看成dict，\_\_getitem\_\_()的参数也可能是一个可以作key的object，例如str。

与之对应的是\_\_setitem\_\_()方法，把对象视作list或dict来对集合赋值。最后，还有一个\_\_delitem\_\_()方法，用于删除某个元素。

其实\_\_getitem\_\_()有点类似于C++中的运算符重载，它重载了方括号[]的下标引用的功能，使得我们自定义的对象也可以通过方括号来引用对象，当实际调用f[0]时，它实际上调用的函数是\_\_getitem\_\_(0)。

总之，通过上面的方法，我们自己定义的类表现得和Python自带的list、tuple、dict没什么区别，这完全归功于动态语言的“鸭子类型”，不需要强制继承某个接口。

#### \_\_getattr\_\_
正常情况下，当我们调用类的方法或者属性时，如果不存在，就会报错，例如定义学生类(Student):
```python
>>> class Student(object):
...     def __init__(self):
...             self.name = 'Michael'
... 
```
调用name属性，没有问题，但是当调用不存在的属性score属性时，就存在问题了:
```python
>>> s = Student()
>>> print s.name
Michael
>>> print s.score
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```
错误信息很明确地告诉我们，没有找到score这个attribute。

要避免这个错误，除了可以加上一个score属性之外，Python还有另外一个机制，那就是写一个\_\_getattr\_\_()方法，动态地方茴一个属性。修改如下:
```python
>>> class Student(object):
...     def __init__(self):
...             self.name = 'Michael'
...     def __getattr__(self,attr):
...             if attr == 'score':
...                     return 99
```
当调用不存在的属性时，比如score，Python解释器试图调用\_\_getattyr\_\_(self,'score')来尝试获得属性，这样，我们就有机会返回score属性的值:
```python
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99
```
**注意，只有在没有找到属性的情况下，才调用\_\_getattr\_\_，已有的属性，比如name，不会在\_\_getattr\_\_()**中查找。

Python中更多的定制方法可以详见[Python的参考文档][python_reference_url]。









[Python_built_in_function_url]:https://docs.python.org/2/library/functions.html
[pypi_url]:https://pypi.python.org/pypi
[python_reference_url]:https://docs.python.org/2/reference/datamodel.html#special-method-names
[animal_classification_url]:http://7xj51c.com1.z0.glb.clouddn.com/animal_classification1.png
[animal_classification_2_url]:http://7xj51c.com1.z0.glb.clouddn.com/animal_classification2.png


[PIL_install_pic_url]:http://7xj51c.com1.z0.glb.clouddn.com/PIL_install_pic.png
[pillow_install_url]:http://7xj51c.com1.z0.glb.clouddn.com/pillow_install_pic.png
[thumb_jpg_url]:http://7xj51c.com1.z0.glb.clouddn.com/pillow_execute_result.png



[python_logo_url]:http://7xj51c.com1.z0.glb.clouddn.com/python_logo.png
[module1_url]:http://7xj51c.com1.z0.glb.clouddn.com/module1.png
[module2_url]:http://7xj51c.com1.z0.glb.clouddn.com/module2.png



