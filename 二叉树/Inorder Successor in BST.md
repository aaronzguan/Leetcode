# Inorder Successor in BST 总结

寻找Inorder Successor in BST,　即找到在中序遍历下的该点的下一个点即可．

1. 如果该节点有右子节点, 那么successor是其右子节点的子树中最左端的节点
2. 如果该节点没有右子节点, 那么successor是离它最近的祖先, 该节点在这个祖先的左子树内.

## 285. Inorder Successor in BST (M)

Given the `root` of a binary search tree and a node `p` in it, return *the in-order successor of that node in the BST*. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

**Example**

![img](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

```
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer is null.
```

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def inorderSuccessor(self, root, p):
        """
        :type root: TreeNode
        :type p: TreeNode
        :rtype: TreeNode
        """
        if root == None:
            return None
        # 该节点属于右子树，向右子树找successor
        if root.val <= p.val:
            return self.inorderSuccessor(root.right, p)
        #　该节点小于root,那么向左子树找successor
        left = self.inorderSuccessor(root.left, p)
        if left != None:
            return left
        else:
            return root
```



## 510. Inorder Successor in BST II (M)

Given a `node` in a binary search tree, return *the in-order successor of that node in the BST*. If that node has no in-order successor, return `null`.

The successor of a `node` is the node with the smallest key greater than `node.val`.

**You will have direct access to the node but not to the root of the tree**. Each node will have a reference to its parent node. Below is the definition for `Node`:

```
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}
```



此题与上一题不同的是，我们只有Node这一个输入，而没有了root的输入，所以只能通过找node.parent来搜索祖先．

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None
"""

class Solution(object):
    def inorderSuccessor(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        # 有右子节点, 找其右子节点的子树中最左端的节点
        if node.right:
            node = node.right
            while node.left:
                node = node.left
            return node
        
        #　没有右子树，那么找离该节点最近的祖先，该节点在这个祖先的左子树内.
        while node.parent and node == node.parent.right:
            node = node.parent
        return node.parent
```

