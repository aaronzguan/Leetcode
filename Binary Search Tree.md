## Binary Search Tree
The left subtree of a node contains only nodes with keys lesser than the node’s key. 左子树都比根节点小
The right subtree of a node contains only nodes with keys greater than the node’s key. 右子树都不小于根节点
The left and right subtree each must also be a binary search tree.



中序遍历 in-order traversal 是**不下降**序列

如果一颗二叉树的中序遍历不是**不下降**序列，则一定不是BST

如果一颗二叉树的中序遍历是不下降，也未必是BST，例如 [1 # 1, 1]



