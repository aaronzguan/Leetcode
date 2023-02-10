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

### Solution 1 -**多重背包问题**

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

### Solution 2 - Top Down with Memorization

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        nums_coins = self.dfs(coins, 0, amount, {})
        return nums_coins if nums_coins != float("inf") else -1
    
    def dfs(self, coins, cur_sum, amount, memo):
        if cur_sum == amount:
            return 0
        # Memorization, the minimum number of coins used to make up amount from cur_sum
        if cur_sum in memo:
            return memo[cur_sum]

        num_coins = float("inf")
        for coin in coins:
            if cur_sum + coin <= amount:
                num_coins = min(num_coins, 1 + self.dfs(coins, cur_sum + coin, amount, memo))
        
        memo[cur_sum] = num_coins
        return memo[cur_sum]
```



## 518. Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

**Example:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

计算给出的总金额可以换取的硬币数量的**总方案数**

### Solution 1 - Bottom Up DP **多重背包问题**

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

### Solution 2 - Top Down with Memorization

```python
class Solution(object):
    def change(self, amount, coins):
        """
        :type amount: int
        :type coins: List[int]
        :rtype: int
        """
        return self.dfs(coins, 0, 0, amount, {})
    
    def dfs(self, coins, index, cur_sum, amount, memo):
        if cur_sum == amount:
            return 1
        
        if index >= len(coins):
            return 0
        
        # Memorization
        if (index, cur_sum) in memo:
            return memo[(index, cur_sum)]
        
        # Use the current coin
        ways1 = 0
        if cur_sum + coins[index] <= amount:
           ways1 = self.dfs(coins, index, cur_sum + coins[index], amount, memo)
        
        # Not use the current coin
        ways2 = self.dfs(coins, index+1, cur_sum, amount, memo)
        memo[(index, cur_sum)] = ways1 + ways2

        return memo[(index, cur_sum)]
```

