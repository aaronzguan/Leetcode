https://www.geeksforgeeks.org/print-all-jumping-numbers-smaller-than-or-equal-to-a-given-value/

A number is called as a Jumping Number if all adjacent digits in it differ by 1. The difference between ‘9’ and ‘0’ is not considered as 1. Given a positive number x, print all Jumping Numbers smaller than or equal to x. The numbers can be printed in any order. All single digit numbers are considered as jumping numbers.

```
Input: x = 20
Output:  0 1 2 3 4 5 6 7 8 9 10 12

Input: x = 105
Output:  0 1 2 3 4 5 6 7 8 9 10 12
         21 23 32 34 43 45 54 56 65
         67 76 78 87 89 98 101
```

### Method 1 - Brute Force

```python
"""
Solution 1
Brute Force
Time Complexity: O(n)
"""
def jumping_number(target):

   
    def is_jumping(n):
      if n <= 10:
        return True

      n = list(str(n))

      if len(n) == 2:
        diff = abs(int(n[0]) - int(n[1]))
        if diff == 1:
          return True
        else:
          return False

      for i in range(1, len(n) - 1):
        left = abs(int(n[i]) - int(n[i-1]))
        right = abs(int(n[i]) - int(n[i+1]))
        if left == 1 and right == 1:
          return True
        else:
          return False
        
     res = []
    # Iterate from 0 to target and check whether the number is jumping number or not
     for num in range(target+1):
        if is_jumping(num):
          res.append(num)
     return res
```

### Method 2 - BFS

```python
"""
Solution 2
BFS
Time Complexity: O(K)
"""
def jumping_number(target):
  	"""
  	Return all jumping number smaller than target
  	"""
    def bfs(target, num):
      res = []
      q = []
      q.append(num)
      while q:
        num = q.pop(0)
        if num < target:
          # Add the jumping number
          res.append(num)
          last_dig = num % 10
          if last_dig == 0:
            q.append((num*10) + (last_dig+1))
          elif last_dig == 9:
            q.append((num*10) + (last_dig-1))
          else:
            q.append((num*10) + (last_dig-1))
            q.append((num*10) + (last_dig+1))
      return res

    res = []
    res.append(0)
    for i in range(1, 10):
      res.extend(bfs(target, i))

    return res
```

