---
layout: posts
title: Grammars|sort|sorted|reserve|reserved
---
```
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
```


 
