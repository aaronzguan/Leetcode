## Generate all permutation of a String

给出一个字符串，找到它的所有排列，注意同一个字符串不要打印两次。

```
输入："abb"
输出：
["abb", "bab", "bba"]
```

### 思路：

利用生成子集的思路去重，对string进行排序，因此我们可以根据：

* 如果当前一位`str[i]`未被选择，且`str[i]`与前一位`str[i-1]`相等，并且`str[i-1]`未被选择
* 那么不选择`str[i]`，继续考虑下一位`str[i+1]`

```python
class Solution:
    """
    @param str: A string
    @return: all permutations
    """
    def stringPermutation2(self, str):
        str = sorted(str)
        result = []
        # 存储每一位的被选取状态
        visited = [False] * len(str)
        self.helper(str, "", visited, result)
        return result
    
    def helper(self, str, permutation, visited, result):
        if len(permutation) == len(str):
            result.append(permutation)
            return

        for i in range(len(str)):
            if visited[i]:
                continue
        	# 去重：如果前一位和这一位相同，且前一位和这一位均未被选择，去重
            if i > 0 and str[i] == str[i - 1] and not visited[i - 1]:
                continue
            # 如果选择，更新标记以避免选到同一位元素，继续搜索下一位
            # 下一位搜索结束，回溯时把标记改回来，即代表这一位不选择
            visited[i] = True
            self.helper(str, permutation + str[i], visited, result)
            visited[i] = False
```

