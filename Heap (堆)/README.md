# Heap 堆

支持操作：

* **Add or Pop** : O(log N) 
* **Min or Max:** O(1), Just return the top element
* **用heapify构建heap:** O(N) (因为用了siftdown，时间复杂度为O(N), 如果用siftup,　时间复杂度为O(NlogN))




堆是一棵满足如下性质的二叉树：
1、父节点的键值总是不大于它的孩子节点的键值（小顶堆）。
2、父节点的键值总是不小于它的孩子节点的键值（大顶堆）。
由于堆是一棵形态规则的二叉树，因此堆的父节点和孩子节点存在如下关系（根节点编号为0）：
**设父节点的编号为 i, 则其左孩子节点的编号为2*i+1, 右孩子节点的编号为2*i+2**
**设孩子节点的编号为i, 则其父节点的编号为(i-1)/2**

由于上面的性质，父节点一定比他的儿节点小（大），所以整个树的树根的值一定是最小（最大）的，那么我们就能在O(1)的时间内，获得整个堆的极值。
现在有没有发现堆和我们曾经遇到过的优先队列具有很相似的特点，那么二者究竟有何联系呢？

优先队列是一种抽象的数据类型，它和堆的关系类似于，List和数组、链表的关系一样；我们常常使用堆来实现优先队列，因此很多时候堆和优先队列都很相似，它们只是概念上的区分。

优先队列的应用场景十分的广泛，常见的应用有：

- Dijkstra’s algorithm（单源最短路问题中需要在邻接表中找到某一点的最短邻接边，这可以将复杂度降低。）
- Huffman coding（贪心算法的一个典型例子，采用优先队列构建最优的前缀编码树(prefixEncodeTree)）
- Prim’s algorithm for minimum spanning tree

在java，python中都已经有封装了的Priority Queue(Heaps)
优先队列是一个至少能够提供插入（Insert）和删除最小（DeleteMin）这两种操作的数据结构。对应于队列的操作，Insert相当于Enqueue，DeleteMin相当于Dequeue。
用堆实现优先的过程中，需要注意最大堆只能对应最大优先队列，最小堆则是对应最小优先队列。

现在我们借助下面的问题，来理解**siftup**和**siftdown**的思想。



## 利用Siftup实现堆化 - 时间复杂度O(NlogN)

给定一个数组A，我们的目的是要将 A 堆化 (Min-Heap)，也就是让A 满足以下要求：

- `A[i * 2 + 1] >= A[i]`
- `A[i * 2 + 2] >= A[i]`

```python
import sys
import collections
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftup(self, A, k):
        while k != 0:
            father = (k - 1) // 2
            if A[k] > A[father]:
                break
            A[father], A[k] = A[k], A[father]
            k = father
            
    def heapify(self, A):
        for i in range(len(A)):
            self.siftup(A, i)
```

算法思路：
对于每个元素`A[i]`，比较`A[i]`和它的父亲结点 `A[i - 1 // 2]` 的大小，如果小于父亲结点，则与父亲结点交换。
交换后再和新的父亲比较，重复上述操作，直至该点的值大于父亲。

时间复杂度分析：
对于每个元素都要遍历一遍，这部分是 O(N)
每处理一个元素时，最多需要向根部方向，及向最上方root交换 logN次 (树高为logN)。
因此总的时间复杂度是 **O(NlogN)**



## 利用Siftdown实现堆化 - 时间复杂度O(N)

除了上面的代码，我们也可以使用更有效率的O(n)的算法。
基于 Siftdown 的版本 O(n)

```python
import sys
import collections
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftdown(self, A, k):
        while k * 2 + 1 < len(A):
            son = k * 2 + 1    #A[i]左儿子的下标
            if k * 2 + 2 < len(A) and A[son] > A[k * 2 + 2]:
                son = k * 2 + 2    #选择两个儿子中较小的一个
            if A[son] >= A[k]:
                break
                
            A[son], A[k] = A[k], A[son]
            k = son
    
    def heapify(self, A):
        for i in range(len(A) - 1, -1, -1):
            self.siftdown(A, i)
```

**算法思路：**
初始选择最接近叶子的一个父结点，与其两个儿子中较小的一个比较，若大于儿子，则与儿子交换。
交换后再与新的儿子比较并交换，直至没有儿子。
再选择较浅深度的父亲结点，重复上述步骤。

