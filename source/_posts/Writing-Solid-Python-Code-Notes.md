---
title: <Writing Solid Python Code> Notes
date: 2016-08-02 09:44:56
tags: [Python]
---

# 《编写高质量代码：改善 Python 程序的 91 个建议》 学习笔记
之前自己学了很多次 Python，由于用不到，所以总是学完就忘掉了。
刚好最近工作需要用到 Python，就借此机会好好学习了一番。
Pyhton 的各种特性和风格让我甚是喜欢。不过我总是感觉自己写的代码不是那么漂亮，不够 Pythonic。
所以我想通过学习一些经典建议来让我有个思路。
<!--more-->
## 建议 1：理解 Pythonic 概念
对于 Pythonic 的概念，大家心中的指南就是 Tim Peters 的 《The Zen of Python》（Python 之禅）。
下面几点来自其中的内容：
* 美胜丑，显胜隐，简胜杂，平胜陡，疏胜密。
* 找到简单问题的一个方法，最好是唯一的方法（正确的解决之道）。
* 难以解释的实现，源自不好的主意；如有非常棒的主意，它的实现肯定易于解释。

代码风格建议参考 PEP8 和 [Python 风格指南](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)
比如：
* 包和模块的命名采用小写、单数形式，而且短小。
* 包通常仅作为命名空间，如只包含空的 `__init__.py` 文件

## 建议 2：编写 Pythonic 代码
1. 要避免劣化代码
  * 避免只用大小写来区分不同的对象 
  * 避免使用容易引起混淆的名称
  比如，重复使用已经存在于上下文中的变量名来表示不同的类型；
  误用了内建名称来表示其他含义的名称而使之在当前命名空间被屏蔽；
  没有构建新的数据类型的情况下使用类似于 element、list、dict等作为变量名；
  使用o、l、等作为变量名。
  * 不要害怕过长的变量名，可读性更重要
2. 深入认识 Python 有助于编写 Pythonic 代码
  * 全面掌握 Python 提供给我们的所有特性，包括语言特性和库特性。
  * 学习每个 Python 新版本提供的新特性，使用 Python 推荐的惯用法来完成任务
  * 深入学习业界公认的比较 Pythonic 的代码，比如 Flask、gevent 和 requests 等。
3. 使用工具来达到检查和约束，比如我个人使用 Pycharm IDE 来写 Python，对代码风格的要求挺严格的...

## 建议 3：理解 Python 与 C 语言的不同之处
我们都知道，Python 底层是用 C 语言实现的，但切忌用 C 语言的思维和风格来编写 Python 代码。尤其重要的是，不要使用之前的编程思想。

1. “缩进”与“{}”
与 C、C++、Java 等语言使用花括号来分隔代码段不同，Python 中使用严格的代码缩进方式分隔代码块。
另外，建议 Tab 替换成 4 个空格，不要混用 Tab 键和空格。
2. ' 与 "
C 语言中单引号 `'` 与双引号 `"` 由严格的区别，单引号代表一个字符，它实际对应于编译器所采用的字符集中的一个整数值。而双引号则表示字符串，默认以 `\0` 结尾。
但是在 Python 中，单引号与双引号没有明显区别。
3. 三元操作符 "?:"
三元操作符是 if...else 的简写方法，语法形式为 C ? X: Y，而在 Python 中的等价形式为 X if C else Y
4. switch...case
Python 中没有像 C 语言那样的 switch...case 分支语句。不过在 Python 中有很多替代的解决方法：

C
```C
switch(n) {
    case 0:
        printf("You typed zero.\n");
        break;
    case 1:
        printf("You are in top.\n");
        break;
    case 2:
        printf("n is an even number.\n");
        break;
    default:
        printf("Only single-digit numbers are allowed.\n");
        break;
}
```

Python
```Python
if n == 0:
    print("You typed zero.")
elif n == 1:
    print("You are in top.")
elif n == 2:
    print("n is an even number.")
else:
    print("Only single-digit numbers are allowed.)
```

或者
```Python
def f(n):
    return {
        0: "You typed zero.",
        1: "You are in top.",
        2: "n is an even number."
    }.get(n, "Only single-digit numbers are allowed.")
```

Python 和其他语言的差异远不止这些。但总归一句话：不要被其他语言的思维和习惯困扰，掌握 Python 的这些和思维方式才是硬道理。

