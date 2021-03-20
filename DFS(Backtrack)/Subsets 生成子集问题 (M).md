# 子集问题

## 无重复的数：78. Subsets (M)

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

```python
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        results = []
        self.dfs(nums, 0, [], results)
        return results
    
    def dfs(self, nums, index, subset, results):
        results.append(list(subset))
        
        for i in range(index, len(nums)):
            subset.append(nums[i])
            self.dfs(nums, i + 1, subset, results)
            subset.pop()
```



## 有重复的数：90. Subsets II (M) 选代表

Given a collection of integers that might contain **duplicates**, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

### 去重操作：选代表

防止 [1, 2, 2] 产生两个[1, 2]，我们只选择第一个2而不选择第二个

```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums = sorted(nums) # 排序，将相同的数挤在一起
        result = []
        self.helper(nums, 0, [], result)
        return result
    
    def helper(self, nums, index, subset, result):
        result.append(subset)
        
        for i in range(index, len(nums)):
            # 去重操作
            if i > 0 and i > index and nums[i] == nums[i - 1]:
                continue
                
            self.helper(nums, i + 1, subset + [nums[i]], result)
        
```