**时间复杂度分析**
这个版本的算法，乍一看也是 O(nlogn)， 但是我们仔细分析一下，算法从第 n/2 个数开始，倒过来进行 siftdown。也就是说，相当于从 heap 的倒数第二层开始进行 siftdown 操作，倒数第二层的节点大约有 n/4 个， 这 n/4 个数，最多 siftdown 1次就到底了，所以这一层的时间复杂度耗费是 O(n/4)，然后倒数第三层差不多 n/8 个点，最多 siftdown 2次就到底了。所以这里的耗费是 O(n/8 * 2), 倒数第4层是 O(n/16 * 3)，倒数第5层是 O(n/32 * 4) ... 因此累加所有的时间复杂度耗费为：
T(n) = O(n/4) + O(n/8 * 2) + O(n/16 * 3) ...
然后我们用 2T - T 得到：
2 * T(n) = O(n/2) + O(n/4 * 2) + O(n/8 * 3) + O(n/16 * 4) ...
T(n) = O(n/4) + O(n/8 * 2) + O(n/16 * 3) ...

2 * T(n) - T(n) = O(n/2) +O (n/4) + O(n/8) + ...
= O(n/2 + n/4 + n/8 + ... )
= O(n)
因此得到 T(n) = 2 * T(n) - T(n) = **O(n)**

This is due to the fact that when you use `siftDown`, the time taken by each call **decreases** with the depth of the node because these nodes are closer to the leaves. When you use `siftUp`, the number of swaps **increases** with the depth of the node because if you are at full depth, you may have to swap all the way to the root. As the number of nodes grows exponentially with the depth of the tree, using `siftUp` gives a more expensive algorithm.

## Heap的实现

```python
class Heap(object):
	def __init__(self):
		self.heap = []

	def _get_parent(self, idx_child):
		idx_parent = int((idx_child - 1) // 2)
		return idx_parent

	def _get_children(self, idx_parent):
		idx_childrens = [2*idx_parent + 1, 2*idx_parent + 2]
		return idx_childrens

	def _siftup(self, idx):
		"""
		Compare the parent node and the idx
		If the parent node is larger, swap them and continue to do siftup
		小的值放父节点上
		Time Complexity: O(logN)
		"""
		idx_parent = self._get_parent(idx)
		if idx_parent >= 0 and self.heap[idx_parent] > self.heap[idx]:
			self.heap[idx_parent], self.heap[idx] = self.heap[idx], self.heap[idx_parent]
			self._siftup(idx_parent)

	def _siftdown(self, idx):
		"""
		Compare the children nodes and the idx
		If children is smaller, swap them and then continue to do siftdown
		小的放父节点上
		Time Complexity: O(logN)
		"""
		idx_childrens = self._get_children(idx)
		for idx_child in idx_childrens:
			if idx_child < len(self.heap) and self.heap[idx_child] < self.heap[idx]:
				self.heap[idx_child], self.heap[idx] = self.heap[idx], self.heap[idx_child]
				self._siftdown(idx_child)

	def _heapify(self):
		"""
		Do heepifying starting from the bottom.
		Time Complexity: O(N)
		"""
		for i in range(len(self.heap) - 1, -1, -1):
			self._siftdown(i)

	def _inv_heapify(self):
		"""
		Do heepifying starting from the root.
		Time Complexity: O(NlogN)
		"""
		for i in range(len(self.heap)):
			self._siftup(i)

	def pop(self):
		"""
		Remove the smallest element from the heap
		Firstly, swtich the head element with the last element
		Then we can do pop to remove the last element which is the smallest
		Then we siftdown the head element
		Time Complexity: same as siftdown - O(logN)
		"""
		if len(self.heap) == 0:
			print("Error: Heap is empty!")
		else:
			self.heap[0], self.heap[-1] = self.heap[-1], self.heap[0]
			heapMin = self.heap.pop()
			self._siftdown(0)
			return heapMin

	def add(self, element):
        """
        Append the element in the heap and then do siftup for the element
        Time Compleixty: Same as siftup - O(logN)
        """
        self.heap.append(element)
        idx_node = len(self.heap) - 1
        self._siftup(idx_node)

	def min(self):
		if not self.heap:
			return None
		return self.heap[0]
```

