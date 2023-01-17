## 合并k个排序数组

将 *k* 个有序数组合并为一个大的有序数组。

```
Input: 
  [
    [1, 3, 5, 7],
    [2, 4, 6],
    [0, 8, 9, 10, 11]
  ]
Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
```

### 1. 归并排序：

**时间复杂度**： O(NlogK), N 是所有元素个数, K是array个数，We can merge two sorted array in O(n) time where n is the total number of elements in two arrays，可以想象成一个Array为N大小，所有其他Array长度都为1。

```python
class Solution:
    """
    @param arrays: k sorted integer arrays
    @return: a sorted array
    """
    def mergekSortedArrays(self, arrays):
        # write your code here
        return self.merge_range(arrays, 0, len(arrays) - 1)
        
    def merge_range(self, arrays, start, end):
        if start == end:
            return arrays[start]
        mid = (start + end)//2
        
        left = self.merge_range(arrays, start, mid)
        right = self.merge_range(arrays, mid + 1, end)
        
        return self.merge_two_array(left, right)
        
    def merge_two_array(self, array1, array2):
        i, j = 0, 0
        array = []
        while i < len(array1) and j < len(array2):
            if array1[i] < array2[j]:
                array.append(array1[i])
                i += 1
            else:
                array.append(array2[j])
                j += 1
        
        while i < len(array1):
            array.append(array1[i])
            i += 1
        
        while j < len(array2):
            array.append(array2[j])
            j += 1
        return array
```

### 2. 优先队列 Heap

- 先将每个有序数组的第一个元素压入优先队列中
- 不停的从优先队列中取出最小元素（也就是堆顶），再将这个最小元素所在的有序数组的下一个元素压入队列中 eg. 最小元素为x，它是第j个数组的第p个元素，那么我们把第j个数组的第p+1个元素压入队列

```python
import heapq
class Solution:
    """
    @param arrays: k sorted integer arrays
    @return: a sorted array
    """
    def mergekSortedArrays(self, arrays):
        """
        使用heap存储k个值
        """
        heap = []
        for i, array in enumerate(arrays):
            if len(array) == 0:
                continue
            
            heapq.heappush(heap, (array[0], i, 0))
        result = []
        
        while heap:
            val, x, y = heapq.heappop(heap)
            result.append(val)
            if y + 1 < len(arrays[x]):
                heapq.heappush(heap, (arrays[x][y + 1], x, y + 1))
        
        return result
```

**时间复杂度**：O(NlogK)，队列元素数量的上限就是K，每次压入元素和取出元素都是logK的， 因为要把k个数组都排序完成，那么所有元素都会入队 再出队一次，所以总共复杂度是O(NlogK)， N是K个数组里面所有元素的数量

