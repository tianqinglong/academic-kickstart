---
title: "Jousekis for Code Interview (1): Depth First Search (DFS)"
date: 2020-07-05T14:59:04-05:00
draft: true
tags: ["Code Jouseki", "Algorithm", "Interview"]
---

## Introduction

Desipte the daunting name, the DFS algorithm is straightforward: you go as deep as possible before going back. _VIsualgo_ provides excelent visualizations of the DFS algorithm ([here](https://visualgo.net/en/dfsbfs)).

The purpose of the DFS algorithm is to __traverse__ the tree/graph. Here are a list of possible applicable questions:

1. Find the __nodes__ that satisfy certain conditions.
2. Find the __strings of nodes__ that satisfy certain conditions.

## Jouseki

Recursive function is almost always used to implement the DFS algorithm. If the interviewers ask you not to use recursive function, well, you might want to use alternative algorithms.

The tree node class is defined below

```{python}
class TreeNode:
	def __init__(self, val=0, left=None, right=None):
  	self.val = val
    self.left = left
    self.right = right
```

Using the DFS algorithm, traverse the binary tree

```{python}

```

###### About

-----

This article is always under construction and more examples will be added.