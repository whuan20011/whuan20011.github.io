---
layout: post
title: Leetcode|Number of Valid Words for Each Puzzle|Python
---

题目大意：给两个列表words和puzzles， 其中都有很多字符串， 都是小写字母，对于puzzles中的某一个puzzle来说，第一点，如果words中的某一个word中含有puzzle的
首字母， 第二点，word中所有字母都存在于puzzle中， words中所有满足这两点的word个数就存放在返回列表中， 所以返回列表中每一个数字对应的是该下标对应的puzzle
在words中找到的符合这两点的数目。

题目限制条件：

1 <= words.length <= 10^5

4 <= words[i].length <= 50

1 <= puzzles.length <= 10^4

puzzles[i].length == 7

words[i][j], puzzles[i][j] are English lowercase letters.

Each puzzles[i] doesn't contain repeated characters.

题目详解：因为题目给了我们上面的限制条件， 如果对于每一个puzzle， 我们都去过一遍words数个数， 那我们将会是n^2的算法， 时间复杂度就是10^4 * 10^5。
这显然会超时的。但是我们发现本题特殊的是每一个puzzle的长度是7， 而且均为不同字母， 想到word中字母都存在于该puzzle中， 说明word是该puzzle的子集， 
所以该puzzle应该有2^7 = 128个子集， 但是第一条题目要求告诉我们word中必须含有puzzle的首字母, 所以每个puzzle有符合条件的2^6 = 64个子集， 也就是说，
对于任何一个puzzle， 它本来有64个满足题目要求的子集情况， 所以我们只要把words用字典的形式把每一个字符串去重排序， 算出个数， 存放在字典里， 我们就
可以对其进行查找， 看该puzzle在words中有多少个子集。这样， 我们就把算法复杂度降到了10^4 * 64，比之前小了很多。

算法：dfs（深度优先算法）

```python
class Solution(object):
    def findNumOfValidWords(self, words, puzzles):
        """
        :type words: List[str]
        :type puzzles: List[str]
        :rtype: List[int]
        """
        #把words中放到字典里
        words_dic = {}
        for word in words:
            w = tuple((sorted(list(set(word)))))
            if w not in words_dic:
                words_dic[w] = 0
            words_dic[w] += 1
        #把每一个puzzle的64个子集存到puzzles_set中， 形成一个大的二维列表
        puzzles_set = []
        #对于每一个puzzle， 寻找它的64个子集
        for puzzle in puzzles:
            arr = []
            #调用形成子集函数
            self.generate_subset(puzzle, 0, [], arr)
            puzzles_set.append(arr)
        res = []
        #去字典中查找
        for puzzle_subsets in puzzles_set:
            cur = 0
            for puzzle_subset in puzzle_subsets:
                if puzzle_subset in words_dic:
                    cur += words_dic[puzzle_subset]
            res.append(cur)
        return res 
    def generate_subset(self, puzzle, idx, pre, arr):
        if idx == 7:
            arr.append(tuple(sorted(pre)))
            return 
        elif idx == 0:
            pre.append(puzzle[0])
            self.generate_subset(puzzle, idx + 1, pre, arr)
        else:
            self.generate_subset(puzzle, idx + 1, pre, arr)
            pre.append(puzzle[idx])
            self.generate_subset(puzzle, idx + 1, pre, arr)
            pre.remove(puzzle[idx])
```
