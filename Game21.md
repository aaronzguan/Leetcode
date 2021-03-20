Given four numbers from 0 ~ 9 and four operators +, -, *, /. Determine if they can form expression that reach to 21.

```python
def Game21(nums, target):
	operator_list = ['+', '-', '*', '/']

	if len(nums) == 1:
		if abs(nums[0] - target) < 0.001:
			return True
		else:
			return False

	for i in range(len(nums)):
		for j in range(len(nums)):
			if i == j:
				continue

			tmp = []

			# Append the other two remaining numbers
			for k in range(len(nums)):
				if k != i and k != j:
					tmp.append(nums[k])

			for op in operator_list:
				# avoid repeat calculation for + and *
				if (op == '+' or op == '*') and i > j:
					continue

				if op == "/" and nums[j] < 0.001:
					continue

				if op == '+':
					tmp.append(nums[i] + nums[j])
				elif op == '-':
					tmp.append(nums[i] - nums[j])
				elif op == '*':
					tmp.append(nums[i] * nums[j])
				elif op == '/':
					tmp.append(nums[i] / nums[j])
				
        # Do the dfs for the tmp
				if Game21(tmp, target):
					return True
					
				# Backtracking
				tmp.pop()
	return False
```

