## 47. Permmutations II (M)

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

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.* 

**Example :**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums = sorted(nums)
        visited = [False for _ in range(len(nums))]
        result = []
        self.helper(nums, visited, [], result)
        return result
        
    def helper(self, nums, visited, permutation, result):
        if len(permutation) == len(nums):
            result.append(list(permutation))
            return
        
        used = set()
        
        for i in range(len(nums)):
            if visited[i]:
                continue
            
            # 这种去重方式更简单易懂
            if nums[i] in used:
                continue
            
            used.add(nums[i])

            visited[i] = True
            permutation.append(nums[i])
            self.helper(nums, visited, permutation, result)
            visited[i] = False
            permutation.pop() 
```



## 267. Palindrome Permutation II (M)

Given a string s, return *all the palindromic permutations (without duplicates) of it*.

You may return the answer in **any order**. If `s` has no palindromic permutation, return an empty list.

**Example:**

```
Input: s = "aabb"
Output: ["abba","baab"]
```

**思路：**

这是一个组合问题 首先这个问题要分字符串长度奇偶讨论，先把每个字母出现多少次数出来．如果奇数个数的字母大于１，那么无法组成palindrome．

若可以组成并有奇数个数的字母，那么奇数个数的字母为mid．

用剩余的字符组成字符串的左半部分，接下来我们只需要给字符串左半部做排列即可．

```python
class Solution(object):
    def generatePalindromes(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        counter = collections.Counter(s)
        odds = filter(lambda x: x % 2, counter.values())
        # More than 1 char has odd numbeer appearance
        if len(list(odds)) > 1:
            return []
        baseStr, mid = self.preProcess(counter)
        
        half_permutations = []
        
        visited = [False for _ in range(len(baseStr))]
        self.get_permutations(baseStr, visited, [], half_permutations)
        
        full_permutations = []
        for hp in half_permutations:
            full_permutations.append(hp + mid + hp[::-1])
        
        return full_permutations
    
    
    def preProcess(self, counter):
        baseStr = mid = ""
        for char in counter:    
            if counter[char] % 2:
                mid = char
            baseStr += char*(counter[char]/2)
        return baseStr, mid

    #　Use the permutation template
    def get_permutations(self, candidates, visited, result, results):
        
        if len(result) == len(candidates):
            results.append("".join(result))
        
        used = set()
        for i in range(len(candidates)):
            if visited[i]:
                continue
            
            if candidates[i] in used:
                continue
            
            used.add(candidates[i])
                
            visited[i] = True
            result.append(candidates[i])
            self.get_permutations(candidates, visited, result, results)
            result.pop()
            visited[i] = False
```

