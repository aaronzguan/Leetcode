## Grouping Options (H) - Lintcode 1834

Given a number of people ***n*** and a number of groups ***k***, find the distinct options to form k contiguous groups out of the *n* people while respecting the following conditions:

* In each option, the total group sizes is equal to the number of people
* In each option, each group's size should be greater than or equal to the group to its left
* The groups formed in each option are distinct, meaning that they differ in at least one group. For example, [1, 1, 1, 3] is distinct from [1, 1, 1, 2] but not from [1, 3, 1, 1].

**Example**

```
people = 8
groups = 4
return 5
The 5 distinct options to form 4 groups with 8 people under the rules are [1, 1, 1, 5], [1, 1, 2, 4], [1, 1, 3, 3], [1, 2, 2, 3], and [2, 2, 2, 2]
```

### 动态规划

https://math.stackexchange.com/questions/1908701/integer-partition-of-n-into-k-parts-recurrence

https://blog.csdn.net/roufoo/article/details/104901535

If the smallest part of the partition is 1, we've added 1 to all partitions of `n - 1` into `k - 1` parts (to guarantee the smallest part is 1); and if the smallest part is not 1, we've added 1 to each of `k` parts in all partitions of `n - k` into `k` parts (guaranteeing that each part is greater than 1). Therefore, we can use dynamic programming.

*𝑝*(*𝑛*,*𝑘*) is the number of ways to partition *𝑛* into *𝑘* parts. It is the same as the number of different ways of placing *𝑛* objects into *𝑘* pots. Firstly put 1 object in each pot. Total *𝑘* objects have been put and now we have to put remaining *𝑛*−*𝑘* objects into *𝑘* pots. Hence,
$$
p(n,k)=p(n-k,1)+p(n-k,2)+\cdots+p(n-k,k-1)+p(n-k,k)
$$
Also by substituting *n-1*, *k-1* for *n* and *k* respectively, we can observe that,
$$
p(n-1,k-1)=p(n-k,1)+p(n-k,2)+\cdots+p(n-k,k-1)
$$
From the above two equations, we conclude:
$$
p(n,k)=p(n-1,k-1)+p(n-k,k)
$$
Let `dp[i][j]` be the number of options to split j numbers into i groups. The solution would be `dp[k][n]`

 `dp[i][j] = dp[i-1][j-1] + dp[i][j-i]`

```python
def countOptions(people, groups):
    if people < groups:
        return 0
    
    dp = [[0] * (people + 1) for _ in range(groups + 1)]
    
    for i in range(1, groups + 1):
        for j in range(i, people + 1):
            if i == j:
                dp[i][j] = 1
            else:
                dp[i][j] = dp[i-1][j-1] + dp[i][j-i]
                
    return dp[-1][-1]
    
```

