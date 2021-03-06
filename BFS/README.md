### BFS模板

时间空间复杂度均为 O(|E|+|V|), E为节点的数量，V为Edge的数量

**不分层：** 连通块（[200. Num of Islands](../DFS/200.%20Number%20of%20islands%20(M).md)） [547. Number of Provinces (M).md](547.%20Number%20of Provinces%20(M).md) ，拓扑排序 [拓扑排序（Topological Sort）](拓扑排序（Topological Sort）) 

```python
def find_nodes_by_bfs(self, node):
    queue = collections.deque([node])
    visited = set([node])
    while queue:
        current_node = queue.popleft()
        for neighbor in current_node.neighbors:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return list(visited)
```

**分层：** 最短路径（Shortest distance）

* 需要多一层循环 `for _ in range(len(queue))`
* 或者使用**distance，哈希表**记录到所有点的距离

```python
def find_nodes_by_bfs(self, node):
    queue = collections.deque([node])
    visited = set([node])
    while queue:
        for _ in range(len(queue)) # 分层，加多一个循环，先遍历完当前层级，再遍历下面的层级
            current_node = queue.popleft()
            for neighbor in current_node.neighbors:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
    return list(visited)
```

