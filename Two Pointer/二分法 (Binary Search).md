# 二分法 - Binary Search - O(logN)

## 算法描述

1. 我们需要先确定二分的上下界限，由于两个数组 A, B 均有序，所以下界为 `min(A[0], B[0])`，上界为 `max(A[A.length - 1], B[B.length - 1])`.

2. 判断当前上下界限下的 mid`(mid = (start + end) / 2)` 是否为我们需要的答案；这里我们可以分别对两个数组进行二分来找到两个数组中小于等于当前 mid 的数的个数`cnt1`与 `cnt2`，`sum = cnt1 + cnt2` 即为 A 跟 B 合并后小于等于当前mid的数的个数.

3. 如果` sum < k`，即中位数肯定不是` mid`，应该大于 `mid`，更新 `start` 为` mid`，否则更新 `end` 为` mid`，之后再重复第二步

4. 当不满足 `start + 1 < end` 这个条件退出二分循环时，再分别判断一下`start`跟 `end` ，最终返回符合要求的那个数即可

   

   #### 注意事项

   - **当循环条件为 `left <= right` 时，`right = mid - 1`；**
   - **当循环条件为 `left < right` 时，`right = mid`。**

   解释如下：在循环条件为 `left <= right` 时，如果 right = mid，会出现循环无法退出的情况，例如 `left = 1`，`right = 1`，此时 mid 也等于 1，如果此时继续执行 `right = mid`，那么就会无限循环；在循环条件为 `left < right`，如果 `right = mid - 1`，会错误跳过查找的数，例如对于数组 [1,2,3]，要查找 1，最开始 l = 0，h = 2，mid = 1，判断 key < arr[mid] 执行 `right = mid - 1 = 0`，此时循环退出，直接把查找的数跳过了。

   - **`Left` 的赋值一般都为 `left = mid + 1`。**

#### - Problem Situation:

When the array is **<u>already sorted</u>**, and you have to search a given element in the array. Instead of having to traverse each element, **binary search** cuts the search time down to ***O(log n)***, where n is the length of the array.

#### - Algorithm

Search a sorted array by repeatedly **<u>dividing</u>** the search interval **<u>in half</u>**. Begin with an interval covering the whole array. `Left = 0, Right = len(array), Middle = (Left + Right) // 2`. If the value of the search key is less than the item in the middle of the interval, narrow the interval to the lower half. Otherwise narrow it to the upper half. Repeatedly check until the value is found or the interval is empty.



***Q: Find the index of the first target in the array***

```python
def binary_search(array, target):
	"""
	Return the index of target in the array
	"""
	left = 0
	right = len(array)
	
	while left <= right:
		mid = (left + right) // 2

		if array[mid] == target:
			return mid
		elif array[mid] > target:
			right = mid - 1
		else:
			left = mid + 1
	return -1
```



```python
def binary_search(array, target):
    """
    array 一定为已排好序的，
    寻找first position of target
    """
    if len(array) == 0:
        return -1
    
    left, right = 0, len(array) - 1
    while left + 1 < right: # left + 1非常重要，保证不会出现死循环
        mid = (left + right) // 2
        # 当array[mid]为target, 左边可能还存在target,所以让right = mid来搜寻左边部分。
        if array[mid] == target:
            right = mid
        # 当array[mid]小于target,证明target在右边，所以让left = mid+1来搜寻右边部分
        elif array[mid] < target:
            left = mid + 1
        else:
        	right= mid - 1
    # left 一定比right小，所以先判断left是否为target，从而保证是first position
    if array[left] == target:
        return left
   	if array[right] == target:
        return right
   
	return -1
```



