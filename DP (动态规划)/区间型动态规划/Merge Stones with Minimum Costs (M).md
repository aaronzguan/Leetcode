## 石子归并 · Stone Game

有一个石子归并的游戏。最开始的时候，有n堆石子排成一列，目标是要将所有的石子合并成一堆。合并规则如下：

1. 每一次可以合并相邻位置的两堆石子
2. 每次合并的代价为所合并的两堆石子的重量之和

求出最小的合并代价。

```
输入: [4, 1, 1, 4]
输出: 18
解释: 
  1. 合并第二堆和第三堆 => [4, 2, 4], score = 2
  2. 合并前两堆 => [6, 4]，score = 8
  3. 合并剩余的两堆 => [10], score = 18
```

思路：区间型动态规划

区间动态规划.

设定状态: `dp[i][j]` 表示合并原序列 `[i, j]`的石子的最小分数，**找到一个位置拆分i,j使得两个区间的差最小**

状态转移: `dp[i][j] = min{dp[i][k] + dp[k+1][j]} + sum[i][j], sum[i][j] 表示原序列[i, j]区间的重量和`

边界: `dp[i][i] = sum[i][i] = 0, dp[i][i+1] = sum[i][i+1]`

答案: `dp[0][n-1]`

```python
class Solution:
    """
    @param A: An integer array
    @return: An integer
    """
    def stoneGame(self, A):
        # write your code here
        if not A:
            return 0
        n = len(A)
        
        dp = [[float('inf')] * n for _ in range(n)] # dp[i][j]表示i到j区间内的最小合并代价
        
        # 若长度为1，那么不需要合并，没有合并代价
        for i in range(n):
            dp[i][i] = 0
        
        range_sum = self.get_range_sum(A)
        
        # 对长度进行从短到长遍历。因为长度为1不需要做合并，没有任何合并代价
        for length in range(2, n + 1):
            # 对起点进行遍历
            for i in range(n - length + 1):
                j = i + length - 1
                for mid in range(i, j):
                    dp[i][j] = min(dp[i][j], dp[i][mid] + dp[mid+1][j] + range_sum[i][j])
                    
        return dp[0][-1]
    
    # 用动态规划思想计算range_sum[i][j]，表示i到j区间的和
    def get_range_sum(self, nums):
        n = len(nums)
        range_sum = [[0] * n for _ in range(n)]
        for i in range(n):
            range_sum[i][i] = nums[i]
            for j in range(i+1, n):
                range_sum[i][j] = range_sum[i][j-1] + nums[j]
        return range_sum
```

