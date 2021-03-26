# Minimum Spanning Tree

We need to find a subset of edges which **connects all the nodes of the graph with the minimum possible total weight**. This is by definition the Minimum Spanning Tree or [MST](https://en.wikipedia.org/wiki/Minimum_spanning_tree) of a graph. There are two main algorithms to solve Minimum Spanning Tree problem:

* **Prim's Algorithm**: It typically uses Heap
* **Kruskal's Algorithm**：  It is implemented using the [Disjoint set](https://en.wikipedia.org/wiki/Disjoint-set_data_structure) Union-Find data structure.



## Prim's Algorithm:

A greedy algorithm, building the tree by adding one vertex each step.

- Data structure: Heap
- Time complexity: O(E logV)

**Overall Procedure:**

1. 首先选择任意一个节点作为一个单节点的树。它就相当于是一棵树的根或者发起点。
2. 我们从这个节点开始，看它关联的所有边。
3. 每次我们选择一个边的时候，挑选一个权值最小的而且不在这个树的节点集合里的。
4. 我们增加一个边的时候，同时就把这个边所关联的另外一个节点加入到前面的树的节点集合里来了。

```pseudocode
S={}
Q=G.V
while Q!={}:
	u=Extract_min(Q)
	S=SU{u}
	for each vertex v connect with u:
		Update(v)
```



## Kruskal Algorithm:

Also a greedy algorithm, building the tree by merging minimum spanning forest. (Union-find)

- Time Complexity: O(E logV)

**Overall Procedure:**

因为要找的 Minimum Spanning Tree 是要求涵盖所有节点并且权值最小，那么我们可以从权值最小的边这个角度入手 -> **每次从树中选择长度最短的边**。

每次我们都选择权值最小的边，再考察它所关联的节点。假定我们图里的每个节点都是一个个独立的集合，它们每个集合就是包含它们本身。而一旦选择了一个边，相当于两个集合直接建立了一个关联，或者说将它们合并了。比如最开始的时候，我们找到第一个边，那么它就将两个独立的节点合并成一个集合。然后再去找下一个权值最小的边。

**所以一定是要对egdes先排序， 每次都从成本最小的开始选， 如果不会够成环，就加到结果里面去，**

```pseudocode
设G={V,E}

A={}
sort edges by weight
for edge ei=(vi,ui) in E:
	if Find-set(vi) != Find-set(ui):
		A = AU{ei}
		set(vi)= set(ui)
```

