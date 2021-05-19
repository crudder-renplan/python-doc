<!-- 
#+TITLE: New in Python:数字字面量中的下划线
#+URL: http://www.blog.pythonlibrary.org/2017/01/11/new-in-python-underscores-in-numeric-literals/                                      
#+AUTHOR: lujun9972
#+TAGS: What's New in Python xx
#+DATE: [2017-03-10 五 16:56]
-->
原文地址: http://www.blog.pythonlibrary.org/2017/01/11/new-in-python-underscores-in-numeric-literals/                                      

Python 3.6 新增了一些有趣的功能. 本文要说的这项功能脱胎于[PEP 515: Underscores in Numeric Literals](https://www.python.org/dev/peps/pep-0515). 正如其名所言,它允许你在写一个很长的数字时用下划线(代替常用的逗号)来进行分段. 即是说, `1000000` 现在可以写成 `1_000_000` 了. 下面来看一些简单的例子:

```python
>>> 1_234_567
1234567
>>>'{:_}'.format(123456789)
'123_456_789'
>>> '{:_}'.format(1234567)
'1_234_567'
```

第一个例子展示了Python是如何解释用下划线分段过的数字的. 第二个例子说明了我们不仅可以使用逗号,还能使用“\_” (下划线) 来将数字格式化成字符串了. 其效果不言自明.

在进行数学运算时,有没有下划线分段都一样:

```python
>>> 120_000 + 30_000
150000
>>> 120_000 - 30_000
90000
```

Python文档和PEP同时也有说明,你可以用下划线对任意进制的数字进行分段. 下面摘抄一些例子:

```python
>>> flags = 0b_0011_1111_0100_1110
>>> flags
16206
>>> 0x_FF_FF_FF_FF
4294967295
>>> flags = int('0b_1111_0000', 2)
>>> flags
240
```

使用下划线对数字分段时需要注意以下几点:

-   你只能使用单下划线进行分段,而且它的位置必须在数字之间,在进制说明符之后
-   下划线不能出现在数字的首部和尾部

这真是一项有趣的新功能. 我本人目前是用不倒这个功能,希望对你用吧.
