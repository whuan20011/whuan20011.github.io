---
layout: post
title: Leetcode|Online Majority Element In Subarray|Python
---

题目大意：本题让我们完成MajorityChecker这个类， 这个类有两个API, 第一个是根据给定arr创建一个MajorityChecker的实例， 第二个函数有三个参数， 其中
left和right表示0 <= left <= right < arr.length， 也就是说left和right可以形成一个arr的subarray， 对于第三个参数threshold， 
2 * threshold > right - left + 1, ie. 严格意义来说， threshold总是subarray的一大半（多余一半）。每个query函数返回🔙在subarray中出现至少
threshold次的那个数字。如果没有， 返回🔙-1。

Constraints:

1 <= arr.length <= 20000

1 <= arr[i] <= 20000

The number of queries is at most 10000


题目详解：题目限制如上所示， 如果按照传统方法数出subarray中各个数字出现的次数， 再去找大于等于threshold次的那个数字， 要O（n）时间， 由于subarray
长度在1到20000只见变化， 即平均长度10000， 再加上query要进行10000次， 10000*10000， 时间复杂度也就是1亿次， 显然会超时。于是我们想有没有更快的方法。
也就是将arr变化一下。如下👇__init__函数， 保留arr， 同时将arr中各数字出现的下标存放在字典中。query函数注释亦如下👇。

算法：完成一个多函数类（运用random和bisect）

```python
import random
import bisect
class MajorityChecker(object):

    def __init__(self, arr):
        """
        :type arr: List[int]
        """
        self.arr = arr
        self.dic = {}
        #将arr中各数字出现的下标存放在字典中
        for i, num in enumerate(self.arr):
            if num not in self.dic:
                self.dic[num] = []
            self.dic[num].append(i)
    def query(self, left, right, threshold):
        """
        :type left: int
        :type right: int
        :type threshold: int
        :rtype: int
        """
        #用十次随机取数， 如果subarray中有数字出现此书超过一半， 则随机抽取到该数字的概率就大一一半， 抽取10次， 取不到的概率就非常非常小
        #所以我们默认如果10次还抽取不到， 则证明这段subarray中没有出现超过一半次数的数字。
        #本处我们采取10次是大致估算的， 开始我们随机抽取30次， 程序通过， 20次， 又通过，最后我们缩小到了10次还是通过了， 而且节省了时间。
        #所以最后我们定了10次。
        for i in range(10):
            #在subarray中得到一个随机下标
            self.arr_idx = left + int(random.random() * (right - left + 1))
            #得到随机的数字
            self.random_num = self.arr[self.arr_idx]
            #把得到的随机取来的数字在dic中进行寻找， 得到该数字之前在arr中出现过的所有下标list。
            self.dic_list = self.dic[self.random_num]
            #用bisect函数把left和right插到下标list中， 就知道在subarray中该数字出现了几次。
            self.right = bisect.bisect_right(self.dic_list, right)
            self.left = bisect.bisect_left(self.dic_list, left) 
            #如果出现次数大于等于threshold， 则返回🔙该数字。
            if (self.right - self.left) >= threshold:
                return self.random_num
        return -1
        
        


# Your MajorityChecker object will be instantiated and called as such:
# obj = MajorityChecker(arr)
# param_1 = obj.query(left,right,threshold)
```
