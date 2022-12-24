## Lintcode 183 · 木材加工 (H)

有一些原木，现在想把这些木头切割成一些长度相同的小段木头，需要得到的小段的数目至少为 `k`。给定L和k，你需要计算能够得到的小段木头的最大长度。

```plain
输入:
L = [232, 124, 456]
k = 7
输出: 114
Explanation: 我们可以把它分成114cm的7段，而115cm不可以
```

### 二分法

以切割的长度为目标进行二分法．同时检查能得到的木头的长度．若少于k，则表示当前切割的长度太长导致获得的数目太少，则减少长度．若大于k，则表示当前切割的长度太短导致获得的数目太多，增加长度

```python
class Solution:
    """
    @param L: Given n pieces of wood with length L[i]
    @param k: An integer
    @return: The maximum length of the small pieces
    """
    def woodCut(self, L, k):
        # write your code here
        if not L:
            return 0
        
        # Determine the wood lenth range
        start, end = 1, max(L)
        
        while start + 1 < end:
            mid = (start + end) // 2
            #　获得的数量多于k，则表示切割长度偏小，增加长度
            if self.get_num_of_pieces(mid, L) >= k:
                start = mid
            # 获得的数量小于k，则表示切割长度偏大，减少长度
            else:
                end = mid
        
        if self.get_num_of_pieces(start, L) >= k:
            return start
        
        if self.get_num_of_pieces(end, L) >= k:
            return end
        
        return 0
    
    def get_num_of_pieces(self, length, L):
        pieces = 0
        for l in L:
            pieces += l // length
        return pieces
```

