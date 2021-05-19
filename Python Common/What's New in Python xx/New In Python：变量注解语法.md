<!-- 
#+TITLE: New In Python:变量注解语法
#+URL: http://www.blog.pythonlibrary.org/2017/01/12/new-in-python-syntax-for-variable-annotations/
#+AUTHOR: lujun9972
#+TAGS: What's New in Python xx
#+DATE: [2017-03-13 一 23:06]
-->

原文地址:http://www.blog.pythonlibrary.org/2017/01/12/new-in-python-syntax-for-variable-annotations/

---

[Python 3.6](https://docs.python.org/3.6/whatsnew/3.6.html#whatsnew36-pep526) 开始支持变量注解了(Syntax for variable annotations). 这是一项脱胎于 [PEP 526](https://www.python.org/dev/peps/pep-0526) 的新功能. 这个PEP将Type Hinting ([PEP 484](https://www.python.org/dev/peps/pep-0484)) 的理念变成了现实. 它可以为Python变量(包括类变量和实例变量)添加额外可选的类型声明. 请注意,添加这些注解或者说声明并不会让Python变成一门静态类型语言. 解释器依然并不关心变量的类型是什么. 但是对于Python IDE或其他类似pylink这样的工具来说,他们可以使用这些注解来进行检查,并高亮出与注解声明类型不一致的变量来.

下面通过一个简单的例子来看看它是怎么工作的:

```python
# annotate.py
name: str = 'Mike'
```

这是 `annotate.py` 中的内容. 其中,我们创建了一个变量: `name`, 它后面的注解说明它是一个字符串. 为变量添加注解就是在变量名后加上一个冒号以及该变量所属的类型. 如果你不想为变量赋值也可以. 下面的句子也没问题.

```python
# annotate.py
name: str
```

当你为一个变量添加了注解,该变量就会被添加到 module 或是 class 的 `__annotations__` 属性中. 下面让我们导入这个 `annotate` module,然后看看它的属性:

```python
>>> import annotate
>>> annotate.__annotations__
{'name': <class 'str'>}
>>> annotate.name
'Mike'
```

你可以看到, `__annotations__` 属性返回了一个Python dict变量, 其中以变量名为key,以对应的注解类型为value.

下面让我们添加一些其他类型的注解,再看看 `__annotations__` 属性会变成怎样的.

```python
# annotate2.py
name: str = 'Mike'

ages: list = [12, 20, 32]

class Car:
    variable: dict
```

在这段代码中, 我们添加了一个带注解的 `list` 变量,以及一个类,这个类中又定义了一个带注解的类变量. 现在我们再导入这个 `annotate` module,然后看看 `__annotations__` 属性是怎样的:

```python
>>> import annotate
>>> annotate.__annotations__
{'name': <class 'str'>, 'ages': <class 'list'>}
>>> annotate.Car.__annotations__
{'variable': <class 'dict'>}
>>> car = annotate.Car()
>>> car.__annotations__
{'variable': <class 'dict'>}
```

这一次你会发现这个注解字典中包含了两项内容,而其中并没有那个类变量. 要看到这个类变量,你需要访问 `Car` 这个类中的 `__annotations__` 属性,或者先创建一个Car实例,然后再访问该实例的 `__annotations__` 属性.

有读者指出, 借助于 `typing` module,你可以让这里的例子更接近PEP484的要求. 比如下面这个例子:

```python
# annotate3.py
from typing import Dict, List

name: str = 'Mike'

ages: List[int] = [12, 20, 32]

class Car:

    variable: Dict
```

在解释器中运行这段代码会发现如下结果:

```python
import annotate

In [2]: annotate.__annotations__
Out[2]: {'ages': typing.List[int], 'name': str}

In [3]: annotate.Car.__annotations__
Out[3]: {'variable': typing.Dict}
```

你会发现大部分的type现在都来自于 `typing` module了.

---


<a id="orgf39fe31"></a>

# 总结
我发现这项新功能确实很有用. 我很喜欢Python的动态特性,但这几年的C++经历也让我同时能发现明确变量类型的价值所在. 当然,Python有很强大的内省能力,要探测出一个对象的类型并不难. 但是这个项新功能能够改进静态检查器,同时让你的代码更易懂. 尤其当你需要回过头来维护一个N久没有接触过的软件时更是如此.

---

扩展阅读
-   PEP 526 – [Syntax for variable annotations](https://www.python.org/dev/peps/pep-0526)
-   PEP 484 – [Type Hinting](https://www.python.org/dev/peps/pep-0484)
-   Python 3.6: [What’s New](https://docs.python.org/3.6/whatsnew/3.6.html)