## 建议 4：在代码中适当添加注释
Python 中有三种形式的代码注释：块注释、行注释以及文档注释（docstring）。这三种注释的惯用法大概如下几种：
1. 使用块或行注释的时候仅仅注释那些复杂的操作、算法，还有可能别人难以理解的技巧或者不够一目了然地代码。
2. 注释和代码隔开一定的距离，同时在块注释之后最好多留几行空白再写代码。
3. 给外部可访问的函数和方法添加文档注释，无论简单与否。
   注释要清楚地描述方法的功能，并对参数、返回值以及可能发生的异常进行说明，使得外部调用它的人员仅仅看docstring就能正确使用。
   较为复杂的内部方法也需要进行注释。
4. 推荐在文件头中包含 copyright 申明、模块描述等，如有比较可以考虑加入作者信息及变更记录。

## 建议 5：通过适当添加空行使代码布局更为优雅、合理
Python 代码布局也有一些基本规则可以遵循（PEP8 中有详细规范..）：
1. 在一组代码表达完一个完整的思路之后，应该用空白行进行间隔。如每个函数之间，导入声明、变量赋值等。
2. 尽量保持上下文语义的易理解性
3. 避免过长的代码行，每行最好不要超过 80 字符。
4. 不要为了保持水平对其而使用多余的空格 （写C/C++就有这习惯...）
5. 空格的使用要能够在需要强调的时候竟是读者，在疏松关系的实体间起到分隔作用。
   * 二元运算符、布尔运算的左右两边应该有空格
   * 逗号和分号前不要使用空格
   * 函数名和左括号之间、序列索引操作时序列名和 [] 之间不需要空格，函数的默认参数两侧不需要空格。
   * 强调前面的操作符的时候使用空格

## 建议 6：编写函数的 4 个原则
** 原则 1 ** 函数设计要尽量短小，嵌套层次不宜过深。
** 原则 2 ** 函数申明应该做到合理、简单、易于使用。
** 原则 3 ** 函数参数设计应该考虑向下兼容。
** 原则 4 ** 一个函数只做一件事儿，尽量保证函数语句粒度的一致性。

Python 中函数设计的好习惯还包括：不要再函数中定义可变对象作为默认值，使用异常替换返回错误，保证通过单元测试等。

## 建议 7：将常量集中到一个文件
Python 中使用常量一般有以下两种方式：
* 通过命名风格来提醒使用者该变量代表的意义为常量，如常量名所有字母大写，用下划线连接各个单词，如 MAX_OVERFLOW，这只是一种约定俗成的风格。
* 通过自定义的类实现常量功能。这要求符合“命名全部为大写”和“值一旦绑定便不可再修改”这两个条件。
```Python
class _const:
    class ConstError(TypeError): pass
    class ConstCaseError(ConstError): pass

    def __setattr__(self, name, value):
        if self.__dict__.has_key(name):
            raise self.ConstError, "Can't change const.{}".format(name)
        if not name.isupper():
            raise self.ConstCaseError, 'const name "{}" is not all uppercase'.format(name)
        self.__dict__[name] = value

import sys
sys.modules[__name__] = _const()
```
如果上面的代码对应的模块名为 const，使用的时候只需要 import const，便可直接定义常量了，如下代码：
```Python
import const
const.COMPANY = "IBM"
```

## 建议 8：利用 assert 语句来发现问题
断言（assert）在很多语言中都存在，它主要为调试程序服务，能够快速方便的检查程序的异常或者发现不恰当的输入等，可防止意想不到的情况出现。

对 Python 中使用断言需要说明如下：
1. __debug__ 的值默认设置为 True，而且是只读的。
2. 断言是有性能影响的。Python 可以在运行脚本时通过 `-O` 标识来禁用断言。比如 `Python -O test.py`

断言实际是被设计用来捕获用户所定义的约束的，而不是用来捕获程序本身错误的，因此食用断言需要注意以下几点：
1. 不要滥用，这是使用断言最基本的原则。
2. 如果 Python 本身的异常能够处理就不要再使用断言。断言没有明确的异常类型。
3. 不要使用断言来检查用户的输入。
4. 在函数调用后，当需要确认返回值是否合理时可以使用断言。
5. 当条件是业务逻辑继续下去的先决条件时可以使用断言。

## 建议 9：数据交换值得时候不推荐使用中间变量

## 建议 10：充分利用 Lazy evaluation 的特性
Lazy evaluation 常被译为“延迟计算”或“惰性计算”，值得是仅仅在真正需要执行的时候才会计算表达式的值。
充分利用 Lazy evaluation 的特性带来的好处有两个方面：
1. 避免不必要的计算，带来性能上的提升。
2. 节省空间，使得无限循环的数据结构成为可能。

