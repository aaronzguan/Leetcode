## Word Search III (Didi Campus Recuriting) (M)

Given a 2D squre board and a word, **find the number of a specific word exists in the grid. Return the number**

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

```
Input:
4
C H I G
E A N I
C B A H
D E F G
CHINA

Output:
2
```


```python
n = int(input())
grid = []
for _ in range(n):
	grid.append(list(map(str, input().split())))
word = list(map(str, input().split()))

def search2D(row, col, word):
	# Set a global variable to store the number of words exists
	global count
	if len(word) == 0:
		count += 1

	# Backtracking
	temp = grid[row][col]
	grid[row][col] = '#'

	for dr, dc in ([1, 0], [0, 1], [-1, 0], [0, -1]):
		new_r, new_c = row + dr, col + dc
		if new_r >= 0 and new_r < n and new_c >= 0 and new_c < n:
			if grid[new_r][new_c] == word[0]:
				search2D(new_r, new_c, word[1:])

	grid[row][col] = temp

count = 0
for i in range(n):
	for j in range(n):
		if grid[i][j] == word[0]:
			search2D(i, j, word[1:])
return count
```