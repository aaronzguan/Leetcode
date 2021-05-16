## Lintcode 570 · 寻找丢失的数 II (M)

给一个由 1 - `n` 的整数随机组成的一个字符串序列，其中丢失了一个整数，请找到它。

n < 100, 数据保证有且仅有唯一解. 如果您找到的答案列表丢失了多个数字，可能是您没有找到正确的字符串拆分方式

**Example**

```
输入: n = 20 和 str = 19201234567891011121314151618
输出: 17
解释:
19'20'1'2'3'4'5'6'7'8'9'10'11'12'13'14'15'16'18
```



### Backtracking

由于存在很多可能的拆分情况，对于一个数可以有三种情况：个位数，十位数，百位数．所以需要用到 DFS并且回溯

```python
class Solution:
    """
    @param n: An integer
    @param str: a string with number from 1-n in random order and miss one number
    @return: An integer
    """
    def findMissing2(self, n, str):
        # write your code here
        self.result = -1
        self.helper(n, str, 0, set(), [])
        return self.result
    
    def helper(self, n, str, index, seq, ans):
        if index >= len(str):
            ans = []

            for i in range(1, n + 1):
                if i not in seq:
                    ans.append(i)
            
            if len(ans) == 1:
                self.result = ans[0]
            
            return
        
        if str[index] == '0':
            return
        
        for i in range(1, 3):
            num = int(str[index: index + i])
            
            if num >=1 and num <= n and num not in seq:
                seq.add(num)
                self.helper(n, str, index+i, seq, ans)
                seq.remove(num)
```

