## Find the missing number (M) - 字节笔试题

从0,1,2,...,n这n+1个数中选择n个数，找出这n个数中缺失的那个数，要求O(n)尽可能小。

```
Input:
[0,1,2,3,4,5,7]
Output
6
```

## 思路：二分　O(logN)

找到中心mid，若当前中心不是mid，那么表示missing number在左半部分（包含mid是missing number的情况）

若当前中心是mid，那么表示missing number在右半部分，进行二分即可

当最后缩小到窗口有四种可能的情况：

1. `[left (missing), right]` ，当前left即为missing number
2. `[left, missing, right]`，left 和right的中间为missing number
3. `[left, right (missing)]`，当前right即为missing number
4. `[left, right, missing]`，　missing为最后的数，即a[-1]

```python
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 找缺失数字
# @param a int整型一维数组 给定的数字串
# @return int整型
#
class Solution:
    def solve(self , a ):
        N = len(a)
        left, right = 0, N - 1
        while left + 1 < right:
            mid = (left + right) // 2
            # The missing number is at Right
            if a[mid] == mid:
                left = mid + 1
            # The missing number is at Left
            else:
                right = mid
                
        if a[left] != left:
            return left
        if a[right] != right:
            return right
        mid = (left + right) // 2
        if a[mid] != mid:
            return mid
        return N
```

