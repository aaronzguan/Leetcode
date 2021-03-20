## 快速排序 - Quick Sort O(NlogN)

先整体有序，再局部有序。

分成**左右两个部分**，**递归方法**再各局部排序左右两个部分, <= P 的在左边，>=P的在右边

平均时间复杂度：O(NlogN)，最坏时间复杂度为O(N^2^)

```python
def quickSort(array, left, right):
    if (left >= right):
        return
    start, end = left, right
    # 拿到pivot数值而非index,因为array之后会改变，pivot随之也会移动
    pivot = array[(left+right)//2]
    
    while left <= right:
        # 找第一个不应该在左边部分的，即比pivot大
        while left <= right and array[left] < pivot:
            left += 1
        # 找第一个不应该在右边部分的，即比pivot小
        while left <= right and array[right] > pivot:
            right -= 1
        if left <= right:
            # 交换左右两个数，即左边大的数放在右边，右边小的数放在左边
            array[left], array[right] = array[right], array[left]
            # 更新左右两个pointer位置
            left += 1
            right -= 1
      
  	# right左边 <= P， left右边 >= P
  	# 递归方法各排序左右两个部分
  	quickSort(array, start, right)
  	quickSort(array, left, end)
```



```python
def partition(arr, low, high, pivot_index):
  """
  Make sure all the elements at the right of pivot_index is smaller
  """
    pivot = arr[pivot_index]
    arr[pivot_index], arr[high] = arr[high], arr[pivot_index] # Put the pivot at the right-most to make sure all the left elements can compare with the pivot
    index = low
    for j in range(low, high):
        if arr[j] <= pivot: # Note here <= is important
            arr[index], arr[j] = arr[j], arr[index] # If the j-th element is smaller or equal to the pivot, put the j-th element at the index position, then increase the index
            index += 1
    arr[index], arr[high] = arr[high], arr[index] # At the end, put the pivot to the index position, therefore, all the elements at the left are smaller than or eqaul to the pivot
    return index

def quick_sort(arr, low, high):
    if low < high:
        pivot_index = np.random.randint(low, high) # Random select the pivot
        pivot_index = partition(arr, low, high, pivot_index)
        quick_sort(arr, low, pivot_index-1) # Sort [low, pivot_index-1]
        quick_sort(arr, pivot_index+1, high) # Sort [pivot_index+1, high]
        
quick_sort(array, 0, len(array-1))
```

