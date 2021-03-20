## Topological Sort - 拓扑排序

**入度（In-degree）:**

有向图（Directed Graph）中指向当前节点的点的个数（或指向当前节点的边的个数）

1. 统计每个点的入度
2. 将每个入度为0的点放入队列（Queue）中作为起始节点
3. 不断从队列中拿出一个点，去掉这个点的所有连边（指向其他点的边），其他点的相应入度 -1
4. 一旦发现新的入度为0的点，丢回队列中

```python
def topSort(self, graph):
    node_to_indegree = self.get_indegree(graph)
    # 将每个入度为0的点放入队列作为起始节点
    start_nodes = [n for n in graph if node_to_indegree[n] == 0]
    queue = collections.deque(start_nodes)
    topo_order = []
    # bfs
    while queue:
        #每次从队列中拿出一个点
        node = queue.popleft()
        topo_order.append(node)
        for neighbor in node.neighbors:
            # 将该点指向的所有点的入度减1， 即 node -> neighbor, 拿出node后，neighbor的入度减1
            node_to_indegree[neighbor] -= 1
            # 当点的入度为0时，加入topological order里面
            if node_to_indegree[neighbor] == 0:
                queue.append(neighbor)
    return topo_order

def get_indegree(self, graph):
    """
    计算该图中每个点的入度，rtype: dict
    """
    node_to_indegree = {x: 0 for x in graph}
    
    for node in graph:
        # 若Node有neighbor,则该Neighbor的入度加1， 即 node -> neighbor
        for neighbor in node.neighbors:
            node_to_indegree[neighbor] += 1
    return node_to_indegree
```



**判断是否存在拓扑排序：所有节点均能从图中被删除进入拓扑序** （没有循环依赖就有拓扑序）：Course Schedule

**当构建graph时，通常用dict来表示graph，key通常为前节点value则为后节点（即key指向value），并对后节点value的入度进行加1**