## 建议 11：理解枚举替代实现的缺陷
在 Python 3.4 之前，并没有提供枚举类型。所以人们充分利用 Python 的dong'tai动态性这个特征，行除了美剧的各种替代实现：

1. 使用类属性
```Python
class Seasons:
    Spring, Summer, Autumn, Winter = range(4)
```

2. 借助函数
```Python
def enum(*posarg, **keysarg):
    return type("Enum", (object,), dict(zip(posarg, xrange(len(posarg))), **keysarg))
```

3. 使用 collections.nametuple
```Pyhton
Seasons = namedtuple('Seasons', 'Spring Summer Autumn Winter')._make(range(4))
```

但是这些替代有其不合理的地方：
* 允许枚举值重复
* 支持无意义的操作，比如相加

在3.4之后，加入了枚举 Enum，其实现主要参考 flufl.enum，但两者之间存在一些差别。

## 建议 12：不推荐使用 type 来进行类型检查
基于内建类型扩展的用户自定义类型，type函数并不能准确返回结果。
任意泪的实例的 `type()` 返回结果都是 `<type 'instance'>`。
我们可以使用 isinstance() 函数来检测类型。

## 建议 13：尽量转换为浮点类型后再做除法

## 建议 14：警惕 eval() 的安全漏洞
Python 中 eval() 函数将字符串 str 当初有效的表达式来求值并返回计算结果。其函数声明如下：
`eval(expression[, globals[, locals]])`
其中参数 globals 为字典形式，locals 为任何映射对象，他们分别表示全局和局部命名空间。
如果传入 globals 参数的字典中缺少 `__builtins__` 的时候，当前的全局命名空间将作为 globals 参数输入并且在表达式计算之前被解析。
locals 参数默认与 globals 相同，如果两者都省略的话，表达式将在 eval() 调用的环境中执行。

如果使用对象不是信任源，应该尽量避免使用 eval，在需要使用 eval 的地方可用安全性更好的 ast.literal_eval 替代。

## 建议 15：使用 enumerate() 获取序列迭代的索引和值

## 建议 16：分清 == 与 is 的适用场景

|操作符|意义           |
|:----|:--------------|
|is   |object identity|
|==   |equal          |

is 表示的是对象标识符 (object identity)，而 == 表示的意思是相等。
is 的作用是用来检查对象的标识符是否一致的，也就是比较两个对象在内存中是否拥有同一块内存空间。
== 才是用来检验两个对象的值是否相等的，它实际调用内部 `__eq__()` 方法。

## 建议 17：考虑兼容性，尽可能使用 Unicode

## 建议 18：构建合理的包层次来管理 module
什么是包？简单说包即是目录，但是与目录不同，它除了包含常规的 Python 文件以外，还包含一个 `__init__.py` 文件，同时它允许嵌套。
包有以下几种导入方法：
1. 直接导入一个包
`import Package`
2. 导入子模块或子包，包嵌套的情况下可以进行嵌套导入
```Python
from Package import Module1
import Package.Module1
```

包的使用能够带来以下便利：
* 合理组织代码，便于维护和使用
* 能够有效的避免命名空间冲突

## 建议 19：有节制的使用 from...import 语句
在使用 import 的时候注意以下几点：
* 一般情况下尽量优先使用 import a 形式
* 有节制地使用 from a import B 形式，可以直接访问 B
* 尽量避免使用 from a import *，因为这会污染命名空间，并且无法清晰的表示导入了哪些对象

当加载一个模块的时候，解释器实际上要完成以下动作：
1. 在 sys.modules 中进行搜索看看模块是否已经存在，如果存在，则将其导入到当前局部命名空间，加载结束。
2. 如果在 sys.modules 中找不到对应的模块名称，则为需要导入的模块创建一个字典对象，并将该对象信息插入 sys.modules 中。
3. 加载钱确认是否需要对模块对应的文件进行编译，如果需要则先进行编译。
4. 执行动态加载，在当前模块的命名空间中执行编译后的字节码，并将其中所有的对象放入模块对应的字典中。

对于 from...import 无节制的使用会带来什么问题：
1. 命名空间的冲突
2. 循环嵌套导入的问题

## 建议 20：优先使用 absolute import 来导入模块

## 建议 21：i+=1 不等于 ++i
Python 中是不支持概念中 ++i 操作的。
但是如果你这么写，会被 Python 解释成 +(+i)，其中 + 表示正号

