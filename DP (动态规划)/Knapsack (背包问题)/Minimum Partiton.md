## Minimum Partition

划分数组为a和b，使得|a-b|最小，求该最小的差值。

Given a set of positive integers, write a function to divide it into two sets S1 and S2 such that the absolute difference between their sums is minimum.

If there is a set `S` with `n` elements, then if we assume Subset1 has `m` elements, Subset2 must have `n-m` elements and the value of `abs(sum(Subset1) – sum(Subset2))` should be minimum.

```
输入: nums = [1, 6, 11, 5]
输出: 1
解释: 
Subset1 = [1, 5, 6]，和是12
Subset2 = [11]，和是11
abs(11 - 12) = 1

输入: nums = [1, 2, 3, 4]
输出: 0
解释: 
Subset1 = [1, 4], 和是 5
Subset2 = [2, 3], 和是5   
abs(5 - 5) = 0   
```

### 思路：

设这个数组所有数之和为SUM，其中一个较小的数组之和为X，问题变为求 |SUM-X-X| = |SUM - 2X|的最小值

即求使得X尽可能接近SUM/2的最大值 = **数组中挑出若干个数尽可能填满一个大小为SUM/2的背包**

### 1. DP

`dp[i][j]`表示前i个数的和能否是 j，最后则从`dp[-1][-1]`到`d[-1][0]`遍历看第一个`dp[-1][j] == 1`的`j`是多少，这个`j`则为最接近SUM/2的和

- `f[i][j]` for whether there is a combination from first `i` numbers that sums to `j`
- `f[i][j] = f[i-1][j] || f[i-1][j-nums[i-1]]`

A. 二维数组DP：

```python
class Solution:
    """
    @param nums: the given array
    @return: the minimum difference between their sums 
    """
    def findMin(self, nums):
        # write your code here
        capacity = sum(nums) // 2
        dp = [[False] * (capacity + 1) for _ in range(len(nums) + 1)]
        
        # 初始化，前i个数的和为0，只要都不选即可，所以是可以的
        for i in range(len(nums) + 1):
            dp[i][0] = True
        
        for i in range(1, len(nums) + 1):
            for j in range(1, capacity + 1):
                # 选当前的数
                if j >= nums[i - 1]:
                    dp[i][j] = dp[i-1][j] or dp[i - 1][j - nums[i - 1]]
                else:
                # 不选当前的数
                	dp[i][j] |= dp[i-1][j] 
        
        for j in range(capacity, -1, -1):
            if dp[-1][j]:
                return abs(sum(nums) - j - j)
```

B. 滚动数组优化：

```python
class Solution:
    """
    @param nums: the given array
    @return: the minimum difference between their sums 
    """
    def findMin(self, nums):
        # write your code here
        capacity = sum(nums) // 2
        dp = [[False] * (capacity + 1) for _ in range(2)]
        
        # 初始化
        for i in range(2):
            dp[i][0] = True
        
        for i in range(1, len(nums) + 1):
            # 对新的一层开始时初始化
            dp[i%2][0] = True
            for j in range(1, capacity + 1):
                # 选当前的数
                if j >= nums[i - 1]:
                    dp[i%2][j] = dp[(i-1)%2][j] or dp[(i - 1)%2][j - nums[i - 1]]
                else:
                # 不选当前的数
                	dp[i%2][j] = dp[(i-1)%2][j] 
        
        for j in range(capacity, -1, -1):
            if dp[len(nums)%2][j]:
                return abs(sum(nums) - j - j)
```

C. 一维数组优化：

```python
class Solution:
    """
    @param nums: the given array
    @return: the minimum difference between their sums 
    """
    def findMin(self, nums):
        # write your code here
        capacity = sum(nums) // 2
        dp = [False] * (capacity + 1)
        dp[0] = True
        
        for i in range(len(nums)):
            for j in range(capacity, -1, -1):
                if j >= nums[i]:
                    dp[j] |= dp [j - nums[i]]
        
        for j in range(capacity, -1, -1):
            if dp[j]:
                return abs(sum(nums) - j - j)
            
```

### 3. DFS

```

```

