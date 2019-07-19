---
layout: posts
title: Grammars|sort|sorted|reserve|reserved|lambda|mutable|immutable
---

```python
1   sorted(iterable, cmp, key, reverse)
    #此函数保留原列表， 不对其进行修改， 而是返回一个新的list。
    #iterable指可迭代对象
    #cmp是比较函数，这个函数有两个参数，参数的值都是从可迭代对象中取出，此函数必须遵守的规则为，大于则返回1，小于则返回-1，等于则返回0。
    #key是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
    #reverse是排序规则，reverse = True 降序 ， reverse = False 升序（默认）。
    >>>a = [5,7,6,3,4,1,2]
    >>> b = sorted(a)       # 保留原列表
    >>> a 
    [5, 7, 6, 3, 4, 1, 2]
    >>> b
    [1, 2, 3, 4, 5, 6, 7]

    >>> L=[('b',2),('a',1),('c',3),('d',4)]
    >>> sorted(L, cmp=lambda x,y:cmp(x[1],y[1]))   # 利用cmp函数
    [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
    >>> sorted(L, key=lambda x:x[1])               # 利用key
    [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
2   list.sort(cmp, key, reverse)
    #该函数没有返回值， 但会对原列表进行排序
3   reversed(seq)
    seq can be tuple, string, list or range
    #该函数有返回值， 不对原seq进行修改。
4   list.reverse()
    #该函数无返回值， 会对原列表进行反向排序。
5   lambda x: x + 1
6   #常见的immutable对象：int， float， string， frozen set
    #常见的mutable对对象：list, dict, set
    Case One
    >>> a = [1, 2, 3]
    >>> id(a)
    4317018016
    >>> a[1] = 10
    >>> id(a)
    4317018016
    >>> a
    [1, 10, 3]
    #不难理解，列表的值在 a[1] = 10 执行后被改变，但是其地址空间没有改变。

    Case Two
    >>> a = 1
    >>> id(a)
    140207626202088
    >>> a = 2
    >>> id(a)
    140207626202064
    #和 C 语言不同，Python 的运算符 = 是引用，而非赋值运算，所以 a = 2 的含义是创建一个值为 2 的 int 对象，再将 a 指向该对象的地址，所以上例的 1 和 2 这两个 int 对象的值并没有改变。

    Case Three
    >>> a = (1, 2, [3])
    >>> id(a)
    4317137728
    >>> a[0] = 1
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>>
    >>> a[2][0] = 100
    >>> id(a)
    4317137728
    >>> a
    (1, 2, [100])
    #从上例可看出，immutable 不能理解为绝对的不可变，对于某些 container 类型的 object，如果它包含了一个 mutable 的 object，那么 mutable         #object 的值是可变的。因为 tuple 所含 list 改变的只是 value 而非 identity，所以 tuple 依旧被称为 immutable object。
```


 
