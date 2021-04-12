 

## 465. Optimal Account Balancing (M)

A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for $10. Then later Chris gave Alice $5 for a taxi ride. We can model each transaction as a tuple (x, y, z) which means person x gave person y $z. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively (0, 1, 2 are the person's ID), the transactions can be represented as `[[0, 1, 10], [2, 0, 5]]`.

Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.

**Note:**

1. A transaction will be given as a tuple (x, y, z). Note that `x ≠ y` and `z > 0`.
2. Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.

**Example 1:**

```
Input:
[[0,1,10], [2,0,5]]

Output:
2

Explanation:
Person #0 gave person #1 $10.
Person #2 gave person #0 $5.

Two transactions are needed. One way to settle the debt is person #1 pays person #0 and #2 $5 each.
```

```python
class Solution(object):
    def minTransfers(self, transactions):
        """
        :type transactions: List[List[int]]
        :rtype: int
        """

        m = collections.defaultdict(int)
        
        for trans in transactions:
            m[trans[0]] -= trans[2]
            m[trans[1]] += trans[2]
        
        non_zero_debit = [debit for debit in m.values() if debit != 0]
        
        def remove_one_zero_clique(non_zero_debit):
                n = len(non_zero_debit)
                q = collections.deque()
                q.append(([0], non_zero_debit[0]))
                min_zero_set = None
                
                while q:
                    curr_set, curr_sum = q.popleft()
                    if curr_sum == 0:
                        min_zero_set = curr_set
                        break
                    else:
                        for j in range(curr_set[-1] + 1, n):
                            q.append((curr_set + [j], curr_sum + non_zero_debit[j]))
                print(min_zero_set)
                return [non_zero_debit[i] for i in range(n) if i not in min_zero_set]               
                
        N = len(non_zero_debit)
        
        while non_zero_debit:
            non_zero_debit = remove_one_zero_clique(non_zero_debit)
            N -= 1
        return N
            
        
        # declare a default dictionary
#         m = collections.defaultdict(int)
        
#         for trans in transactions:
#             m[trans[0]] -= trans[2]
#             m[trans[1]] += trans[2]
            
#         debit = m.values()
        
#         def dfs(start):
#             # go from the first non-zero debit value
#             # 首先跳过为0的账户
#             while (start <len(debit) and debit[start] == 0):
#                 start += 1
#             # 然后看若此时 start 已经是 debit 数组的长度了，说明所有的账户已经检测完了
#             if start == len(debit):
#                 return 0
    
#             r = float('inf')
#             #否则就开始遍历之后的账户，如果当前账户和之前账户的钱数正负不同的话，将前一个账户的钱数加到当前账户上
#             for i in range(start+1, len(debit)):
#                 # cannot be = 0 because debit[i] maybe 0
#                 if debit[start] * debit[i] < 0: # the s and i-th debit has opposite sign
#                     # 将前一个账户的钱数加到当前账户上
#                     debit[i] += debit[start]
                
#                     r = min(r, 1 + dfs(start+1))
                    
#                     # backtracking
#                     debit[i] -= debit[start]
                
#             return r
        
#         return dfs(0)

        
```

