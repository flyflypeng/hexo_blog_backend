title: "Python 学习笔记（4）"
date: 2015-07-25 19:04:23
categories: 技术
tags:
- Python
- 学习
- 笔记
---
![python_logo][python_logo_url]

>### 目录
>1. [错误、测试和调试](#1)
>	1.1 [错误处理](#1.1)
>	1.2 [调试](#1.2)
>	1.3 [单元测试](#1.3)
>2. [IO编程](#2)
>	2.1 [文件读写](#2.1)
>	2.2 [操作文件和目录](#2.2)
>	2.3 [序列化](#2.3)

## 1. 错误、测试和调试 <span id="1"></span>

### 1.1 错误处理 <span id="1.1"></span>

Python中内置和许多高级语言一样的try...except...finally...的错误处理机制，下面我们就通过一个例子来看看Python中的错误处理机制:
```python
try:
	print 'try...'
	r = 10/0
	print 'result',r
except ZeroDivisionError,e:
	print 'except:',e
finally:
	print 'finally...'
print 'END'
```
当我们认为某些代码可能会出错时，就可以用try来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转到错误处理代码，即except语句块，执行完except之后，如果有finally语句块，则执行finally语句块，至此，执行完毕。

<!--more-->

上面的代码中在计算10/0时会产生一个除法运算错误:
```python
try...
except: integer division or modulo by zero
finally...
END
```
从输出的结果我们可知，当发生错误时，后续的语句**print 'result',r**就不再被执行，except由于捕获到**ZeroDivisionError**，因此被执行。最后，finally语句块被执行，然后，程序继续按照流程接下去执行。

我们有时很好奇为什么try...except...finally...语句块中，为什么还要有finally语句块，这不是可有可无的吗？

的确finally语句块有时在程序里的确不起什么作用，例如上面的例子中我们的finally语句块中就打印了一个“finally”的提示信息，并没有什么特殊作用。但是，但是，当我们遇到某些情况时，例如，我们对一个文件进行写操作，但是程序在try语句块中出现了错误，抛出了错误，但是呢，这个错误并没有被except语句块捕捉到，这时，我们就可以在finally语句块中，写上关闭文件的代码，以便其他程序还能对该文件进行读写操作，所以这就是finally语句块的作用。它通常是用来关闭一些文件和数据库连接，或者是释放网络连接等。


此外，如果没有错误发生，我们可以在except语句块之后加上一个else，当没有错误发生时，就会自动执行else语句:
```python
try:
    print 'try...'
    r = 10 / int('a')
    print 'result:', r
except ValueError, e:
    print 'ValueError:', e
except ZeroDivisionError, e:
    print 'ZeroDivisionError:', e
else:
    print 'no error!'
finally:
    print 'finally...'
print 'END'
```

Python的错误其实也是类(class)，所有的错误类型都继承自**BaseException**，所以在使用except时需要注意的是，它不但捕获该类型的错误，还把其子类也“一网打尽”，例如:
```python
try:
	foo()
except StandardError,e:
	print 'StandardError'
except ValueError,e:
	print 'ValueError'
```

第二个except永远也捕获不到ValueError，因为ValueError是StandardError的子类，如果有错误产生，就会被第一个except捕获到。

Python中所有的错误都是从BaseException类派生而来，常见的错误类型和继承关系，请点击下面的链接查看[https://docs.python.org/2/library/exceptions.html#exception-hierarchy][Exception_about_url]

#### 调用堆栈
如果错误没有被捕获，他就回一直向上抛，最后被Python解释器捕获，打印一个错误信息，然后程序退出，下面我们就以一个例子为例:
```python
def foo(s):
	return 10/int(s)

def bas(s):
	return foo(s)*2

def main():
	bas('0')

main()
```
执行的结果如下所示: 
```python
Traceback (most recent call last):
  File "err.py", line 10, in <module>
    main()
  File "err.py", line 8, in main
    bas('0')
  File "err.py", line 5, in bas
    return foo(s)*2
  File "err.py", line 2, in foo
    return 10/int(s)
ZeroDivisionError: integer division or modulo by zero
```
从上面的运行结果我们可以看出，当我们的程序出错时，Python解释器会打印出整个错误的调用函数链:
>main()  -->  bas() -->  foo()  -->  最后到出错的源头

#### 抛出错误
因为错误是一个class，捕获一个错误就是捕获到了该class的一个实例。因此，错误并不是凭空产生的，而是有意创建并被抛出的。Python的内置函数会抛出很多类型的错误，而我们自己定义的函数也可以抛出错误。

我们可以抛出两种错误，一种是自己自定义的错误类，另一种是Python内置的错误类型，例如:ValueError、ZeroDivisionError等。

Python中抛出错误采用的是**raise**关键字，例如:
```python
class FooError(StandardError):
	pass

def foo(s):
	n = int(s)
	if n == 0:
		raise FooError('invalid value:%s' % s)
	return 10 / n

foo(0)
```
执行完上面的代码之后，我们就可以得到这样的错误提示信息:
```python
Traceback (most recent call last):
  File "raise_err.py", line 10, in <module>
    foo(0)
  File "raise_err.py", line 7, in foo
    raise FooError('invalid value:%s' % s)
__main__.FooError: invalid value:0
```
最后，我们再来看一种错误的处理方式:
```python
def foo(s):
	n = int(s)
	return 10/n

def bar(s):
	try:
		return foo(s)*2
	except StandardError,e:
		print 'Error'
		raise

def main():
	bar('0')

main()
```
上面代码的执行结果是:
```python
Error
Traceback (most recent call last):
  File "err2.py", line 15, in <module>
    main()
  File "err2.py", line 13, in main
    bar('0')
  File "err2.py", line 7, in bar
    return foo(s)*2
  File "err2.py", line 3, in foo
    return 10/n
ZeroDivisionError: integer division or modulo by zero
```

在bar()函数中，我们明明已经捕获了错误，但是打印一个Error后，又把错误通过raise语句抛出去了，这是为什么呢？

其实这种错误的处理方式不但没有错，还非常常见。在bar中捕获错误，其实本质上只是记录一下，便于后面进行跟踪。但是由于当前函数并不知道怎么处理该错误，所以，最恰当的方式就是将错误继续向上抛出，让顶层的调用者去处理。


### 1.2 调试 <span id="1.2"></span>

程序写完并且能够一次运行通过的概率很小，总是会存在着各种各样的bug需要我们去修正，这样的一个过程就叫做调试。调试的方法有很多种，下面就依次来介绍下各种调试的方法。

#### 使用print输出关键变量
这种方法非常简单粗暴，就是使用print语句输出有关关键变量的有关信息，例如:
```python
# err.py
def foo(s):
    n = int(s)
    print '>>> n = %d' % n
    return 10 / n

def main():
    foo('0')

main()
```
执行后在输出中查找打印的变量值:
```python
>>> n = 0
Traceback (most recent call last):
  File "debug1.py", line 9, in <module>
    main()
  File "debug1.py", line 7, in main
    foo('0')
  File "debug1.py", line 4, in foo
    return 10 / n
ZeroDivisionError: integer division or modulo by zero
```
但是，使用print最大的坏处就是你将来还得将print语句删除掉，想想程序里到处都是print，运行结果也会包含很多的垃圾信息。

#### 断言
凡是用print来辅助查看的地方，都可以使用断言**assert**来代替:
```python
def foo(s):
    n = int(s)
    assert n!=0,'n is zero!'
    return 10 / n

def main():
    foo('0')

main()
```
assert的意思是断言（按照我自己的理解来讲就是预言，预测的意思，它可以根据断言后面的表达式来判断后续的代码是否会出错），在本例子中，表达式n!=0应该是True，否则后面的代码就要出错。如果断言失败，assert语句就会抛出AssertError:
```python
Traceback (most recent call last):
  File "debug2.py", line 9, in <module>
    main()
  File "debug2.py", line 7, in main
    foo('0')
  File "debug2.py", line 3, in foo
    assert n!=0,'n is zero!'
AssertionError: n is zero!
```
如果程序中也到处充斥着assert，和print相比也好不到那里去，但是可以通过运行py文件时带上参数**-O**来关闭断言功能:
```python
$ python -O debug2.py 
Traceback (most recent call last):
  File "debug2.py", line 9, in <module>
    main()
  File "debug2.py", line 7, in main
    foo('0')
  File "debug2.py", line 4, in foo
    return 10 / n
ZeroDivisionError: integer division or modulo by zero
```
关闭断言功能后，你可以把程序中所有的assert语句当做pass语句来看。

#### logging
把print替换为logging是第三种方式，和assert相比，logging不会抛出错误，而且可以输出到文件:
```python
import logging
logging.basicConfig(level=logging.INFO)

s='0'
n = int(s)
logging.info('n = %d' % n)
print 10/n
```
运行之后，我们可以看到下面的结果:
```python
INFO:root:n = 0
Traceback (most recent call last):
  File "debug3.py", line 7, in <module>
    print 10/n
ZeroDivisionError: integer division or modulo by zero
```
这就是logging的好处，它允许你指定记录信息的级别，有debug,info,warning,error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。

logging还有一个好处就是通过简单的配置，一条语句可以同时输出到不同的地方，比如console和文件。


#### pdb
第四种方式就是Python解释器自带的pdb调试器，它可以让程序一步一步执行，并可以随时查看运行状态。我们先准备好程序:
```python
s='0'
n = int(s)
print 10/n
```
然后启动pdb调试器:
```python
$ python -m pdb debug4.py
> /home/woshijpf/Nutstore/学习/Python学习/Python_Code/Exception_Processing/debug4.py(1)<module>()
-> s='0'
```
以参数**-m pdb**启动后，pdb定位到下一步要执行的代码"-> s = '0'"。输入命令**l**来查看代码:
```python
(Pdb) l
  1  ->	s='0'
  2  	n = int(s)
  3  	print 10/n
[EOF]
```
输入命令**n**可以单步执行代码:
```python
(Pdb) n
> /home/woshijpf/Nutstore/学习/Python学习/Python_Code/Exception_Processing/debug4.py(2)<module>()
-> n = int(s)
(Pdb) n
> /home/woshijpf/Nutstore/学习/Python学习/Python_Code/Exception_Processing/debug4.py(3)<module>()
-> print 10/n
```
并且，在任何时候都可以输入命令**p 变量名**来查看变量:
```python
(Pdb) p s
'0'
(Pdb) p n
0
```
输入命令***q**可以结束调试，退出程序:
```python
(Pdb) q
```

这种通过pdb在命令行调试的方法理论上是万能的，但实在是太麻烦了，如果有一千行代码，要运行到第999行得敲多少命令啊。还好，我们还有另一种调试方法。

#### pdb.set_trace()设置断点
这种方法也是使用pdb，但是不需要单步执行，我们只需要import pdb，然后，在可能出错的地方放一个pdb.set_trace()，就可以设置一个断点:
```python
import pdb

s='0'
n = int(s)
pdb.set_trace()  #程序运行到这行就会自动暂停
print 10/n
```
运行代码，程序会自动在pdb.set_trace()处暂停，并进入pdb调试环境，可以用命令p查看变量，或者用命令c继续运行:
```python
$ python debug5.py 
> /home/woshijpf/Nutstore/学习/Python学习/Python_Code/Exception_Processing/debug5.py(6)<module>()
-> print 10/n
(Pdb) n
ZeroDivisionError: 'integer division or modulo by zero'
```
#### IDE
如果想非常简单直接地设置断点和单步执行，这时一个支持调试功能的IDE就非常强大，目前python中比较好用的IDE是Pycharm。

### 1.3 单元测试 <span id="1.3"></span>

单元测试是用来对一个模块、一个函数或者一个类进行正确性检验的测试工作。

下面我们就编写一个Dict类，这个类的行为和dict一致，但是可以通过属性来访问，用起来就像下面这样:
```python
>>> d = Dict(a=1, b=2)
>>> d['a']
1
>>> d.a
1
```
mydict.py的代码如下:
```python
class Dict(dict):
	def __init__(self,**kw):
		super(Dict,self).__init__(**kw)

	def __getattr__(self,key):
		try:
			return self[key]
		except KeyError:
			raise AttributeError(r"Dict object has no attribute %s" % key)

	def __setattr__(self,key,value):
		self[key] = value
```
为了编写测试单元，我们需要引入Python自带的unittest模块，编写mydict_test.py文件:
```python
import unittest

from mydict import Dict

class TestDict(unittest.TestCase):
	def test_init(self):
		d = Dict(a=1,b='test')
		self.assertEquals(d.a,1)
		self.assertEquals(d.b,'test')
		self.assertTrue(isinstance(d,dict))

	def test_key(self):
		d = Dict()
		d['key'] = 'value'
		self.assertEquals(d.key,'value')

	def test_attr(self):
		d = Dict()
		d.key = 'value'
		d.assertTrue('key' in d)
		d.assertEquals(d['key'],'value')

	def test_keyerror(self):
		d = Dict()
		with self.assertRaise(KeyError):
			value = d['empty']

	def test_attrerror(self):
		d = Dict()
		with self.assertRaise(AttributeError):
			value = d.empty
```
编写单元测试时，我们需要编写一个测试类，从unittest.TestCase继承。

**以test开头的方法就是测试方法，不易test开头的方法不被认为是测试方法，测试的时候不会被执行。**

对每一类测试都需要编写一个test_xxx()的方法。由于unittest.TestCase类内置许多条件判断，我们只需要调用这些方法就可以断言输出是否是我们所期望的，我们最常用的断言就是assertEquals: 
```python
self.assertEquals(abs(-1),1)   #断言 函数返回的结果与1相等
```
另一种重要的断言就是期待抛出指定类型的Error，比如通过d['empty']访问不存在的key时，断言会抛出KeyError:
```python
with self.assertRaise(KeyError):
	vlaue = d['empty']
```
而通过d.empty访问不存在的key时，我们期待抛出AttributeError:
```python
with self.assertRaise(AttributeError):
	value = d.empty
```

#### 运行单元测试
最简单的运行单元测试的方法，就是在我们所编写的测试类文件的末尾加上下面两句代码:
```python
if __name__ == '__main__':
	unittest.main()
```
这样就可以把mydict_test.py当做普通的python脚本运行:
```bash
$ python mydict_test.py
```

另一种更加常见的方法是在命令行，通过参数-m unittest直接运行单元测试:
```bash
$ python -m unittest mydict_test.py
```
这是推荐的做法，因为这样可以同时一次批量运行很多的单元测试，并且，有很多工具可以自动来运行这些单元测试。

运行得到的结果为:
```python
======================================================================
ERROR: test_attr (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 20, in test_attr
    d.assertTrue('key' in d)
  File "mydict.py", line 9, in __getattr__
    raise AttributeError(r"Dict object has no attribute %s" % key)
AttributeError: Dict object has no attribute assertTrue

======================================================================
ERROR: test_attrerror (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 30, in test_attrerror
    with self.assertRaise(AttributeError):
AttributeError: 'TestDict' object has no attribute 'assertRaise'

======================================================================
ERROR: test_keyerror (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 25, in test_keyerror
    with self.assertRaise(KeyError):
AttributeError: 'TestDict' object has no attribute 'assertRaise'

----------------------------------------------------------------------
Ran 5 tests in 0.000s

FAILED (errors=3)
```

#### setUp和tearDown()
可以子啊单元测试中编写两个特殊的函数setUp()和tearDown()。这两个方法会分别在调用每一个测试方法的前后分别被执行。

setUp()和tearDown()方法有什么用呢？设想你的测试需要启动一个数据库，这时，就可以在setUp()方法中连接数据库，在tearDown()方法中关闭数据库，这样，不必在每个测试方法中重复相同的代码:
```python
	def setUp(self):
		print 'setUp...'

	def tearDown(self):
		print 'tearDown...'
```
可以再次运行测试，看看每个测试方法调用前是否会打印处setUp...和tearDown...。
再次运行的结果为:
```python
setUp...
EtearDown...
setUp...
EtearDown...
setUp...
tearDown...
.setUp...
tearDown...
.setUp...
EtearDown...

======================================================================
ERROR: test_attr (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 20, in test_attr
    d.assertTrue('key' in d)
  File "mydict.py", line 9, in __getattr__
    raise AttributeError(r"Dict object has no attribute %s" % key)
AttributeError: Dict object has no attribute assertTrue

======================================================================
ERROR: test_attrerror (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 30, in test_attrerror
    with self.assertRaise(AttributeError):
AttributeError: 'TestDict' object has no attribute 'assertRaise'

======================================================================
ERROR: test_keyerror (mydict_test.TestDict)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "mydict_test.py", line 25, in test_keyerror
    with self.assertRaise(KeyError):
AttributeError: 'TestDict' object has no attribute 'assertRaise'

----------------------------------------------------------------------
Ran 5 tests in 0.001s

FAILED (errors=3)
```

总结一些单元测试:
1. 单元测试可以有效地测试某个程序模块的行为，是未来重构代码的信心保证。
2. 单元测试的测试用例要覆盖常用的输入组合、边界条件和异常。
3. 单元测试代码要非常简单，如果测试代码太复杂，那么测试代码本身就可能有bug。
4. 单元测试通过了并不意味着程序就没有bug了，但是不通过程序肯定有bug。

## 2. IO编程 <span id="2"></span>

IO编程中，Stream（流）是一个非常重要的概念，可以把流想象成一个水管，数据就是水管里的水，但是只能单向流动。Input Stream就是数据从外面（磁盘、网络）流进内存，Output Stream就是数据从内存留到外面去。

由于CPU和内存的速度原原高于外设的速度，所以在IO编程中，就会存在着严重的速度不匹配的问题。举个例子来说，比如要把100M的数据写入到磁盘中，CPU输出100M的数据只需要0.01秒，可是磁盘接收这100M数据可能需要10秒，怎么办呢？下面就有两种解决方案:
第一种是让CPU等着，也就是程序暂停执行后续的代码，等待100M数据在10s后写入磁盘，再接着往下执行，这种模式称为同步IO。

第二种是不让CPU等待，只是告诉磁盘，“您老慢慢写，不着急，我接着干别的事情去了”，于是，后续的代码就会继续执行，这种模式称为异步IO。

同步IO和异步IO的最重要的区别就是: 是否等待IO执行的结果。在本章中介绍的是同步IO，有关异步IO会在以后分析时，进行介绍。

### 2.1 文件读写 <span id="2.1"></span>

读写文件是最常见的IO操作，Python内置了读写文件的函数，用法和C是兼容的。

####读文件
要以读文件的模式打开一个文件对象，使用Python内置的open()函数，传入文件名和标示符:
```python
>>> f = open('hello.txt','r')
```
其中标示符"r"表示读，这样，我们就成功地打开文件了。

文件打开成功后，我们就可以通过read()函数一次性读取文件中的全部内容，Python把文件中的内容读取到内存，并用一个str对象表示:
```python
>>> f.read()
'Hello World!\n'
```
完成文件读取操作之后，我们还要记得关闭文件，Python中采用close()函数来关闭打开的文件。
```python
>>> f.close()
```

由于文件在读写时都有可能会产生IOError，一旦出错，后面的f.close()就不会被调用了。所以，为了保证无论是否出错都能正确关闭文件，我们可以使用try...finally...来实现:
```python
try:
	f = open('/path/to/file','r')
    print f.read()
finally:
	if f:
    	f.close()
```
但是，如果每次都这么写，那也实在是太繁琐了，所以，Python中引入了with语句来帮助我们调用close()方法:
```python
with open('path/to/file','r') as f:
	print f.read()
```
这和前面的try...finally...的效果是一样的，但是代码更加简洁，并且不用调用f.close()方法。

读取文件有以下几个方法，大家可以根据具体情况，具体要求来选择合适的方法:

| 文件读取方法       |              描述              |
|------------------|--------------------------------|
|    read()        |  一次性读取文件的全部内容          |
|    read(size)    |   每次最多读取size个字节的内容     |
|    readline()    |   每次读取一行内容                |
|    readlines()   |   一次性读取所有内容并按行返回list  |

如果文件很小，read()一次性读取最方便；如果不能够确定大小，反复调用read(size)最为保险；如果是配置文件，调用readlines()，最为方便。
```python
for line in f.readlines():
	print(line.strip())   #把每行末尾的换行符‘\n’都去掉
```

#### file-like Object
向open()函数返回的这种有个read()方法的对象，在Python中统称为file-like Object。除了file之外，还可以是内存的字节流、网络流、自定义流等等。file-like Object不要求从某个类继承，只要写个read()方法就可以了。

StringIO就是在内存中创建的file-like Object，常用作临时缓冲区。

#### 二进制文件

前面讲的默认都是读取文本文件，并且是ASCII编码的文本文件。要读取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可:
```python
>>> f = open('test.jpg','rb')
>>> f.read()
'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节
```

#### 字符编码
如果想要读取非ASCII编码的文本文件，就必须以二进制模式打开，再解码。比如utf-8编码的文件:
```python
>>> f = open('hello.txt','rb')
>>> u = f.read().decode('utf-8')
>>> u
u'Hello World!\n\u4f60\u597d\uff0c\u4e16\u754c\uff01\n'
>>> print u
Hello World!
你好，世界！

```
如果每次都这么手动转换编码嫌麻烦（写程序怕麻烦是好事，不怕麻烦就会写出又长又难懂又没法维护的代码），Python还提供了一个codecs模块帮我们在读文件时自动转换编码，直接读出unicode:
```python
>>> import codecs
>>> with codecs.open('hello.txt','r','utf-8') as f:
...     u = f.read()
...     print u
... 
Hello World!
你好，世界！

```
#### 写文件

写文件和读文件是一样的，唯一的区别就是调用open()函数时，传入标识符‘w’和‘wb’表示写文本文件或者写二进制文件:
```python
>>> with open('test.txt','w') as f:
...     f.write('Hello World!')
```
然后你就可以打开test.txt文件，看看是否写入了‘Hello World!’。
![test_txt][test_txt_url]

如果我们想向文件写入中文怎么办呢？
```python
>>> with open('test.txt','w') as f:
...     f.write('你好，世界！')
```
**这种方法写入的中文采用的是utf-8编码**

如果我们想写入指定编码的文本到文件中，可以模仿codecs的例子来进行写入，例如现在我们想写入一个GBK编码格式的文本，我们可以这样写入:
```python
>>> with codecs.open('test.txt','w','gbk') as f:
...     f.write(u'你好，世界！')
```
写完之后，我们打开文本文件，我们看到的是这样的:
![test_txt2][test_txt2_url]

而当我们选择GBK编码格式时，结果是这样的:
![test_txt3][test_txt3_url]

在这里还要注意一点，请注意我们写入的语句:
```python
>>> f.write(u'你好，世界！')
```

这里为什么要用unicode编码呢，我如果直接这样写呢:
```python
>>> f.write('你好，世界！')
```
如果是这种方式，写入带有中文的字符串就会抛出下面这样的错误:
```python
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
  File "/usr/lib/python2.7/codecs.py", line 688, in write
    return self.writer.write(data)
UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)
```
根据提示，错误的原因就是我们想写入的字符串‘你好，世界！’无法用ASCII码进行编码，所以需要写成unicode的形式。

有关Python中的打开文件open()函数的标示符的详细介绍请点击下面的链接:[https://docs.python.org/2/library/io.html?highlight=open#io.open][open_func_url]

### 2.2 操作文件和目录 <span id="2.2"></span>

在Python中如果想执行目录和文件的操作，可以使用Python内置的**os**模块或者直接调用操作系统的相关接口函数。

下面我们就打开Python交互式命令行，来看看如何使用os模块的基本功能:
```python
>>> import os
>>> os.name
'posix'
```
如果是posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统。

要获取详细的系统信息，可以调用uname()函数:
```python
>>> os.uname()
('Linux', 'woshijpf', '3.16.0-30-generic', '#40~14.04.1-Ubuntu SMP Thu Jan 15 17:43:14 UTC 2015', 'x86_64')
```
注意uname()函数在Windows上不提供，也就是说，os模块的某些函数是跟操作系统相关的。

#### 环境变量

在操作系统中定义的环境变量，全部保存在**os.environ**这个dict中，可以直接查看:
```python
>>> os.environ
{'MANDATORY_PATH': '/usr/share/gconf/ubuntu.mandatory.path', 'XDG_GREETER_DATA_DIR': '/var/lib/lightdm-data/woshijpf', 'GNOME_DESKTOP_SESSION_ID': 'this-is-deprecated', 'UPSTART_EVENTS': 'started starting', 'LESSOPEN': '| /usr/bin/lesspipe %s', 'COLORTERM': 'gnome-terminal', 'ADBHOST': '192.168.1.100', 'QT_IM_MODULE': 'xim', 'LOGNAME': 'woshijpf', 'USER': 'woshijpf', 'PATH': '/usr/local/MATLAB/R2014a/bin:/home/woshijpf/android/adt-bundle-linux-x86_64-20140702/ndk:/home/woshijpf/android/adt-bundle-linux-x86_64-20140702/sdk/platform-tools:/home/woshijpf/android/adt-bundle-linux-x86_64-20140702/sdk/tools:/usr/local/java/jdk1.8.0_05/bin:/usr/local/java/jdk1.8.0_05/jre/bin:/usr/local/java/jdk1.8.0_05:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games'}
```

如果想要获取某个环境变量的值，则可以通过os.getenv()函数进行获取:
```python
>>> os.getenv('USER')
'woshijpf'
```

#### 操作文件和目录

操作文件和目录的函数一部分放在os模块中，一部分放在os.path模块中，这一点需要注意一下。查看、创建和删除目录可以这么调用:
```python
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/home/woshijpf/Nutstore/\xe5\xad\xa6\xe4\xb9\xa0/Python\xe5\xad\xa6\xe4\xb9\xa0/Python_Code/Exception_Processing'
# 在某个目录下创建一个新目录，
# 首先把新目录的完整路径表示出来:
>>> os.path.join('/home/woshijpf/Nutstore/\xe5\xad\xa6\xe4\xb9\xa0/Python\xe5\xad\xa6\xe4\xb9\xa0/Python_Code/Exception_Processing','testdir')
'/home/woshijpf/Nutstore/\xe5\xad\xa6\xe4\xb9\xa0/Python\xe5\xad\xa6\xe4\xb9\xa0/Python_Code/Exception_Processing/testdir'
# 然后创建一个目录:
>>> os.mkdir('/home/woshijpf/Nutstore/\xe5\xad\xa6\xe4\xb9\xa0/Python\xe5\xad\xa6\xe4\xb9\xa0/Python_Code/Exception_Processing/testdir')

# 删掉一个目录:
>>> os.rmdir('/home/woshijpf/Nutstore/\xe5\xad\xa6\xe4\xb9\xa0/Python\xe5\xad\xa6\xe4\xb9\xa0/Python_Code/Exception_Processing/testdir')
```

把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符。在Linux/Unix/Mac下，os.path.join()返回这样的字符串:
```python
part-1/part-2
```
而在Windows系统中则会返回这样的字符串:
```python
part-1\part-2
```
同样的道理，如果我们想要拆分路径时，也不要直接u拆字符串，而可以通过os.path.split()函数，这样就可以把一个路径拆分成两部分，后一部分总是最后级别的目录或者文件名:
```python
>>> import os
>>> os.path.split('/home/woshijpf/eclipse.png')
('/home/woshijpf', 'eclipse.png')
```

os.path.splitext()函数还可以让你直接获得文件的扩展名，有时候这种操作就非常方便:

```python
>>> os.path.splitext('/home/woshijpf/eclipse.png')
('/home/woshijpf/eclipse', '.png')
```

这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作。

文件操作则可以使用下面的函数。假定当前目录下有一个test.txt的文件:
```python
# 对文件重命名:
>>> os.rename('test.txt', 'test.py')
# 删掉文件:
>>> os.remove('test.py')
```

Python中的复制文件的函数并在os模块中，而是在shutil模块中，你还可以在shutil模块中找到许多实用函数，它们可以看做是os模块的补充。

最后看看如何利用Python的特性来过滤文件。比如我们要列出当前目录下的所有目录，只需要一行代码:
```python
>>> [x for x in os.listdir('.') if os.path.isdir(x)]
['.kde', '.pki', 'OpenCV', '.PyCharm40', '.dbus', '.wireshark', 'gedit_textmate', '.local', 'pycharm-4.5.3', 'android', '.sunpinyin', '.gvfs', '\xe9\x9f\xb3\xe4\xb9\x90', 'ffmpeg_sources', 'openvpn', 'temp', '\xe5\x9b\xbe\xe7\x89\x87', '.purple', '.compiz', '.java', '.synaptic', 'Matlab', '.gstreamer-0.10', '.npm', '.gnome', '\xe8\xa7\x86\xe9\xa2\x91', 'HexoBlog', 'Java', '.android', '.cache', '.openshot', 'opencv-2.4.10', '.gconf', '\xe6\xa1\x8c\xe9\x9d\xa2', 'Nutstore', '.gem', '.subversion', '.pip', '.matlab', '.python-eggs', '.thunderbird', '.ssh', '.nutstore', '\xe6\x96\x87\xe6\xa1\xa3', 'PycharmProjects', 'python_module', 'pidgin-lwqq', 'themes', '.swt', '.macromedia', '\xe5\x85\xac\xe5\x85\xb1\xe7\x9a\x84', '.mozilla', '.gimp-2.8', '.config', 'goagent', 'android-4.2.2_r1', '.pcb', '\xe4\xb8\x8b\xe8\xbd\xbd', '\xe6\xa8\xa1\xe6\x9d\xbf', '.adobe', '.gphoto', '.kingsoft', 'laserkbd', 'software', '.thumbnails']
```

如果想要列出所有的.py文件，也只需要一行代码:
```python
>>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
['test.py']
```

### 2.3 序列化 <span id="2.3"></span>

我们把变量从内存中变成可存储或传输的过程称之为序列化，在Python中pickling，在其他语言中也被称为之为serialization，marshalling，flattening等等，都是一个意思。

序列化之后，就可以把序列化后的内容写入到磁盘中，或者通过网络传输到别的机器上。

反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。

Python提供两个模块来实现序列化: cPickle和pickle。这两个模块功能是一样的，区别在于cPickle是用C语言写的，速度快，pickle则是用纯Python写的，速度慢，跟cStringIo和StringIO一个道理，用的时候，先尝试导入cPickle，如果失败再导入Pickle:
```python
try:
	import cPickle as pickle
except ImportError:
	import pickle
```

首先，我们尝试把一个对象序列化并写入文件:
```python
>>> d = dict(name='Bob',age=20,score=88)
>>> pickle.dumps(d)
"(dp1\nS'age'\np2\nI20\nsS'score'\np3\nI88\nsS'name'\np4\nS'Bob'\np5\ns."
```

pickle.dumps()方法可以把任意可序列化的对象序列化成一个字符串，然后，就可以把这个str写入文件。或者用另外一个方法pickle.dump()直接把对象序列化后写入一个file-like Object:
```python
>>> f = open('dump.txt','wb')
>>> pickle.dump(d,f)
>>> f.close()
```
查看写入到dump.txt文件中内容都是一堆乱七八糟的，这些都是Python保存的对象内部信息。

当我们要把对象从磁盘读到内存时，可以先把内容读到一个str，然后用pickle.loads()方法反序列化出对象，也可以直接用pickle.load()方法从一个file-like Object中直接反序列化出对象，例如我们把刚才序列化写入dump.txt文件中的对象，使用pickle.load()方法将其反序列化成对象:
```python
>>> f = open('dump.txt','rb')
>>> d = pickle.load(f)
>>> d
{'age': 20, 'score': 88, 'name': 'Bob'}
>>> f.close()
```
我们可以看到变量的内容又回来了！但是这个变量和我们原来那个变量是完全不相干的对象，它们只是内容相同而已。

Pickle的问题和所有其他编程语言特有的序列化问题一样，就是它只能用于Python，并且可能不同版本的Python彼此都不兼容，因此，只能用Pickle保存那些不重要的数据，不能成功地反序列化也没关系。

#### JSON

如果我们想要在不同的编程语言之间传递对象，就必须把对象序列化成标准的格式，比如XML，但更好的方法是序列化为JSON，因为JSON表示出来的就是一个字符串，可以被所有的语言读取，也可以方便地存储到磁盘或者通过网络传输。JSON不仅是标准格式，并且比XML更快，而且可以直接在Web页面中读取，非常方便。

JSON表示的对象就是标准的JavaScript语言对象，JSON和Python内置的数据类型对应如下:

|     JSON类型     |     Python类型     |
|-----------------|--------------------|
|      {}         |        dict        |
|      []         |        list        |
|   "String"      |        'str'或者u‘unicode’   |
|    1234.56      |     int或float     |
|  true/false     |     True/False     |
|    null         |        None        |

Python内置的json模块提供了非常完善的Python对象到JSON格式的转换。我们先看看如何把Python对象变成一个JSON:
```python
>>> d = dict(name='Bob',age=20,score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```
dumps()方法返回一个str，内容就是标准的JSON。类似的，dump()方法可以直接把JSON写入一个file-like Object。

要把JSON反序列化为Python对象，用loads()或者对应的load()方法，前者把JSON的字符串反序列化，后者从file-like Object中读取字符串并反序列化:
```python
>>> json_str='{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)
{u'age': 20, u'score': 88, u'name': u'Bob'}
```
有一点需要注意，就是反序列化得到的所有字符串对象默认都是unicode而不是str。由于JSON标准规定JSON编码是UTF-8，所以我们总能正确地在Python的str或者unicode与JSON的字符串之间进行转换。

#### JSON进阶

Python中的dict对象是可以直接序列化成JSON的{}，但是很多时候，我们自定义的类的对象是无法直接序列化。例如，我们自定义一个Student对象，然后序列化:
```python
import json

class Student(object):
	def __init__(self,name,age,score):
		self.name = name
		self.age = age
		self.score = score

s = Student('Bob',20,88)

print json.dumps(s)
```
运行代码，我们则会得到这样一个结果:
```bash
File "/usr/lib/python2.7/json/encoder.py", line 184, in default
    raise TypeError(repr(o) + " is not JSON serializable")
TypeError: <__main__.Student object at 0x7fd5d3ffa710> is not JSON serializable
```
导致错误的原因就是Student对象不是一个可序列化的JSON对象。

我们分析json库中的dumps()方法的参数列表可知，除了第一个必须的obj参数是必选参数之外，dumps()方法还提供了一大堆可选的参数，有关函数说明，详见下面的链接:[https://docs.python.org/2/library/json.html#json.dumps][json_dumps_url]

dumps()方法中的可选参数defult()就是把任意一个对象变成一个可序列化的JSON对象，我们只需要为Student专门写一个转换函数，再把转换函数传进去即可:
```python
def student2dict(std):
	return {
		'name':std.name,
		'age':std.age,
		'score':std.score
	}

s = Student('Bob',20,88)

print json.dumps(s,default=student2dict)
```

这样，Student类的实例就会首先被student2dict()函数转换成dict，然后在被序列化成JSON。

不过，下次如果遇到一个Teacher类的实例，照样无法序列化为JSON。我们可以偷个懒，把任意class的实例变为dict:
```python
print(json.dumps(s,default=lambda obj:obj.__dict__))
```

因为通常class实例都有一个\_\_dict\_\_属性，它就是一个dict，用来存储实例变量。也有少数例外，比如定义了\_\_slots\_\_的类。

同样的道理，如果我们要把JSON反序列化为一个Student对象实例，loads()方法首先转换出一个dict对象，然后，我们传入的object_hook函数负责把dict转换成Student实例。
```python
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

json_str = '{"age": 20, "score": 88, "name": "Bob"}'
print(json.loads(json_str, object_hook=dict2student))
```
最后，运行得到的结果为:
```bash
$ python student_pickling.py 
<__main__.Student object at 0x7f30d3b80810>
```
打印出的是反序列化后的Student实例对象。











[Exception_about_url]:https://docs.python.org/2/library/exceptions.html#exception-hierarchy
[json_dumps_url]:https://docs.python.org/2/library/json.html#json.dumps

[python_logo_url]:http://7xj51c.com1.z0.glb.clouddn.com/python_logo.png
[test_txt_url]:http://7xj51c.com1.z0.glb.clouddn.com/test_txt.png
[test_txt2_url]:http://7xj51c.com1.z0.glb.clouddn.com/test_txt2.png
[test_txt3_url]:http://7xj51c.com1.z0.glb.clouddn.com/test_txt3.png
[open_func_url]:https://docs.python.org/2/library/io.html?highlight=open#io.open

