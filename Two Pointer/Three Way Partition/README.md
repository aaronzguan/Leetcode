## Three Way Partition 

Three Way Partition (三分法) 可以**在 O(N) 的时间复杂度内将一个数组分为3个部分**。需要注意的是三分法是分类，并不是排序。

- 可以**按一个定值**将数分为三部分 **[ 小于数值的部分， 等于数值的部分，大于数值的部分 ]**
- 可以**按一个范围**将数组分为三个部分 **[ 小于范围的部分， 符合范围的部分，大于范围的部分 ]**

### 具体思路：

1. 定义两个指针 `Left, Right`:  `Left`是左侧部分的最右边界，`Right`是右侧部分的最左边界
2. 定义当前位置指针 `cur`, 从左向右移动 `cur` , 直到 `cur` 超过 `Right`:
   1. 如果`nums[cur] < target`, swap it with `nums[left]`，move **both `cur` and `left` to the right**
   2. 如果`nums[cur] > target`, swap it with `nums[right]`, move **only `right` to the left**
   3. 如果`nums[cur] == target`, move `cur` to the right



##### 按`target`将数组分为三部分  [ <target， =target，>target]:

```python
def threeWayPartition(nums, target):
    # Initialize the boundary for left and right part:
    # 	- the rightmost boundary for the left part is 0 
    # 	- the leftmost boundary for the right part is len(nums) - 1
    left, right = 0, len(nums) - 1 
    # index of current element to consider
    cur = 0
    while cur <= right:
        # 若cur指向的值小于target，则和left进行交换，保证小于target的值位于左侧
        if nums[cur] < target:
            nums[left], nums[cur] = nums[cur], nums[left]
            # 此时可以确认left是小于target的，自信++
            left += 1
            # 换上来的数据也肯定是经过筛选，不用再次参与循环，自信++
            cur += 1
        # 若cur指向的值大于target，则和right进行交换，保证大于target的值位于右侧
        elif nums[cur] > target:
            nums[right], nums[cur] = nums[cur], nums[right]
            # 这里只移动right，不移动cur，目的是为了能在下一次循环中比较从right换过来的值
            right -= 1
        # 若cur指向的值等于target/位于中间范围，跳过
        else:
            cur += 1
```



##### 按范围`[rangeLeft, rangeRight]` 将数组分为三部分：

```python
def threeWayPartition(nums, rangeLeft, rangeRight):
    left, right = 0, len(nums) - 1 
    # index of current element to consider
    cur = 0
    while cur <= right:
        # 若cur指向的值小于范围，则和left进行交换，保证小于范围的值位于左侧
        if nums[cur] < rangeLeft:
            nums[left], nums[cur] = nums[cur], nums[left]
            # 此时可以确认left是小于范围的，自信++
            left += 1
            # 换上来的数据也肯定是经过筛选，不用再次参与循环，自信++
            cur += 1
        # 若cur指向的值大于范围，则和right进行交换，保证大于范围的值位于右侧
        elif nums[cur] > rangeRight:
            nums[right], nums[cur] = nums[cur], nums[right]
            # 这里只移动right，不移动cur，目的是为了能在下一次循环中比较从right换过来的值
            right -= 1
        # 若cur指向的值位于中间范围，跳过
        else:
            cur += 1
```