## 建议 22：使用 with 自动关闭资源
with 语句可以在代码块执行完毕后还原进入该代码块时的现场。包含有 with 语句的代码块的执行过程如下：
1. 计算表达式的值，返回一个上下文管理器对象。
2. 加载上下文管理器对象的 `__exit__()` 方法以备后用
3. 调用上下文管理器对象的 `__enter__()` 方法
4. 如果 with 语句中设置了目标对象，则将 `__enter__()` 方法的返回值赋值给目标对象
5. 执行 with 中的代码块
6. 如果步骤5中代码正常结束，调用上下文管理器的 `__exit__()` 方法，其返回值直接忽略。
7. 如果步骤5中代码执行过程中发生异常，调用上下文管理器的 `__exit__()` 方法，并将异常类型、值及 traceback 信息作为参数传递给 `__exit__()` 方法。
如果 `__exit__()` 返回值为 False，则异常会重新抛出；如果其返回值为 True，异常被挂起，程序继续执行。

## 建议 23：使用 else 子句简化循环（异常处理）

## 建议 24：遵循异常处理的几点基本原则
1. 注意异常的粒度，不推荐在 try 中放入过多的代码。
2. 谨慎使用单独的 except 语句处理所有异常，最好能定位具体的异常。
3. 注意异常捕获的顺序，在合适的层次处理异常。推荐的方法是将继承结构中子类异常在前面的 except 语句中抛出，而父类异常在后面的 except 语句中抛出。
4. 使用更为友好的异常信息，遵循异常参数的规范。

## 建议 25：避免 finally 中可能发生的陷阱
在实际应用程序开发过程中，并不推荐在 finally 中使用 return 语句或 break 进行返回，这种处理方式不仅会带来误解而且可能会引起非常严重的错误。

## 建议 26：深入理解 None，正确判断对象是否为空
Python 中以下数据会当作空来处理：
* 常量 None
* 常量 Flase
* 任何形式的数值类型零，如0、0L、0.0、0j
* 空的序列，如 ''、()、[]
* 空的字典，如 {}
* 当用户定义的类中定义了 nonzero() 方法和 len() 方法，并且该方法返回整数0或者布尔值 False 的时候。

其中常量 None 的特殊性体现在它既不是0、False，也不是空字符串，他就是一个空值对象。其数据类型为 NoneType，遵循单例模式，是唯一的，因而不能创建 None 对象。
所有赋值为 None 的变量都相等，并且 None 与任何其他非 None 的对象比较结果都为 False

**错误的比较**
```Python
test_list = []
if test_list is not None:
    print('list is:', test_list)
else:
    print('list is empty') 
```

**正确的比较**
```Python
test_list = []
if test_list:
    print('list is:', test_list)
else:
    print('list is empty') 
```

## 建议 27：连接字符串应优先使用 join 而不是 +
jion 的效率要高于 + 操作符
jion 的时间复杂度为O(n), + 的时间复杂度为 O(n^2)

## 建议 28：格式化字符串时尽量使用 .format 方式而不是 %
% 操作符格式化字符串时有如下几种用法：
1. 直接格式化字符或者数值
```Python
print('your score is %06.1f' % 9.5)
```
2. 以元组的形式格式化
```Python
import math
print('the %s of a circle with radius %f is %0.3f' %('circumference', 3, math.pi*radius*2))
```
3. 以字典的形式格式化
```Python
itemdict = {'itemname': 'circumference', 'radius': 3, 'value': math.pi*radius*2}
print('the %(itemname)s of a circle with radius %(radius)f is %(value)0.3f' % itemdict)
```

.format 方式格式化字符串的基本语法为：`[[填充符] 对齐方式][符号][#][0][宽度][,][.精确度][转换类型]`
其中填充符可以是除了 `{` 和 `}` 符号之外的任意符号。

|对其方式|解释                                                        |
|:------|:-----------------------------------------------------------|
|<      |表示左对其，是大多数对象为默认的对其方式                       |
|>      |表示右对其，数值默认的对其方式                                |
|=      |仅对数值类型有效，如果有符号的话，在符号后数值前进行填充，如-0029|
|^      |居中对其，用空格进行填充                                      |

|符号   |解释                                                        |
|:------|:----------------------------------------------------------|
|+      |正数前加 +，负数前加 -                                       |
|-      |正数前不加符号，负数前加 -，为数值的默认形式                   |
|空格   |正数前加空格，负数前加 -                                      |

