## Merge Sort 归并排序

归并排序使用了分治的思想，无论什么样的数据，所花费时间都是 **O(NlogN)**

先局部有序再整体部有序

```python
class Solution(object):
    def sortArray(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        self.mergeSort(nums, 0, len(nums) - 1)
        return nums

    
    def mergeSort(self, array, start, end):
        if start >= end:
            return
        # 左边部分mergeSort
        self.mergeSort(array, start, (start + end) // 2)
        # 右边部分mergeSort
        self.mergeSort(array, (start+end) // 2 + 1, end)
        
        self.merge(array, start, end)
    
    def merge(self, array, start, end):
        temp = []
        
        middle = (start + end) // 2
        left = start
        right = middle + 1
        while left <= middle and right <= end:
            # 左边的数小于右边的，temp增加左边的数
            if array[left] < array[right]:
                temp.append(array[left])
                left += 1
            # 左边的数大于右边的，temp增加右边的数
            else:
                temp.append(array[right])
                right += 1
                
        # 确保左右两边都结束
        while left <= middle:
            temp.append(array[left])
            left += 1
            
        while right <= end:
            temp.append(array[right])
            right += 1
            
        array[start:end+1] = temp
```

