## 换零钱问题

有关问题：**322. Coin Change** 和 **518. Coin Change 2**

## 322. Coin Change

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the **fewest number of coins** that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

计算给出的总金额可以换取的**最少**的硬币数量

```
输入：
[1, 2, 5]
11
输出： 3
解释： 11 = 5 + 5 + 1
```

**多重背包问题**

`dp[i]`表示换算到总金额 i 所用的最少硬币数量

$dp[i][j]=max(dp[i-1][j],dp[i-1][j-k*coin[i]]+k) \quad (0\leq k*coin[i] \leq j)$

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        dp = [float('inf') for _ in range(amount + 1)]
        dp[0] = 0
        for i in range(1, amount + 1):
            for j in range(len(coins)):
                if i >= coins[j]:
                    dp[i] = min(dp[i], 1 + dp[i - coins[j]])
        return dp[-1] if dp[-1] != float('inf') else -1
```

n表示金额数，m表示硬币面值数

时间复杂度：O(nm)

空间复杂度：O(n)

## 518. Coin Change

计算给出的总金额可以换取的硬币数量的**总方案数**

**多重背包问题**

`dp[i]`表示可换算到总金额 i 总方案数

```python
class Solution(object):
    def change(self, amount, coins):
        """
        :type amount: int
        :type coins: List[int]
        :rtype: int
        """
        dp = [0 for _ in range(amount + 1)]
        dp[0] = 1
        # 注意 for loop的顺序，来考虑1 + 2 和 2 + 1都等于3的情况，但其实为一种方案
        for i in range(len(coins)):
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]
        
        return dp[-1]
```