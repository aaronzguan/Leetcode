## Quick Select O(N) - Select the kth smallest element

Quick Select使用了Quick Sort， 但是Quick Sort 时间复杂度为O(NlogN)， 由于Quick Select只考虑所寻找的目标所在的那一部分子数组，所以Quick Select平均时间复杂度为 O(N)

```python
class Solution:
    """
    @param k: An integer
    @param nums: An integer array
    @return: kth smallest element
    """
    def kthSmallest(self, k, nums):
        # write your code here
        self.quickSelect(0, len(nums) - 1, k-1, nums)
	
    def quickSelect(self, left, right, k, nums):
        # 0. 保存当前开始和结束的Index
        start, end = left, right
        # 1. 使用经典的quick sort中partition模板
        pivot = nums[(left + right) // 2]
        while left <= right:
            while left <= right and nums[left] < pivot:
                left += 1
            while left <= right and nums[right] > pivot:
                right -= 1
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1 
                right -= 1
        
        # 2. 查看k在哪个区间再去只排序该区间
        if k >= left:
            # 2.1 k大于等于left，则k在右区间，再去排[left, end]区间即可
            self.quickSelect(left, end, k, nums)
        if k <= right:
            # 2.2 k等于right，则k在左区间，再去排[start, right]区间即可
            self.quickSelect(start, right, k, nums)
        
        # 3. 当找到第K小的数时，此时 left == right == K, 再过一遍quickSelect之后则 right < k < left
        # 此时可返回nums[k]
        return nums[k]
```

