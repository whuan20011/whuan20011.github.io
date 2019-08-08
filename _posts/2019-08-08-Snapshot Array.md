---
layout: post
title: Leetcode|Snapshot Array|Python
---

题目大意：

题目详解：

算法：

```python
import bisect
class SnapshotArray(object):

    def __init__(self, length):
        self.snap_id = 0
        self.dic = {}
        

    def set(self, index, val):
        """
        :type index: int
        :type val: int
        :rtype: None
        """
        if index not in self.dic:
            self.dic[index] = [[self.snap_id, val]]
        elif self.snap_id == self.dic[index][-1][0]:
            self.dic[index][-1] = [self.snap_id, val]
        else:
            self.dic[index].append([self.snap_id, val])
        
        

    def snap(self):
        """
        :rtype: int
        """
        self.last_snap = self.snap_id
        self.snap_id += 1
        return self.last_snap
        

    def get(self, index, snap_id):
        """
        :type index: int
        :type snap_id: int
        :rtype: int
        """
        if index not in self.dic:
            return 0
        pos = bisect.bisect(self.dic[index], [snap_id, float('inf')])
        if pos == 0:
            return 0
        return self.dic[index][pos - 1][1]
        
            
            
        
        
        


# Your SnapshotArray object will be instantiated and called as such:
# obj = SnapshotArray(length)
# obj.set(index,val)
# param_2 = obj.snap()
# param_3 = obj.get(index,snap_id)
```
