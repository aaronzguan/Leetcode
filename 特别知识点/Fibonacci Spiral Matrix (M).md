## Fibonacci Spiral Matrix (M)

Construct a spiral matrix nxn, with all elements in Fibonacci sequence. The top-left is the starting point with the biggest number. The Fibonacci numbers are in clock-wise order.

```
Input:
3
Output:
34 21 13
1 1 8
2 3 5
```

```python
n = int(input())  # Get the size of the square grid

# Recursion to get the n-th fibonacci number
def fibo(num):
    if num <= 0:
        return 0
    if num == 1:
        return 1
    return fibo(num - 1) + fibo(num - 2)


row, col = 0, 0  # define the starting position
directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # define the clock-wise direction
d = 0  # d%4 can denote which direction is being used

grid = [[0 for _ in range(n)] for _ in range(n)]  # construct the empty matrix to store the spiral matrix

# Construct the spiral matrix
for i in range(n ** 2):
    grid[row][col] = str(fibo((n**2) - i))
    dr, dc = directions[d % 4]  # get the correct direction
    new_r = row + dr
    new_c = col + dc
    # Check if the next position is bounded and never visited
    # If the next position is ok, then update the position to the next
    if 0 <= new_r < n and 0 <= new_c < n and grid[new_r][new_c] == 0:
        row = new_r
        col = new_c
    # otherwise, it means it should changed to the next direction and update the position to the next
    else:
        d += 1
        dr, dc = directions[d % 4]
        row, col = row + dr, col + dc

for i in grid:
    print(' '.join(i))
```