# House Robber 打家劫舍

只能选择不相邻的权重，求能所得的最大权重。

## 198. House Robber (M)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

有array，每个元素分别表示权重，不可以同时取左右相邻的元素，求所能取得的最大权重。

**Example:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

### Solution - DP

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        dp = [0] * (len(nums) + 1)
        
        dp[1] = nums[0]
        
        for i in range(2, len(nums) + 1):
            dp[i] = max(dp[i - 2] + nums[i - 1], dp[i-1]) #选择当前的那么考虑前i-2个数最大权重加上当前权重，不选择当前的则考虑前i-1最大
        return dp[-1]
```

滚动数组优化：

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
    	if not nums:
            return 0
        size = len(nums)
        if size == 1:
            return nums[0]
        
        prev, cur = nums[0], max(nums[0], nums[1])
        for i in range(2, size):
            prev, cur = cur, max(prev + nums[i], cur)
        return cur
```

## 213. House Robber II (M)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

头尾相连，增加环形数组的条件，求所能取得的最大值

**Example :**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

### Solution - DP：

* 把此环状排列房间问题约化为两个单排排列房间子问题：

  * 在不选择头的情况下（即 `nums[1:]`），最大金额是 p1
  * 在不选择尾的情况下（即 `nums[:n-1]`），最大金额是 p2 
  * **综合最大值：** 为以上两种情况的较大值，即`max(p1,p2)`

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        if len(nums) == 1:
            return nums[0]
        
        dp = [0] * (len(nums) + 1)
        
        #选择头不选择尾的情况，考虑nums[:n-1]
        dp[1] = nums[0]
        for i in range(2, len(nums)):
            dp[i] = max(dp[i-1], dp[i - 2] + nums[i - 1])
        cur_max = dp[-2]
        #选择尾不选择头的情况，考虑nums[1:]
        dp = [0] * (len(nums) + 1)
        for i in range(2, len(nums) + 1):
            dp[i] = max(dp[i-1], dp[i - 2] + nums[i - 1])
            
        cur_max = max(cur_max, dp[-1])
        return cur_max
```

滚动数组优化：

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        size = len(nums)
        if size == 1:
            return nums[0]
        return max(self.helper(nums[:-1]), self.helper(nums[1:]))
   	
    def helper(self, nums):
        cur, pre = 0, 0
        for num in nums:
            cur, pre = max(pre + num, cur), cur
        return cur
```

## 337. House Robber III (M)

数组转化为二叉树，每个node有权重，不可以选择相邻的父子节点，求可选择的最大权重。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return max(self.helper(root))
    
    def helper(self, node):
        if not node:
            return [0, 0] # (选择了当前节点的最大权重,不选择当前节点的最大权重)
        
        left = self.helper(node.left)
        right = self.helper(node.right)
        # 选择该点，那么左右子节点都不可选择
        rob = node.val + left[1] + right[1]
        # 不选择该店，那么左右子节点可选择也可不选择，则分别找最大的
        not_rob = max(left) + max(right)
    
        return [rob, not_rob]
    
    # Time complexity: O(N) since we visit all nodes once.
```

