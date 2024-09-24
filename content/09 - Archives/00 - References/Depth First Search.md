---
time created: 2024-09-05 00:41
prerequisites:
  - "[[Recursion]]"
tags:
  - "#recursion"
  - "#divideAndConquer"
references:
---
---
## An Overview

**DFS is a bold search.** 
We go as deep as we can to look for a value, and when there is nothing new to discover, we retrace our steps to find something new. 

To put it in a term we already know, the pre-order traversal of a tree is DFS. 
Let's look at a simple problem of searching for a node in a binary tree whose value equals *target*. 

```python
def dfs(root, target):
	if root is None:
		return None
	if root.val == target:
		return root
	# return non-null return value from recursive calls
	left = dfs(root.left, target)
	if left is not None:
		return left

	# at this point, we know left is null, and right could be 
	# null or non-null. 
	# we return right's recursive call directly because 
	# 1. if its not-null, we should return it
	# 2. if it is null, then we need to return null anyway.

	return dfs(root.right, target)
```

---
## When to use DFS
### 1. Tree
DFS is essentially pre-order tree traversal. 
- Traverse and find/create/modify/delete node. 
- Traverse with return value (finding max subtree, detect balanced tree). 

### 2. Combinatorial Problems
[[Depth First Search]] and combinatorial problems are a match made in heaven. As we will see in the [[Combinatorial Search]] module, combinatorial search problems boil down to searchign in trees. 
- How many ways are there to arrange something. 
- Find all possible combinations of something.
- Find all solutions to some puzzle.

### 3. Graph
Trees are special graphs that have no cycle. We can still use DFS in graphs with cycles. We just have to record the nodes we have visited and avoid re-visiting them and going in an infinite loop. 
- Find a path from point A to B. 
- Find connected components. 
- Detect cycles.
---
## Older Notes on DFS
Here is a generic approach to solve DFS related problems: 
- Choose a starting point
- Keep track of visited nodes to avoid cycles
- Perform required operation
- Explore unvisited neighbours until all nodes are visited
```python
def dfs_recursive(self, v. visited):
	visited.add(v)
	print(v, end=' ')

	for neighbour in self.grah[v]:
		if neighbour not in visited:
			self.dfs_recursive(neighbour, visited)
			
visited = set()
dfs_recursive(1, visited)
```
- [[Leetcode Practice]] - 133, 113, 210