.format 常用用法：
1. 使用位置符号
2. 使用名称
3. 通过属性
```Python
class Customer(object):
    def __init__(self, name, gender, phone):
        self.name = name
        self.gender = gender
        self.phone = phone
    def __str__(self):
        # 通过 str() 函数返回格式化的结果
        return 'Customer({self.name},{self.gender},{self.phone})'.format(self=self)

str(Customer('Lisa', 'Female', '67889'))

'Customer(Lisa, Female, 67889)'
```
4. 格式化元组的具体项
```Python
point = (1,3)
'X:{0[0]};Y:{0[1]}'.format(point)

'X:1;Y:3'
```

使用 .format 的理由：
1. format 方式在使用上较 % 操作符更为灵活
2. format 方式可以方便的作为参数传递
3. % 最终会被 .format 方式所替代
4. % 方法在某些情况下使用时需要特别小心

## 建议 29：区别对待可变对象和不可变对象
数字、字符串、元组属于不可变对象
字典、列表、字节数组属于可变对象

看一个经典例子：
```Python
class Student(object):

    def __init__(self, name, coures=[]):
        self.name = name
        self.course = coures

    def add_course(self, course_name):
        self.course.append(course_name)

    def print_course(self):
        for item in self.course:
            print(item)


def main():
    stu_a = Student('Wang yi')
    stu_a.add_course('English')
    stu_a.add_course('Math')
    print(stu_a.name + "'s course:")
    stu_a.print_course()

    print('----------------------------')

    stu_b = Student('Li san')
    stu_b.add_course('Chinese')
    stu_b.add_course('Physics')
    print(stu_b.name + "'s course:")
    stu_b.print_course()


if __name__ == '__main__':
    main()
```
结果
```Python
C:\Users\MeeSong\AppData\Local\Programs\Python\Python35\python.exe C:/Users/MeeSong/Desktop/test/test.py
Wang yi's course:
English
Math
----------------------------
Li san's course:
English
Math
Chinese
Physics

Process finished with exit code 0
```

看到没，结果与预想的并不一样。
我们通过 `id(stu_a.course)` 和 `id(stu_b.course)` (id 是查看对象的内存标识的，即内存地址) 发现两个结果是一样的，说明两个list对象指的是同一块地址。
但 `stu_a` 和 `stu_b` 本身却是两个不同的对象。在实例化两个对象的时候，这两个对象被分配了不同的内存空间，并且调用 `init()` 函数进行了初始化。 
但由于 `init()` 函数的第二个参数是个默认参数，默认桉树在函数被调用的时候仅仅被评估一次，以后都会使用第一次评估的结果，因此实际上对象空间里面 course 所指向的是同一个list地址。

我们改成这样就好了
```Python
class Student(object):

    def __init__(self, name, coures=None):
        self.name = name
        self.course = coures if coures else []

    def add_course(self, course_name):
        self.course.append(course_name)

    def print_course(self):
        for item in self.course:
            print(item)
```

另外，切片操作相当于浅拷贝。

```Python
>>> b
['a', 'b', 'c', 'd']
>>> a
['a', 'b', 'c', 'd']

>>> id(a[0])
2279905391312
>>> id(b[0])
2279905391312
>>> id(a)
2279910237896
>>> id(b)
2279910245384
```

对于不可变对象，当我们对其进行相关操作的时候，Python 实际上仍然保持原来的值，并重新创建一个新的对象。
比如字符串操作

```Python
>>> s1 = '123'
>>> s2 = s1
>>> id(s2)
2279910241368
>>> id(s1)
2279910241368
>>> id(s1[:1])
2279905395408
```

## 建议 30：[]、() 和 {}：一致的容器初始化形式
建议使用列表解析来初始化，即列表推导式（或元组和字典）
列表推导式的语法为：`[expr for iter_item in iterable if cond_expr]`
元组推导式的语法为：`(expr for iter_item in iterable if cond_expr)`
集合推导式的语法为：`{expr for iter_item in iterable if cond_expr}`
字典推导式的语法为：`{exprk:exprv for iter_item in iterable if cond_expr}`

```Python
>>> [v**2 if v%2 == 0 else v+1 for v in [2,3,4,-1] if v>0]
[4, 4, 16]
```

列表推导式非常灵活：
1. 支持多重嵌套
2. 支持多重迭代
3. 列表推导式的语法中的表达式可以是简单表达式，也可以是复杂表达式，甚至是函数
4. 列表推导式语法中的iterable可以是任意可迭代对象

