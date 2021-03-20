## Sequence Reconstruction - Topological Sort 判断拓扑序是否唯一

判断是否序列 `org` 能**唯一**地由 `seqs`重构得出. `org`是一个由从1到n的正整数排列而成的序列。 重构表示组合成`seqs`的一个最短的父序列 (意思是，一个最短的序列使得所有 `seqs`里的序列都是它的子序列). 判断是否有且仅有一个能从 `seqs`重构出来的序列，并且这个序列是`org`。

```
输入:org = [1,2,3], seqs = [[1,2],[1,3]]
输出: false
解释: [1,2,3] 并不是唯一可以被重构出的序列，还可以重构出 [1,3,2]

输入: org = [1,2,3], seqs = [[1,2]]
输出: false
解释: 能重构出的序列只有 [1,2].

输入: org = [1,2,3], seqs = [[1,2],[1,3],[2,3]]
输出: true
解释: 序列 [1,2], [1,3], 和 [2,3] 可以唯一重构出 [1,2,3].

输入:org = [4,1,5,2,6,3], seqs = [[5,2,6,3],[4,1,5,2]]
输出:true
```

```python
class Solution:
    """
    @param org: a permutation of the integers from 1 to n
    @param seqs: a list of sequences
    @return: true if it can be reconstructed only one or false
    """
    def sequenceReconstruction(self, org, seqs):
        # Construct the graph
        # 1. 对每个数在图中都建立一个节点，graph = {x:set() for x in nodes}
        graph = {}
        for seq in seqs:
            for node in seq:
                graph[node] = set()
        # 2. 对每个节点建立边的关系，即graph[key].append(neighbor)
        for seq in seqs:
            for i in range(1, len(seq)):
                graph[seq[i - 1]].add(seq[i]) # graph: dict{label:neighbor}, label -> neighbor or [label, neighbor]
        
        # 3. 对每个节点都建立入度关系
        indegrees = {node: 0 for node in graph}
        for node in graph:
            for neighbor in graph[node]:
                indegrees[neighbor] += 1
                
        # 4. 将每个入度为0的点放入队列（Queue）中作为起始节点
        start_num = [x for x in graph if indegrees[x] == 0]
        queue = collections.deque(start_num)
        topo_order = []

        while queue:
            if len(queue) > 1:
                # there must exist more than one topo orders
                return False
            num = queue.popleft()
            topo_order.append(num)
            for neighbor in graph[num]:
                indegrees[neighbor] -= 1 
                if indegrees[neighbor] == 0:
                    queue.append(neighbor)

        return topo_order == org
```

