## Increasing Coding System

TuSimple needs a coding system for word. The letters of the word in this coding system are in lexicographical order (each character is smaller than the next character).

The coding system works like this:

* Only includes small characters of the alphabet
* The words are sorted by increasing length
* For the same length, the words are sorted in lexicographical order.

The order can be seen like this:

```
a, b, ..., z, ab, ..., az, bc, ..., abcde, abcdf, ..., abcdefghijklmnopqrstuvwxyz
```

**Example:**

```
Input String: ab
result: 27
ab is the 27th word in this coding system
```

### 动态规划

`dp[i][j]`表示以字母`j`开头的长度为`i`的字符串起始位置

```python
def get_rank_for_coding_system(word):
    l = len(word)
    if l == 1:
        return ord(word) - ord('a') + 1
    
    dp = [[0] * 26 for _ in range(l)]
    
    for i in range(26):
        dp[0][i] = i + 1
    
    for i in range(1, l):
        for j in range(26 - i):
            if j == 0:
                dp[i][j] = dp[i-1][26-i] + 1  # the final order of last length + 1
            else:
                dp[i][j] = dp[i][j-1] + (dp[i-1][26-i] + 1 - dp[i-1][j])  # (dp[i-1][26-i] + 1 - dp[i-1][j]) is the total number of words with length i and starts with character j - 1
    
    res = 0
    for i in range(l):
        if i == 0:
            res += dp[l-1][ord(word[i]) - ord('a')]
        else:
            res += dp[l-i-1][ord(word[i]) - ord('a')] - dp[l-i-1][ord(word[i-1]) - ord('a') + 1]
    return res
            
    
```

