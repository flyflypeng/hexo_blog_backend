title: "Python 学习笔记（2）"
date: 2015-07-20 22:09:56
categories: 技术
tags:
- Python
- 学习
- 笔记
---

![python_logo][python_logo_url]

>### 目录
>1. [函数](#1)
>	1.1 [函数调用](#1.1)
>	1.2 [函数定义](#1.2)
>	1.3 [空函数](#1.3)
>	1.4 [参数检查](#1.4)
>	1.5 [返回多个返回值](#1.5)
>	1.6 [函数的参数](#1.6)
>2. [高级特性](#2)
>	2.1 [切片](#2.1)
>	2.2 [迭代](#2.2)
>	2.3 [列表生成式](#2.3)
>	2.4 [生成器](#2.4)
>3. [函数式编程](#3)
>	3.1 [高阶函数](#3.1)
>	3.2 [返回函数](#3.2)
>	3.3 [匿名函数](#3.3)
>	3.4 [装饰器](#3.4)
>	3.5 [偏函数](#3.5)
>


## 1. 函数 <span id="1"></span>

### .1 函数调用 <span id="1.1"></span>
Python中内置了许多函数，在python程序中我们可以直接通过函数名调用，例如: abs,chr,ord等等。我们也可以从Python的官方文档中查看到内置函数的具体信息:[https://docs.python.org/2/library/functions.html#abs][python_built-in_function_url]

Python中函数的函数名其实本质上是一个指向函数对象的引用，完全可以把函数名赋值给一个变量，这相当于给函数换了个“别名”，例如:
```python
>>> a = abs
>>> a(-1)
1
>>> a(100)
100
```
<!--more-->
在使用Python中内置的函数时，需要根据函数的参数定义，传入合适的参数。

### 1.2 函数定义 <span id="1.2"></span>

在Python中，定义一个函数需要使用def语句，依次写出函数名、括号、括号中的参数和冒号“:“，然后在缩进块中编写函数体，函数的返回值用return语句返回，下面就以自定义的求绝对值函数my_abs为例:
```python
def my_abs(x):
    if x >= 0:
    	return x
    else:
    	return -x
```

### 1.3 空函数 <span id="1.3"></span>

如果想定义一个什么也不做的空函数，可以采用**pass**语句:
```python
def nop():
	pass
```
pass语句什么也不做，实际上pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。

### 1.4 参数检查 <span id="1.4"></span>

Python解释器可以检查我们调用自定义函数时的**参数的个数**是否正确，但它不会检查我们调用函数时函数参数的类型是否正确，所以我们在自定义函数时，最好还要添加上参数类型检查。

在Python的内置函数中有一个函数叫做isinstance()，这个函数是用来判断一个变量是否是某种类型数据的实例，例如我们将上面的my_abs函数进行该进，为其添加上参数检查功能:
```python
def my_abs(x):
    if not isinstance(x,(int,float)):
    	raise TypeError('bad operand type')
    if x >= 0:
    	return x
    else:
    	return -x
```

### 1.5 返回多个返回值 <span id="1.5"></span>

在Python中支持返回多个返回值，看起来这与我们以前C语言中一个函数只能返回一个返回值有很大的不同啊？但是，实际上呢，Python中函数返回多个函数值，其实本质上是将多个返回值，组成一个tuple类型的数据返回，所以，本质上函数的返回值还是一个。
例如:
```python
import math

def move(x,y,step,angle=0):
    nx = x + step*math.cos(angle)
    ny = y + step*math.sin(angle)
    return nx,ny

x,y = move(100,100,60,math.pi/6)
print x,y

r = move(100,100,60,math.pi/6)
print r
```

最终运行得到的结果是:
>151.961524227 130.0
>(151.96152422706632, 130.0)

### 1.6 函数的参数 <span id="1.6"></span>

Python的函数定义非常简单，但灵活度却非常大。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数，使得函数定义出来的接口，不但能处理复杂的参数，还可以简化调用者的代码。

#### 默认参数
下面就以X的N次幂为例，介绍函数默认参数的用法:
```python
def power(x,n=2):
	s = 1
    while n>0:
    	s = s*x
        n = n -1
    return s
    
print 'power(5) = ',power(5)
print '\npower(5,2) = ',power(5,2)
```
当我们调用power(5)时，就相当调用了power(5,2)，最后运行的结果为:
>power(5) =  25

>power(5,2) =  25

**使用默认参数时的注意事项:**
第一，必选参数在前，默认参数在后，否则Python解释器会报错。
第二，当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。
第三，默认参数一定要指向不可变对象。

#### 可变参数
可变参数，顾名思义就是函数中参数的个数是可变的，不固定的，可以是0个，1个，2个等等。

**可变参数的定义方法是:只要在函数的参数列表前加上一个星号*即可，例如: **
```python
def calc(*numbers):
	sum = 0
    for x in numbers:
    	sum += x*x
    return sum
    
print 'calc(1,2) = ',calc(1,2)
print 'calc() = ',calc()
```
最后运行得到的结果是:
>calc(1,2) =  5
>calc() =  0

#### 关键字参数
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个**含参数名的参数**，这些关键字参数在函数内部自动组装为一个dict。关键字参数的定义: 在函数的形参前加上两个星号"**"，请看示例:
```python
def person(name,age,**kw):
    print 'name:',name,'age:',age,'others:',kw
```

函数person在被调用时可以只传入必选参数，也可以传入任意个数的关键字参数。
```python
def person(name,age,**kw):
    print 'name:',name,'age:',age,'others:',kw

print person('Bob',15)
print person('Adam',35,job='engineer',gender='m')
print person('Peter',20,city='ShangHai')
```
运行的结果是:
>name: Bob age: 15 others: {}
>None
>name: Adam age: 35 others: {'gender': 'm', 'job': 'engineer'}
>None
>name: Peter age: 20 others: {'city': 'ShangHai'}
>None

在上面的运行结果中，含有空值“None”的输出的原因是函数person的默认返回值为None。

关键字参数有什么用？它可以扩展函数的功能。比如，在person函数里，我们保证能接收到name和age这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。

#### 参数的组合
在Python中定义函数，可以有必选参数、默认参数、可变参数和关键字参数，这4中参数可以一起使用，也可以使用其中的某一些，但是它们在函数参数列表中的定义的先后顺序必须是: 必选参数、默认参数、可变参数、关键字参数。

比如，定义一个函数包含上面的四种参数:
```python
def func(a,b,c=0,*args,**kw)
	print 'a = ',a,'b = ',b,'c = ',c,'args = ',args,'kw = ',kw
```

在函数调用时，Python解释器按照参数名和参数位置将参数传递进去
```python
def func(a,b,c=0,*args,**kw):
	print 'a=:',a,'b=:',b,'c=',c,'args=:',args,'kw=:',kw

func(1,2)

func(1,2,c=3)

func(1,2,3,'a','b')

func(1,2,3,'a',5)

func(1,2,3,'a','b',x=99)
```

最后运行得到的结果是:
>a=: 1 b=: 2 c= 0 args=: () kw=: {}
a=: 1 b=: 2 c= 3 args=: () kw=: {}
a=: 1 b=: 2 c= 3 args=: ('a', 'b') kw=: {}
a=: 1 b=: 2 c= 3 args=: ('a', 5) kw=: {}
a=: 1 b=: 2 c= 3 args=: ('a', 'b') kw=: {'x': 99}

对于任意的函数都可以通过func(\*args,**kw)的方式进行调用，无论它的函数参数定义是什么样的，例如:

```python
args = (1,2,3,4)
kw = {'x':99}
func(*args,**kw)
```

最后运行得到的结果是:
>a=: 1 b=: 2 c= 3 args=: (4,) kw=: {'x': 99}

最后，特别要注意的是
\*args是可变参数，接受的是一个tuple
**kw 是关键字参数，接受的是一个dict

可变参数既可以直接传入: func(1, 2, 3)，又可以先组装list或tuple，再通过\*args传入: func(\*(1, 2, 3))；

关键字参数既可以直接传入:func(a=1, b=2)，又可以先组装dict，再通过\*\*kw传入: func(**{'a': 1, 'b': 2})。

使用\*args和\*\*kw是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。


## 2. 高级特性 <span id="2"></span>

Python的设计思想就是简单，能够用一行代码解决的问题，绝对不用5行代码取解决，所以Python中就有许多非常简洁又非常方便地一些高级特性，下面就一一作个介绍。

### 2.1 切片 <span id="2.1"></span>
Python中可以通过切片操作非常方便地取出list或者tuple中的想要的元素，而不必很繁琐地通过索引和循环的方式来获取元素。

例如，我们取一个list中前3个元素，我们就可以用以下的一行语句来解决:
```python
>>> L = ['Michael','Bob','James','Peter']
>>> L[0:3]
```
L[0:3]表示从索引0开始取，直到索引3时结束，但是不包括索引3，所以最后取得元素是L[0],L[1],L[2]共三个元素。

其中L[0:3]也可以简写为L[:3]。

和取List的倒数索引类似，Python也支持倒数的切片方式，例如:
```python
>>> L[-2:-1]
['James']
>>> L[-3:-1]
['Bob', 'James']
```
Python还支持每隔几个元素的切片操作，例如:
```python
>>> L
['Michael', 'Bob', 'James', 'Peter', 'Fred', 'Tom']
>>> L[::2]
['Michael', 'James', 'Fred']
>>> L[0:6:3]
['Michael', 'Peter']
```

其中L[0:6:3]表示的意思是从索引0开始取，每隔3个元素取一个元素，直到取到索引位置为6为止，但是不包括索引位置6。

对字符串同样可以采用切片操作，例如:
```python
>>> str = 'Hello World!'
>>> str
'Hello World!'
>>> str[:6]
'Hello '
```
 
### 2.2 迭代 <span id="2.2"></span>
Python中的迭代是指: 能够通过for循环来遍历可迭代的对象，在Python中可以迭代的对象有list，tuple，dict，字符串等。

例如，dict类型对象的迭代方法如下所示:
```python
d = {'a':1,'b':2,'c':3}

print d

for key in d:
    print key
```
最后运行得到的结果是:
>{'a': 1, 'c': 3, 'b': 2}
a
c
b

因为dict的存储不是按照list的方式进行排列的，所以输出的结果可能不一样。

判断一个对象是不是可迭代的对象的方法是: 通过collections模块的iterable类型判断，例如:
```python
>>> from collections import Iterable
>>> isinstance('abc',Iterable)
True
>>> isinstance([1,2,3],Iterable)
True
>>> isinstance(123,Iterable)
False
```

如果要对list实现类似Java那样的下标循环怎么办？Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身:
```python
for i,value in enumerate(['A','B','C']):
	print i,value
```
最后运行得到的结果是:
>0 A
1 B
2 C

### 2.3 列表生成式 <span id="2.3"></span>
列表生成式即List Comprehensions，它是Python内置的非常简单却十分强大的List生成式。

按照前面所学习的知识，在生成List对象时，我们可能会直接初始化赋值给出List中的数据元素，也可能通过range函数来构造List，或者通过for循环和List的append函数来构造list，但是上面这些方法，都有些繁琐，在Python中就是怎么简单就怎么来，下面我们就通过列表生成式的例子来介绍列表生成式:
```python
>>> [x*x for x in range(1,11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
上面这个例子就给我们演示了生成1-10数字的x*x的方法，是不是很简单呢？

**写列表生成式时，首先要把生成的元素放在前面，后面再根for循环，就能创建出list了**

而且我们也可以通过列表生成式，开生成全排列，例如:
```python
>>> [m+n for m in 'ABC' for n in '123']
['A1', 'A2', 'A3', 'B1', 'B2', 'B3', 'C1', 'C2', 'C3']
```

列出当前目录下的所有文件和目录，我可以通过下面这一行简单的代码就能实现了:
```python
>>> import os
>>> [d for d in os.listdir('.')]
['define_generator.pyc', 'iteration_example.py~', 'iteration_example.py', 'fibonacci.py', 'define_generator.py', 'fibonacci.pyc']
```

### 2.4 生成器 <span id="2.4"></span>
通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器（Generator）。

创建generator的方法有很多，第一种方法就是把生成式中的[]换成()就可以了，由此就能够生成一个generator，例如:
```python
>>> L = [x*x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x*x for x in range(10))
>>> g
<generator object <genexpr> at 0x7f425f70ecd0>
```
list可以通过索引或者迭代的方式进行访问，那么如何访问generator中的元素呢？
前面讲到，generator的目的就是为了节约内存空间，它的元素不是全部一次性就计算出来的，而是通过保存了算法，然后根据算法一步一步地计算出来的。因此，generator中采用了next()方法来获取generator中的元素，例如:
```python
>>> g.next()
0
>>> g.next()
1
>>> g.next()
4
>>> g.next()
9
>>> g.next()
16
>>> g.next()
25
>>> g.next()
36
>>> g.next()
49
>>> g.next()
64
>>> g.next()
81
>>> g.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
当generator计算到最后一个元素之后，如果还继续调用next()方法，generator就会抛出一个StopIteration的错误。

但是上面这种调用generator的方法的确有些复杂，由于generator对象也是一个可迭代的对象，所以我们可以通过for循环来访问generator中的元素，例如:
```python
>>> g = (x*x for x in range(10))
>>> for n in g:
...     print n
...
0
1
4
9
16
25
36
49
64
81
```

创建generator的第二种方法就是: 如果一个函数定义中包含了yield关键字，那么这个函数就不在是一个普通函数，而是一个generator。

在这里最难理解的就是generator的执行顺序和函数的执行顺序不一样。函数是顺序执行，遇到return语句或者执行完函数的最后一行语句后返回。而变成generator的函数，在每次遇到next()的时候执行，遇到yield语句返回，再次执行时从上一次返回的yield语句处接下去继续执行，直到遇到函数体中的return语句或者执行到函数体的最后一行语句才停止，下面就举个简单的例子:
```python
>>> def odd():
...     print 'step 1'
...     yield 1
...     print 'step 2'
...     yield 2
...     print 'step 3'
...     yield 3
... 
>>> o = odd()
>>> o.next()
step 1
1
>>> o.next()
step 2
2
>>> o.next()
step 3
3
>>> o.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

generator是非常强大的工具，在Python中，可以简单地把列表生成式改成generator，也可以通过函数实现复杂逻辑的generator。

## 3. 函数式编程 <span id="3"></span>

### 3.1 高阶函数 <span id="3.1"></span>
变量可以指向函数，函数的参数又可以接受变量，所以当一个函数A可以接受另一个函数B作为参数时，我们就称函数A为高阶函数。例如，
```python
>>> def add(x,y,f):
...     return f(x)+f(y)
... 
>>> print add(-10,10,abs)
20
```
上面的例子中，函数add就是一个高阶函数，它能够接受一个函数作为参数。

**把函数当做参数传入到另外一个函数中，这样的函数就称为高阶函数。**

#### map/reduce高阶函数的用法
Python中内建了map()和reduce()函数，有关Map/Reduce的概念，请详见google的一片论文，[MapReduce: Simplified Data Processing on Large Clusters][mapreduce_paper_url]

##### map
map函数接收两个参数，一个是函数，一个是序列，map将接收到的函数依次作用于序列中的每个元素，并把结果作为新的list返回，例如:
```python
>>> def f(x):
...     return x*x
...     
... 
>>> map(f,[1,2,3,4,5])
[1, 4, 9, 16, 25]
```

所以，map作为高阶函数，它把计算规则抽象化了，因此，我们不仅可以计算简单的f(x)=x*x，而且还可以计算更为复杂的函数。

##### reduce
reduce把一个函数作用在一个序列[x1,x2,x3...]上，**这个函数必须接收两个参数**，reduce将计算得到的结果再与序列中的下一个元素做累计计算，其效果就是:
>reduce(f,[x1,x2,x3,x4])=f(f(f(x1,x2),x3),x4)

例如，一个序列求和就可以通过reduce实现
```python
>>> def add(x,y):
...     return x+y
... 
>>> reduce(add,[1,2,3,4,5])
15
```

在介绍完，map和reduce的用法之后，我们就给出一个两者混合使用的例子: 将字符串形式表示的整数转换成整数。
```python
>>> def fn(x,y):
...     return x*10+y
... 
>>> def num2int(s):
...     return {'0':0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}[s]
... 
>>> reduce(fn,map(num2int,'13579'))
13579
```

##### 练习
利用map()函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入: ['adam', 'LISA', 'barT']，输出: ['Adam', 'Lisa', 'Bart']。

解答: 
```python
def name_format(s):
	s = s.lower()
	s = s.capitalize()
	return s

print map(name_format,['adam','LISA','barT'])
```

#### filter高阶函数的用法
Python内建的filter函数用于过滤序列。和map()相似，filter()也接受一个函数和一个序列共两个参数，但和map()不同的是，filter()依次把函数作用于序列中每个元素，然后根据返回值是True还是False来保留还是丢弃该元素。

例如，在一个list中删除掉其中的偶数，那么这就可以这么写:
```python
def is_odd(x):
	return x%2 == 1

print filter(is_odd,[0,1,2,3,4,5,6,7,8,9])
```
最后运行得到的结果是:
>[1, 3, 5, 7, 9]

如果is_odd()得到的结果是True，那么则保留该元素，如果is_odd()得到的函数是False，那么则删除该元素。


##### 练习
请尝试使用filter()删除1-100之间的素数。

```python
import math

def filter_prime_number(x):
	i = 2
	n = (int)(math.sqrt(x))
	while i <= n:
		if x % i == 0:
			return True
		i = i + 1
	if i > n:
		return False

print filter(filter_prime_number,range(1,101))
```

运行得到的结果是:
>[4, 6, 8, 9, 10, 12, 14, 15, 16, 18, 20, 21, 22, 24, 25, 26, 27, 28, 30, 32, 33, 34, 35, 36, 38, 39, 40, 42, 44, 45, 46, 48, 49, 50, 51, 52, 54, 55, 56, 57, 58, 60, 62, 63, 64, 65, 66, 68, 69, 70, 72, 74, 75, 76, 77, 78, 80, 81, 82, 84, 85, 86, 87, 88, 90, 91, 92, 93, 94, 95, 96, 98, 99, 100]


#### sorted排序函数
Python内置的sorted()函数能够对list进行排序，例如:
```python
>>> sorted([1,3,5,2,9,7,4,6,8])
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```
同时sorted函数还是一个高阶函数，它还能够接收一个比较函数来实现自定义排序的功能。

例如，我们想实现一个排倒序的功能，那么我们可以自定义一个排倒序的比较函数，然后将这个函数传递到sorted函数中:
```python
>>> def reversed_cmp(x,y):
...     if x < y:
...             return 1
...     if x > y:
...             return -1
...     return 0
... 
>>> sorted([1,3,5,2,9,7,4,6,8],reversed_cmp)
[9, 8, 7, 6, 5, 4, 3, 2, 1]
>>> 
```

### 3.2 返回函数 <span id="3.2"></span>
函数除了可以作为另外一个函数的参数之外，还可以作为函数的返回值。

下面就以一个求和函数为例:
```python
>>> def lazy_sum(*args):
...     def sum():
...         ax = 0
...         for n in args:
...             ax = ax + n
...         return ax
...     return sum
```
当我们调用lazy_sum()时，返回的不是最后计算得到的数值，而是一个函数对象
```python
>>> f = lazy_sum(1,3,5,7,9)
>>> f
<function sum at 0x7f55b35b96e0>
```
只有再调用函数f()之后才能得到求和的结果:
```python
>>> f()
25
```
在这个例子中，我们在lazy_sum()函数中还定义了一个sum()函数，并且内部函数sum还引用了外部函数lazy_sum函数中的参数和局部变量，当lazy_sum函数返回函数sum时 ，相关参数和局部变量都保存在返回函数值中，这种程序成为“闭包”。

##### 闭包的使用
闭包的使用虽然简单，但是它有许多需要注意的地方，下面就举个例子:
```python
>>> def count():
...     fs = []
...     for i in range(1,4):
...             def f():
...                 return i*i
...             fs.append(f)
...     return fs
... 
>>> f1,f2,f3 = count()
```
在上面例子中，每次循环都建立了一个新的函数，然后将3个函数都通过列表的形式返回了。

你可能认为，三个函数的执行结果分别是1,4,9，但是实际的运行结果则是:
```python
>>> f1()
9
>>> f2()
9
>>> f3()
9
```
三个函数的运行结果全部都是9！这是因为函数f在定义中引用了局部变量i，并且函数f并不是立即运行。等到三个函数都返回时，保存在返回函数中的局部变量i的值已经都变成了3，因此最终的结果都是9。

**所以在返回闭包时一定要注意，千万不要在返回的函数中使用循环中的变量，或者后续会发生变化的变量**

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变:
```python
>>> def count():
...     fs=[]
...     for i in range(1,4):
...         def f(j):
...             def g():
...                 return j*j
...             return g
...         fs.append(f(i))
...     return fs
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

### 3.3 匿名函数 <span id="3.3"></span>
当我们在传入函数时，有些时候我们可以不用显示地定义一个函数，而直接传入匿名函数更为方便。

以map()函数为例，计算f(x)=x*x时，除了定义一个f(x)之外，还可以直接传入匿名函数:
```python
>>> map(lambda x:x*x,[1,2,3,4,5])
[1, 4, 9, 16, 25]
```

通过对比可以知道，lambda x:x*x实际上等效于:
```python
def f(x):
	return x*x
```
从上面的例子中，我们可以总结一下匿名函数的定义形式:
```python
lambda 形参:返回值表达式
```
其中关键字**lambda**表示匿名函数，冒号前面是形参，冒号后面就是返回值表达式。但是从上面的匿名函数定义形式我们就可以看出，匿名函数有个限制，就是它的返回值只能有一个，因为冒号后面只允许有一个返回值表达式。

用匿名函数有一个好处，因为没有函数名，就不存在函数名冲突。此外，匿名函数还是一个函数对象，还可以将匿名函数对象赋值给一个变量，然后就可以通过该变量来调用该函数，例如:
```python
>>> f = lambda x:x*x
>>> f
<function <lambda> at 0x7f55b35b98c0>
>>> f(5)
25
```
同样，匿名函数也可以作为返回值返回。

### 3.4 decorator函数（装饰器） <span id="3.4"></span>

函数对象有一个__name__属性，可以拿到函数的名字:
```python
>>> def now():
...     print '2015-07-20'
... 
>>> now._name_
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'function' object has no attribute '_name_'
>>> now.__name__
'now'
>>> g = now
>>> g.__name__
'now'
```
在这里要注意一点: “\_\_name\_\_“属性中“name"前面和后面均有**两个英文下划线**。

现在，如果我们要增强now()函数的功能，比如，在函数调用前自动打印日志，但又不希望修改原来now()函数的定义，这种在代码运行期间动态增加功能的方式，称之为”装饰器“(Decorator)。

本质上，decorator就是一个返回函数的高阶函数。所以，我们要定义一个能打印日志的decorator，可定义如下:
```python
>>> def log(func):
...     def wrapper(*args,**kw):
...         print 'call %s():'%(func.__name__)
...         return func(*args,**kw)
...     return wrapper
```

观察上面的log，因它是一个decorator，所以接受一个函数作为参数，并返回一个函数。我们需要借助Python中的@语法，把decorator置于函数的定义处:
```python
@log
def now():
	print '2015-07-20'
```
把@log放在now()函数的定义处，相当于执行了语句:
```python
now = log(now)
```
下面，我们就对上述的过程进行一个分析:
![decorator_analysis_process][decorator_analysis_process_url]

如果decorator需要传入参数，那就需要编写一个返回decorator的高阶函数，例如:
```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print '%s %s():' % (text, func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator
```
运行的结果是:
```python
>>> now()
execute now():
2013-12-25
```
上面的语句的执行效果等效于:
```python
>>> now = log('execute')(now)
```
整个执行的过程，如下图所示:
![decorator_analysis_process2][decorator_analysis_process2_url]

以上两种decorator的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有__name__等属性，但你去看经过decorator装饰之后的函数，它们的__name__已经从原来的'now'变成了'wrapper':
```python
>>> now.__name__
'wrapper'
```
因为返回的那个wrapper()函数名字就是'wrapper'，所以，需要把原始函数的\_\_name\_\_等属性复制到wrapper()函数中，否则，有些依赖函数签名的代码执行就会出错。

不需要编写 wrapper.__name__ = func.__name__ 这样的代码，因为Python内置的functools.wraps就是干这事的，所以一个完整的decorator的写法如下:
```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print 'call %s():' % func.__name__
        return func(*args, **kw)
    return wrapper
```
或者是带参数的decorator
```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print '%s %s():' % (text, func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator

@log('execute')
def now():
	print '2015-07-20'

now()

print 'now.__name__: %s' % now.__name__
```
后者运行的结果为:
```python
execute now():
2015-07-20
now.__name__: now
```

**import functools**是导入functools模块。

请编写一个decorator，能在函数调用的前后打印出'begin call'和'end call'的日志。
```python
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print '%s %s():' % ('begin call', func.__name__)
        func(*args, **kw)
	print '%s %s():' % ('end call', func.__name__)
    return wrapper

@log
def now():
	print '2015-07-20'

now()
```
运行得到的结果:
>begin call now():
2015-07-20
end call now():
now.__name__: now

编写一个@log的decorator，使它既支持:
```python
@log
def f():
	pass
```
又支持:
```python
@log('execute')
def f():
	pass
```
从上面这个需求分析，我们可以采用默认参数的方法来解决上面这个问题:
```python
import functools

def log(text=None):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
			if text == None:
				print '%s():' % (func.__name__)
			else:
				print '%s %s():' % (text, func.__name__)
			return func(*args,**kw)
        return wrapper
    return decorator

@log('execute')
def now():
	print '2015-07-20-now()'

@log()
def now1():
	print '2015-07-20-now1()'

now()

now1()
```
上面运行的结果是:
>execute now():
2015-07-20-now()
now1():
2015-07-20-now1()

### 3.5 偏函数 <span id="3.5"></span>

Python中的functools模块提供了许多有用的功能，其中一个就是偏函数(Partial Function)。要注意这里的偏函数和数学上的偏函数的意义不一样。

下面就以int()函数为例，来介绍偏函数。

int()函数的功能是将字符串形式的整数转换成整型的整数形式，例如:
```python
>>> int('12345')
12345
```
但是int()还提供了额外的参数base，默认值为10。如果传入base的参数为N，那么int()就可以作N进制的转换。例如:
```python
>>> int('123',base=8)
83
>>> int('10000000',base=2)
128
```
假设要转换大量的二进制字符串，每次都传入int(x, base=2)非常麻烦，于是，我们想到，可以定义一个int2()的函数，默认把base=2传进去:
```python
>>> def int2(x):
...     return int(x,base=2)
...
>>> int2('1000')
8
```
functools.partial就是帮助我们创建一个偏函数的，不需要我们自己定义int2()，可以直接使用下面的代码创建一个新的函数int2:
```python
>>> import functools
>>> int2 = functools.partial(int,base=2)
>>> int2('1000')
8
```
简单总结一下functools.partial的作用就是将函数中的一些参数固定（将参数设置为默认值），然后返回一个新函数，通过这个函数进行调用就会变得非常简单。当函数的参数个数太多，需要简化时，使用functools.partial可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。


[python_built-in_function_url]:https://docs.python.org/2/library/functions.html#abs
[mapreduce_paper_url]:https://research.google.com/archive/mapreduce.html


[python_logo_url]:http://7xj51c.com1.z0.glb.clouddn.com/python_logo.png
[decorator_analysis_process_url]:http://7xj51c.com1.z0.glb.clouddn.com/decorator_analysis_process.png
[decorator_analysis_process2_url]:http://7xj51c.com1.z0.glb.clouddn.com/decorator_analysis_process2.png

