<!-- 
#+TITLE: Python中的lambda表达式
#+URL: https://allanderek.github.io/posts/python-and-lambdas/
#+AUTHOR: lujun9972
#+TAGS: Python Common
#+DATE: [2017-03-22 三 16:44]
-->

原文链接: https://allanderek.github.io/posts/python-and-lambdas/

我拥有函数式编程的经验, 因此我非常热衷于使用函数以及lambda表达式,也就是所谓的匿名函数. 然而我发现当写Python时我并不怎么使用它的lambda语法,我很好奇怎么会这样. 这么说似乎对lambda很不利 但是请记住,总体来说我并不讨厌lambda.

所有的lambda表达式都能被普通函数所代替. 问题是,什么情况下使用lambda会比定义普通函数拥有更好的可读性? 答案是几乎总是这样. 因为你要定义的函数太简单了,定义的语法反而会阻碍对内容的理解. 这些函数一般都会作为其他方法的参数来使用. 比如可以作为 list的 `sort` 方法中的 `key` 参数的值,在比较元素时使用. 如果你有一个包含item的列表,然后想根据这些item的price来排序,那么你可以这么做:

```python
list_of_items.sort(key=lambda x: x.price)
```

当然可以通过定义函数的方式来代替lambda:

```python
def price(item):
    return item.price
list_of_items.sort(key=price)
```

严格来说,这两者是不等价的,因为后面这种方式会引入一个新名字到当前作用域中而前者不会, 不过我并不会仅仅为了避免污染命名空间就选择用lambda表达式.

我们甚至可以定义一个一般性的高阶函数,用于生成一个访问属性的函数.

```python
def f_attr(attr_name):
    def attr_getter(item):
        return getattr(item, attr_name)
    return attr_getter

list_of_items.sort(key=f_attr('price'))
```

这看起来要比前两个版本更糟糕, 但若你要根据不同属性进行多次排序,那么使用定义普通函数的方法会很烦:

```python
def price(item):
    return item.price
list_of_items.sort(key=price)
do_something(list_of_items)

def quantity(item):
    return item.quantity
list_of_items.sort(key=quantity)
do_something(list_of_items)

def margin(item):
    return item.margin
list_of_items.sort(key=margin)
do_something(list_of_items)
```

相比来说使用lambda就简单多了:

```python
list_of_items.sort(key=lambda x: x.price)
do_something(list_of_items)
list_of_items.sort(key=lambda x: x.quantity)
do_something(list_of_items)
list_of_items.sort(key=lambda x: x.margin)
do_something(list_of_items)
```

使用f\_attr也一样:

```python
list_of_items.sort(key=f_attr('price'))
do_something(list_of_items)
list_of_items.sort(key=f_attr('quantity'))
do_something(list_of_items)
list_of_items.sort(key=f_attr('margin'))
do_something(list_of_items)
```

甚至还能用循环来解决:

```python
for attribute in ['price', 'quantity', 'margin']:
    list_of_items.sort(key=f_attr(attribute))
    do_something(list_of_items)
```

我认为lambda版本至少在某些情况下会更有可读性. 但是函数天然具有自我说明的功能,因为函数名本身就有说明的作用,尤其那些能用lambda表达式代替的简单函数,他们的自描述性更强.

所以,最好用lambda表达式来代替那些内容足够简单,无需函数名就能充分理解意义的函数(比如单纯的访问对象属性).

不过这种情况对我来说很少见. 也许是我定义的函数还不够一般化以至于要接受其他函数作为参数吧.
