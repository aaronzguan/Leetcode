## Lintcode 139 · 最接近零的子数组和 (M)

给定一个整数数组，找到一个和最接近于零的子数组。返回满足要求的子数组的起始位置和结束位置。

```
输入: 
[-3,1,1,-3,5] 
输出: 
[0,2]
解释: [0,2], [1,3], [1,1], [2,2], [0,4]
```



### 思路

先对数组求一遍前缀和，然后把前缀和排序，令排完序的前缀和是B数组 题目要求子数组的和最接近0，也就是B数组两个值相减最接近0 既然B数组两个值相减最接近0，也就是B数组两个最接近的数 对B数组排序，最接近的数肯定是相邻的 排完序后，我们只要找相邻元素做差就好了

```python
class Solution:
    """
    @param: nums: A list of integers
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def subarraySumClosest(self, nums):
        prefix_sum = [(0, -1)]
        for i, num in enumerate(nums):
            prefix_sum.append((prefix_sum[-1][0] + num, i))
            
        prefix_sum.sort()
        
        closest, answer = float('inf'), []
        for i in range(1, len(prefix_sum)):
            
            if closest > prefix_sum[i][0] - prefix_sum[i - 1][0]:
                closest = prefix_sum[i][0] - prefix_sum[i - 1][0]
                left = min(prefix_sum[i - 1][1], prefix_sum[i][1]) + 1
                right = max(prefix_sum[i - 1][1], prefix_sum[i][1])
                answer = [left, right]
        
        return answer
```

