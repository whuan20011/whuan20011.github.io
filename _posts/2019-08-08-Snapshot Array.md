---
layout: post
title: Leetcode|Snapshot Array|Python
---

题目大意：给一个类， 类中有四个函数， 让我们完成这四个函数的功能。第一个函数是初始化函数， 是给一个列举的数据类型设置为length的长度。第二个set函数
是把列表中下标为index的值设置为val。第三个函数是对当前列表进行照相。第四个函数是对snap_id的那次照相， 照出来的列表取下标为index的值。

题目详解：

算法：对类中各函数功能的完成

```python
import bisect
class SnapshotArray(object):

    def __init__(self, length):
        self.snap_id = 0
        #用dic来保各下标的照相次数和值的改动情况
        self.dic = {}
        

    def set(self, index, val):
        """
        :type index: int
        :type val: int
        :rtype: None 此处表示该函数无返回值
        """
        #如果index不在dic中， 表示是第一次对此index的值进行改动， 把照相次数和值保存起来
        if index not in self.dic:
            self.dic[index] = [[self.snap_id, val]]
        #如果在里面， 并且当前照相次数和之前的相同， 则说明现在只是改值， 没有照相， 即只改值， 保留照相次数不变
        elif self.snap_id == self.dic[index][-1][0]:
            self.dic[index][-1] = [self.snap_id, val]
        #如果在里面， 且与之前保存的照相次数变了， 则说明在本次改值之前照相了， 照了几次不知道，所以把现在的照相次数和要改的值加进去
        else:
            self.dic[index].append([self.snap_id, val])
        
        

    def snap(self):
        """
        :rtype: int
        """
        #之前的self.snap_id才是现在的照相次数， 例如self.snap_id初始值设置为0， 现在第0次照相， 所以要返回之前的值， 现在的值是用于set接下来
        #该值时候用的
        self.last_snap = self.snap_id
        self.snap_id += 1
        return self.last_snap
        

    def get(self, index, snap_id):
        """
        :type index: int
        :type snap_id: int
        :rtype: int
        """
        #下标不在dic中，说明没有对该下标改动过， 所以一直是初始值0
        if index not in self.dic:
            return 0
        #本次用了bisect二分法找snap_id(这里的snap_id是外面传进来的， 不是自己的self.snap_id)
        #我们把这个snap_id和无穷数放进去找到位置， 则前一个就是照相次数时的值。
        pos = bisect.bisect(self.dic[index], [snap_id, float('inf')])
        #pos是0，说明本次之前没有改值，可能经过了一到多次照相，保持0
        if pos == 0:
            return 0
        return self.dic[index][pos - 1][1]
        
            
            
        
        
        


# Your SnapshotArray object will be instantiated and called as such:
# obj = SnapshotArray(length)
# obj.set(index,val)
# param_2 = obj.snap()
# param_3 = obj.get(index,snap_id)
```
