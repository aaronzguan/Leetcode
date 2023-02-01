# Union Find

并查集是一种可以动态维护若干个不重叠的集合，并支持**合并**与**查询**两种操作的一种数据结构

一般我们建立一个数组fa 或者用map表示一个并查集，fa[i]表示i的父节点。

- 初始化：每一个点都是一个集合，因此自己的父节点就是自己fa[i]=i
- 查询：每一个节点不断寻找自己的父节点，若此时自己的父节点就是自己，那么该点为集合的根结点，返回该点。
- 修改**：**合并两个集合只需要合并两个集合的根结点，即fa[RootA]=RootB，其中RootA,RootB是两个元素的根结点。
- 路径压缩：实际上，我们在查询过程中只关心根结点是什么，并不关心这棵树的形态(有一些题除外)。因此我们可以**在查询操作**的时候将访问过的每个点都指向树根，这样的方法叫做路径压缩，单次操作复杂度为O(logn),经过路径压缩后可将查询时间优化到O(1)

凡是这种动态的加入，查询某个集合的问题都可以考虑并查集



As suggested by its name, a typical Union-Find data structure usually provides two interfaces as follows:

- `find(a)`: this function returns the group that the individual `a` belongs to.
- `union(a, b)`: this function merges the two groups that the individuals `a` and `b` belong to respectively, if the groups are not of the same group already.

To make the `union(a, b)` function more useful, one can return a boolean value in the function to indicate whether the merging actually happens or not. For example, `union(a, b)` would return true when `a` and `b` (and their respective groups) are merged together, and false when `a` and `b` are already in the same group and thus do not need to be merged together.



```python
# Initialization: Every node's father is itself at beginning
self.father = {i:i for i in range(n)}
def find(self, node):
    """
    Use recursion to find the father of node
    """
    if self.father[node] != node:
        self.father[node] = self.find(self.father[node])
        return self.father[node]
    
def union(self, a, b):
    fa, fb = self.find(a), self.find(b)
    if fa != fb:
        self.father[fb] = fa
        return True
    else:
        return False
```

