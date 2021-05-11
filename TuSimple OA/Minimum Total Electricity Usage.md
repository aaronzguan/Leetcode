## Minimum Total Electricity Usage

TuSimple has **N** machine to running models right now, and we know **P** greater relationship between these machines (we assume greater at minimum 1)/

Besides, we know the minimum usage **M** of one machine currently, could you estimate the total usage of all the machines?

If the relationship contradicts with each other, output -1

The machine number is 1 ~ N

**Example:**

```
N=10, M=1, P=2
and if we have following P greater relationships
usage[1] < usage[2]
usage[1] < usage[3]
final minimum usage = [1, 2, 2, 1, 1, 1, 1, 1, 1, 1]
sum: 12
```



### BFS - Course Schedule

对greater relationship建立有向图，查看是否存在环，若存在环则不成立，输出-1，若有向图是valid，则寻找入度为0的machine并且他们的usage都为M，按图依次增加usage即可．要注意一些复杂情况：

A->B<-C<-D, 这种情况下B应该增加两次，找最长的edge

```python
def minimum_total_usage(N, M, relationships):
    graph = collections.defaultdict(list)
    machine_indegree = {x: 0 for x in range(1, N + 1)}
    for edge in relationships:
        graph[edge[0]].append(edge[1])
        machine_indegree[edge[1]] += 1
    
    # 得到入度为0的机器，即不存在任何greater relationship
    start_machine = [(m, M) for m in range(1, N + 1) if machine_indegree[m] == 0]
    
    if start_machine == []:
        return -1
    
    result = 0
    nums_machine = 0
    # 用steps来记录edge的长度
    steps = {m: 0 for m in range(1, N + 1)}
    queue = collections.deque(start_machine)
    while queue:
        machine, usage = queue.popleft()
        result += usage
        nums_machine += 1
        
        if nums_machine == N:
            return result
        
        for neighbor in graph[machine]:
            machine_indegree[neighbor] -= 1
            steps[neighbor] = max(steps[neighbor], steps[machine] + 1)
            if machine_indegree[neighbor] == 0:
                queue.append((neighbor, M + steps[neighbor]))
                
    return -1
            
    
```

