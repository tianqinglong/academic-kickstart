---
title: "Jousekis for Code Interview (1): Depth First Search (DFS)"
date: 2020-07-05T14:59:04-05:00
draft: false
tags: ["Code Jouseki", "Algorithm", "Interview"]
summary: "Using DFS to Solve Coding Problems"
---

## Introduction

Despite the daunting name, the DFS algorithm is straightforward: you go as deep as possible before going back. _Visualgo_ provides excelent visualizations of the DFS algorithm ([here](https://visualgo.net/en/dfsbfs)).

The purpose of the DFS algorithm is to __traverse__ the tree/graph. Here are a list of possible applicable questions:

1. Find the __connecting nodes__ that satisfy certain conditions.
2. Find the __paths__ that satisfy certain conditions.

## Jouseki

Recursive function is almost always used to implement the DFS algorithm. If the interviewers ask you not to use recursive function, well, you might want to use alternative algorithms.

The Python code for traversing a binary tree is given below and by changing the order of the ``print`` function, there are three different ways: inorder, preorder, and posorder. If you are confused with the name, just remember that the prefix indicates where the ``print`` function is.
```{python}
class TreeNode:
def __init__(self, val=0, left=None, right=None):
	self.val = val
	self.left = left
	self.right = right
    
def dfsInorder(root):
	if root:
		dfsInorder(root.left)
		print(root.val)
		dfsInorder(root.right)
  
def dfsPreorder(root):
	if root:
		print(root.val)
		dfsPreorder(root.left)
		dfsPreorder(root.right)

def dfsPosorder(root):
	if root:
		dfsPosorder(root)
		dfsPosorder(root)
		print(root.val)

root = TreeNode(1) 
root.left = TreeNode(2) 
root.right = TreeNode(3) 
root.left.left = TreeNode(4) 
root.left.right = TreeNode(5) 

# dfsInorder(root)
# dfsPreorder(root)
# dfsPosorder(root)
```

Using recursive function to implement DFS is clean and simple. However, the power of it will be unleashed when __extra information__ is passed through the many layers of the recursive function. In the next section, we will see the derivations of the DFS Jouseki and what does __extra information__ mean.

### Derivations

#### 111. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

This problem can be solved using DFS and in the recursive function. The recursive function has one input, which is ``node`` and the output of the __minimum depth to reach a leaf from the current node__.

To write a recursive function, the first step is to determine the base case. For this problem, the base case is that the current node is a leaf. If the current node is not a leaf, then there are three situations:

- Left only
- Right only
- Both

If both children exist, the output given current node is ``1+min(_minDepth(node.left), _minDepth(node.right))``. Of course we need to address the other situations.

```python
def _minDepth(root):
    if not root.left and not root.right:
        return 1
    _min = float('inf')
    if root.left:
        _min = min(_min, 1+_minDepth(root.left))
    if root.right:
        _min = min(_min, 1+_minDepth(root.right))
    return _min

def minDepth(self, root: TreeNode) -> int:
    if not root:
        return 0
    return _minDepth(root)
```

In this problem, the extra information is the minimum steps ahead to reach a leaf. Similarly, the following problem can be solved using the same method without effort.

104\. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)


#### 112. [Path Sum](https://leetcode.com/problems/path-sum/)

DFS is very handy tool to traverse all the paths in the tree. In this example, the output of the recursive function is ``True`` or``False``. 

Suppose we arrive at a leaf node, how do we know if it is ``True`` or ``False``? Then answer is we need to record the sum of the path. In order to remember the sum, the recursive function takes two variable, ``Node`` and ``target``.

For the base case, the node is a leaf, we will check if  ``target==0``. If so, we will return ``True``, otherwise ``False``. If we reach a ``None`` (and still haven't reach a leaf with ``target==0``), it is a dead end.

If a node is not a leaf, then we subtract the current value from ``target`` and then, again, we either go left or go right.

```{python}
def _hasPathSum(root, target):
    if not root:
        return False
    
    if not root.left and not root.right and target == root.val:
        return True
    
    return _hasPathSum(root.left, target-root.val) or _hasPathSum(root.right, target-root.val)

def hasPathSum(self, root: TreeNode, sum: int) -> bool:
    return _hasPathSum(root, sum)
```

Unlike the previous example, we need to include __an extra argument__ to record the status and pass through the recursive function.


#### 113\. [Path Sum II](https://leetcode.com/problems/path-sum-ii/)

This problem is more complicated than the previous one because we need return all the paths.

So, what else do we need?

1. A list to record the path we have already taken.
2. A list (of list) to record the paths.

The second one is easy to solve, we only need to use an additional list to record the paths that has the given sum.

For the first one, we need to be careful and should properly pass the list to next layer of recursive function. One easy solution is to copy the list and pass that copy to next layer but that may cost a lot of extra space.

```{python}
def _pathSum(root, target, curList, totalList):
    if not root:
        return
    if not root.left and not root.right and target==root.val:
        totalList.append(curList+[root.val])
    _pathSum(root.left, target-root.val, curList+[root.val], totalList)
    _pathSum(root.right, target-root.val, curList+[root.val], totalList)

def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
    curList = []
    totalList = []
    _pathSum(root, sum, curList, totalList)
    return totalList
```
This solution is acceptable and a lot of answers in the discussion section are using this approach. However, a better solution using __backtracking__ is available [here](https://www.educative.io/courses/grokking-the-coding-interview).

In this problem, we do not need the recursive function to return anything. The list ``totalList`` takes the role as the output of the recursive function. We only need to tell the function when to stop.

129\. [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

This problem is similar, the recursive function needs __two extra arguments__, one to record the number of the current path, the other records the total sum.

257\. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)


#### 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

This is an easy problem on Leetcode but it should be classified as, at least, medium.



-----

- I will keep updating this article and more examples will be added.

- I made my best effort writing this article and the code I provided have been accepted by Leetcode but there is __no__ guarantee regarding the correctness.