为什么推荐俺在需要生成列表的时候使用列表推导式呢？
1. 使用列表推导式更为直观清晰，代码更为简洁
2. 列表推导式的效率更高

## 建议 31：记住函数传参既不是传值也不是传引用
先看两张图

![](python-31.1.png)
![](python-31.1.png)

对于在Python函数参数是传值还是传引用这个问题：
正确叫法应该是传对象或者说传对象的引用。函数参数在传递的过程中将整个对象传入，
对可变对象的修改在函数外部以及内部都可见，调用者和被调用者之间共享这个对象，
而对于不可变对象， 由于并不能真正被修改，因此修改往往是通过生成一个新对象然后赋值来实现的

## 建议 32： 警惕默认参数潜在的问题
这个问题同 [建议 29：区别对待可变对象和不可变对象](#建议-29：区别对待可变对象和不可变对象) 的例子

## 建议 33：慎用变长参数
Python 支持可变长度的参数列表，可以通过在函数定义的时候使用 *args 和 **kwargs 这两个特殊语法来实现。
1. 使用 *args 来实现可变参数列表： *args 用于接收一个包装为元组形式的参数列表来传递非关键字参数，参数个数可以任意。
2. 使用 **kwargs 接受字典形式的关键字参数列表，其中字典的键值对分别表示不可变参数的参数名和值

为什么要慎用可变长度参数呢：
1. 使用过于灵活
2. 如果一个函数的参数列表很长，虽然可以通过使用 *args 和 **kwargs 来简化函数的定义，但通常这意味着这个函数可以有更好的实现方式，应该被重构。
3. 可变长参数适合在下列情况下使用：
* 为函数添加一个装饰器
* 如果参数的数目不确定，可以考虑使用变长参数
* 用来实现函数的多态或者在继承情况下子类需要调用父类的某些方法的时候

## 建议 34：深入理解 str() 和 repr() 的区别
函数 str() 和 repr() 都可以将 Python 中的对象转换为字符串，他们的使用及输出都非常相似

![](python-34.1.png)
![](python-34.2.png)

区别主要有以下几点：
1. 两者之间的目标不同：str() 主要面向用户，其目的是可读性，返回形式为用户友好性和可读性都较强的字符串类型;
而 repr() 面向的是Python解释器，或者说开发人员，其目的是准确性，返回值表示 Python 解释器内部的含义，常作为编程人员 debug 用途
2. 在解释器中输入a时，默认调用 `repr()` 函数，而 `print(a)` 则调用 str() 函数
3. repr() 的返回值一般可以用 eval() 函数来还原对象，通常来说有这个等式：`obj == eval(repr(obj))`
4. 这两个方法分别调用内建的 `__str__()` 和 `__repr__()` 方法，一般来说在类中都应该定义 `__repr__()` 方法，而 `__str__()` 方法则为可选，
当可读性比准确性更重要的时候应该考虑定义 `__str__()` 方法。如果类中没有定义 `__str__()` 方法，则默认会使用 `__repr__()` 方法的结果来返回对象的字符串形式。
用户实现 `__repr__()` 方法的时候最好保证其返回值可以用 eval() 方法使对象重新还原

## 建议 35：分清 staticmenthod 和 classmethod 的适用场景
静态方法没有常规方法的特殊行为，如绑定、非绑定、隐式参数等规则
类方法的调用使用类本身作为其隐含参数，但调用本身并不需要显示提供该参数

类方法能够根据不同的类型返回对应的类的实例
既不跟特定的实例相关，也不跟特定的类相关的时候，用静态方法更合适

## 建议 36：掌握字符串的基本用法
小技巧
Python 遇到未闭合的小括号时会自动将多行代码拼接为一行和把相邻的两个字符串字面量拼接到一起。
```Python
>>> s = ('SELECT * '
...      'FROM atable '
...      'WHERE afield="value"')
>>> print(s)
SELECT * FROM atable WHERE afield="value"
```

性质判定

方法          |描述
:------------|:-----
isalnum()    |是否只是数字或字母
isalpha()    |是否字母
isdigit()    |是否数字
islower()    |是否小写
isupper()    |是否大写
isspace()    |是否空白符
istitle()    |是否标题化的，即每个单词首字母是否大写
startswith(prefix[,start[,end]])|是否以prefix开头，可范围内检查，prefix可接受tuple类型的实参
endswith(suffix[,start[,end]])  |是否以suffix结尾，可范围内检查，suffix可接收tuple类型的实参

查找

方法                      |描述
:------------------------|:----------
count(sub[,start[,end]]) |查找sub在字符串中出现的次数，这个数值在调用replace方法时用得着
find(sub[,start[,end]])  |查找sub在字符串中的位置，找不到时返回-1
index(sub[,start[,end]]) |同find，不过找不到会抛出 ValueError 异常，另外对于是否包含字串，更推荐使用 in 和 not in 操作符
rfind(sub[,start[,end]]) |同find，从右侧开始
rindex(sub[,start[,end]])|同index，从右侧开始

替换

方法                             |描述
:-------------------------------|:------
replace(old, new[,count])       |把字符串中的old替换为new，count为最多替换次数
translate(table[,deletechars])  |根据table转换字符串的字符，可以由string.maketrans(frm,to)生成；deletechars为过滤掉的字符

分切

方法                     |描述
:-----------------------|:--------------
partition(sep)          |它接受一个字符串参数，并返回一个3个元素的 tuple 对象。如果sep没出现在母串中，返回值是 (sep, ‘’, ‘’)；否则，返回值的第一个元素是 sep 左端的部分，第二个元素是 sep 自身，第三个元素是 sep 右端的部分。
rpartition(sep)         |
splitlines([keepends])  |
split([sep [,maxsplit]])|参数 maxsplit 是分切的次数，即最大的分切次数，所以返回值最多有 maxsplit+1 个元素。
rsplit([sep[,maxsplit]])|

不过有一个需要注意的地方
对于字符串s、s.split() 和 s.split(' ') 返回值是不同的
```Python
>>> ' hello  world!'.split()
['hello', 'world!']
>>> ' hello  world!'.split(' ')
['', 'hello', '', 'world!']
```
产生差异的原因在于当忽略 sep 参数或sep参数为 None 时与明确给 sep 赋予字符串值时 split() 采用两种不同的算法。
对于前者，split() 先去除字符串两端的空白符，然后以任意长度的空白符串作为界定符分切字符串（即连续的空白符串被当作单一的空白符看待）；对于后者则认为两个连续的 sep 之间存在一个空字符串。

连接
join() 函数的高效率（相对于循环相加而言），使它成为最值得关注的字符串方法之一。
它的功用是将可迭代的字符串序列连接成一条长字符串，如：

```Python
>>> conf = {'host':'127.0.0.1',
...     'db':'spam',
...     'user':'sa',
...     'passwd':'eggs'}
>>> ';'.join("%s=%s"%(k, v) for k, v in conf.iteritems())
'passswd=eggs;db=spam;user=sa;host=127.0.0.1'
```

变形

方法                     |描述
:-----------------------|:--------------
lower()                 |转小写
upper()                 |转大写
capitalize()            |把字符串的第一个字符大写
swapcase()              |翻转 string 中的大小写
title()                 |返回"标题化"的 string,就是说所有单词都是以大写开始，其余字母均为小写

title()函数是比较特别的，它的功能是将每一个单词的首字母大写，并将单词中的非首字母转换为小写（英文文章的标题通常是这种格式）。

```Python
>>> 'hello world!'.title()
'Hello World!'
```

因为title() 函数并不去除字符串两端的空白符也不会把连续的空白符替换为一个空格，所以建议使用string 模块中的capwords(s)函数，它能够去除两端的空白符，再将连续的空白符用一个空格代替。

```Python
>>> ' hello   world!'.title()
' Hello   World!'
>>> string.capwords(' hello   world!')
'Hello World!'
```

删减

方法                     |描述
:-----------------------|:--------------
strip([chars])          |在 string 上执行 lstrip()和 rstrip()
lstrip([chars])         |截掉 string 左边的空格
rstrip([chars])         |删除 string 字符串末尾的空格.

*strip()函数族用以去除字符串两端的空白符，空白符由string.whitespace常量定义。

填充

方法                     |描述
:----------------------- |:--------------
center(width[, fillchar])|返回一个原字符串居中,并使用空格填充至长度 width 的新字符串， fillchar 参数指定了用以填充的字符，默认为空格
ljust(width[, fillchar]) |返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串
rjust(width[, fillchar]) |返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串
zfill(width)             |返回长度为 width 的字符串，原字符串 string 右对齐，前面填充0
expandtabs([tabsize])    |把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8

编码

方法                          |描述
:----------------------------|:--------------
encode([encoding[,errors]])  |以 encoding 指定的编码格式编码 string，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace'
decode([encoding[,errors]])  |以 encoding 指定的编码格式解码 string，如果出错默认报一个 ValueError 的 异 常 ， 除 非 errors 指 定 的 是 'ignore' 或 者'replace'

## 建议 37：按需选择 sort() 或者 sorted()
`sorted(iterable[, cmp[, key[, reverse]]])`
`s.sort([cmp[, key[, reverse]]])`

* cmp 为用户定义的任何比较函数，函数的参数为两个可比较的元素（来自iterable或者list），函数根据第一个参数与第二个参数的关系依次返回 -1、0、+1（第一个参数小于第二个参数则返回负数）。该参数默认值为None。
* key 是带一个参数的函数，用来为每个元素提取比较值，默认为None（即直接比较每个元素）
* reverse 表示结果是否反转

sort() 与 sorted() 之间的比较：
1. 相比于 sort()，sorted() 使用范围更为广泛
2. 当排序对象为列表的时候两者适合的场景不同。sorted() 函数会返回一个排序后的立标，原有列表保持不变；而 sort() 函数会直接修改原有列表，函数返回为 None
3. 无论是 sort() 还是 sorted() 函数，传入参数 key 比传入参数 cmp 效率要高。
4. sorted() 函数功能非常强大，使用它可以方便的针对不同的数据结构进行排序，从而满足不同需求。对于 itemgetter 的使用，参见 [python operator.itemgetter函数与sorted的妙用](http://beginman.cn/python/2015/05/18/python-operator-sorted/)
* 对字典进行排序
```Python
>>> phonebook = {'Linda':'7750', 'Bob':'9345', 'Carol':'5834'}
>>> from operator import itemgetter
>>> sorted_pb = sorted(phonebook.items(), key=itemgetter(1))
>>> sorted_pb
[('Carol', '5834'), ('Linda', '7750'), ('Bob', '9345')]
```
* 多维 list 排序
```Python
>>> from operator import itemgetter
>>> gameresult = [['Bob', 95.00, 'A'], ['Alan', 86.0, 'C'], ['Mandy', 82.5, 'A'], ['Rob', 86, 'E']]
>>> sorted(gameresult, key=itemgetter(2,1))
[['Mandy', 82.5, 'A'], ['Bob', 95.0, 'A'], ['Alan', 86.0, 'C'], ['Rob', 86, 'E']]
```
* 字典中混合 list 排序
```Python
mydict = {'Li': ['M', 7],
          'Zhang': ['E', 2],
          'Wang': ['p', 3],
          'Du': ['c', 2],
          'Ma': ['c', 9],
          'Zhe': ['H', 7]}

print(sorted(mydict.items(), key=lambda item: itemgetter(1)(itemgetter(1)(item))))

[('Zhang', ['E', 2]), ('Du', ['c', 2]), ('Wang', ['p', 3]), ('Li', ['M', 7]), ('Zhe', ['H', 7]), ('Ma', ['c', 9])]
```
* list 中混合字典排序
```Python
gameresult = [{'name': 'Bob', 'wins': 10, 'losses': 3, 'rating': 75.00},
              {'name': 'David', 'wins':3, 'losses': 5, 'rating': 57.00},
              {'name': 'Carol', 'wins':4, 'losses': 5, 'rating': 57.00}]

print(sorted(gameresult, key=itemgetter('rating', 'name')))

[{'wins': 4, 'name': 'Carol', 'losses': 5, 'rating': 57.0}, {'wins': 3, 'name': 'David', 'losses': 5, 'rating': 57.0}, {'wins': 10, 'name': 'Bob', 'losses': 3, 'rating': 75.0}]
```

## 建议 38： 使用 copy 模块进行深拷贝对象
概念：
* 浅拷贝（shallow copy）：构造一个新的复合对象并将从原对象中发现的引用插入该对象中。浅拷贝的实现方式与多种，如工厂函数、切片操作、copy模块中的copy操作。
* 深拷贝（deep copy）：也是构造一个新的复合对象，但是遇到引用会继续递归拷贝其所指向的具体内容，也就是说它会针对引用所指向的对象继续执行拷贝，因此产生的对象不受其他引用对象操作的影响。

## 建议 39：使用 Counter 进行计数统计
Counter 类是自 Python2.7 起增加的，属于字典的子类，是一个容器对象，主要用来统计散列对象。

## 建议 40：深入掌握 ConfigParser

## 建议 41：使用 argparse 处理命令行参数
另外，还有更先进好用的 docopt，不过暂时还没加入标准库。详见 [docopt](http://docopt.org/)

## 建议 42：使用 pandas 处理大型 CSV 文件

...