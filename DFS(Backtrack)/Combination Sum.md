# Combination Sum 系列问题

## 39. Combination Sum (M) - 无重复数组不限次选择组成Target

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. 

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        result = []
        self.helper(candidates, 0, [], target, result)
        return result
   
	def helper(self, candidates, index, subset, target, result):
        if sum(subset) > target:
            return
        if sum(subset) == target:
            result.append(subset)
        
        for i in range(index, len(candidates)):
            self.helper(candidates, i, subset + [candidates[i]], target, result)
    
    """
    The fan-out of each node would be bounded to N
    The maximal depth of the tree would be T/M, where M is the smallest value among the candidates
    So the maximal number of nodes in N-ary tree of T/M height, would be N^(T/M + 1)
    """
```



## 40. Combination Sum II (M) - 有重复数仅选择一次组成Target

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

思路：

因为不能有重复的，所以对于candidates中有重复的数时要选代表，如 [1,1,7] target = 8, 只能选第一个1

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        result = []
        candidates.sort()
        self.helper(candidates, 0, [], target, result)
        return result
   
	def helper(self, candidates, index, subset, target, result):
        if sum(subset) > result:
            return
        if sum(subset) == target:
            result.append(subset)
        
        for i in range(index, len(candidates)):
            if i > index and candidates[i] == candidates[i - 1]:
                continue
            
            self.helper(candidates, i + 1, subset + [candidates[i]], target, result)
```

