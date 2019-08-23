---
layout: post
title: Leetcode|Validate Stack Sequences|Python
---

题目大意：给两个列表pushed和popped， 里面的值没有重复的。有且仅当这两个列表的操作是从一个空栈操作后合理， 返回🔙true， 否则返回false。

题目详解：这是一道典型的栈的操作问题。详解见注释。

算法：栈

```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        """
        :type pushed: List[int]
        :type popped: List[int]
        :rtype: bool
        """
        #先处理特殊情况
        if not pushed and not popped:
            return True
        if not pushed and popped:
            return False
        if pushed and not popped:
            return True
        #先得到pop操作的第一个数在push操作中的位置i， 说明push操作中位置i及其前面的数字全部被push到了空栈中， 那我们就把这个空栈构建出来
        i = pushed.index(popped[0])
        stack = pushed[:(i + 1)]
        i += 1
        #遍历pop操作中的数字， 如果这个数字刚好是栈顶元素， 则这个pop是一次有效操作
        #如果不等于栈定元素， 说明之前push可能进行了多次压栈， 所以要把push中从位置i开始的元素压到栈里， 知道找到那个和popped中当前元素相等的位置
        #说明之前压栈到该位置， 然后又进行了该元素的pop操作。找到该位置之后， 停掉while循环， 用tag标记已经找到
        #如果找不到， 则说明当前pop元素既不等于栈顶元素， 又不没有进行压栈找到， 所以是错误操作， 返回false
        for num in popped:
            if stack and num == stack[-1]:
                stack.pop()
                continue
            else:
                tag = 0
                while i < len(pushed): 
                    if num != pushed[i]:
                        stack.append(pushed[i])
                        i += 1
                    else:
                        i += 1
                        tag = 1
                        break
                if tag == 0:
                    return False
        return True
```
