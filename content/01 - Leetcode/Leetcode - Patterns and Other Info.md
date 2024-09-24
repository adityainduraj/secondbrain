---
time created: 2024-08-11 22:37
tags:
  - leetcode
  - interviews
  - coding
references:
  - "[[Leetcode Practice]]"
source: https://www.youtube.com/watch?v=DjYZk8nrXVY
addn source: https://blog.algomaster.io/p/15-leetcode-patterns
---
# Patterns:
##  1. [[Prefix Sum]]:
- Used in problems where we need to calculate sum of subarray
- We create a prefix-sum array where each element is the sum of elements leading up to that element
- Then we use the formula $SUM[i,j]=P[j]-P[i-1]$
-  Don't always need a new array for prefix sum, we can sometimes use the input array itself for better space complexity. Example:
```python
def create_prefix_sum(arr):
	for i in range(1, len(arr)):
		arr[i] += arr[i-1]
	return arr
```
- [[Leetcode Practice]] Problems to Practice - 303, 525, 560

## 2. [[Two Pointers]]:
-  Self-explanatory, imagine the 'isPalindrome' problem.
- [[Leetcode Practice]] Problems to Practice - 167, 15, 11

## 3. [[Sliding Window]]:
- Self-explanatory
- [[Leetcode Practice]] Problems to Practice - 643, 3, 76

## 4. [[Fast and Slow Pointers]]:
- Variant of two pointers approach, moving 2 pointers at different speeds
- Start by placing both slow and fast pointers at the head of a linked list. 
- Move the slow pointer **one** node at a time and the fast pointer **two** nodes at a time. 
- If there is a cycle, the two pointers will eventually meet
- This technique can also find the middle node of a linked list **in one pass**. When the fast pointer reaches the end, the slow pointer will be at the middle.
- [[Leetcode Practice]] - 141, 202, 287

## 5. [[Linked List In-Place Reversal]]:
- Consider the example of reversing a linked list
- Use 3 pointers, *previous*, *current* and *next* 
- Set *previous* to null, set *current* to head of the linked list
- Use a while loop to go through the list till *current* is null
- Set *next* to node after *current* 
- Then reverse pointer so that *current* points to *previous*. Then move *previous* to *current* and *current* to *next*. 
- Example Code:
```python
def reverse_linked_list(head):
	prev = None
	current = head

	while current is not None:
		next = current.next
		current.next = prev
		prev = current
		current = next

	return prev ## head of reversed linked list
```
- [[Leetcode Practice]] - 206, 92, 24


## 6. [[Monotonic Stack]]:
- Uses a stack to find the next greater or next smaller element in an array
- Use stack to track indices of elements for which we have not found next greatest element
- Initialize the result array with -1s
- For each element, check if its greater than the element at the top of the stack. 
- If it is, pop that element and push the index of new greater element onto the stack. 
- If it is not, then just push index of new element onto the stack
```python
def next_greater_element(arr):
	n = len(arr)
	stack = []
	result = [-1] * n

	for i in range(n):
		while stack and arr[i] > arr[stack[-1]]:
			result[stack.pop()] = arr[i]
		stack.append(i)

	return result
```
- [[Leetcode Practice]] - 496, 739, 84

## 7. [[Top K Elements]]:
- K largest, smallest or most frequent elements in an array
- Avoid sorting
- Example - To find 3 largest elements in an array. 
- We add first 3 elements to minheap. Then if nextelement is greater, pop the top element from the heap and push the new element. 
- At the end, the heap will contain 3 largest elements
- For K-Largest use a minheap, for K-Smallest use a maxheap.
- [[Leetcode Practice]] -  215, 327, 373

## 8. [[Overlapping Intervals]]:
- Intervals or ranges that may overlap
- Merging intervals - merge all overlapping intervals into 1
- Interval intersection - find the intersection between two groups of intervals
- Insert interval - insert an interval into a group of intervals
- Problems like - *Finding a minimum number of meeting rooms for meetings with overlapping times*
- Example - Merging Intervals
	- First you sort the list of intervals by start time
	- Then check if it overlaps with next interval (if start time of second is less than end time of first)
	- If it overlaps, choose start of first and end of second as interval and add to empty list of intervals. 
	- If it doesn't overlap, add it normally. 
	- You will end up with a list of merged intervals. 
- [[Leetcode Practice]] - 56, 57, 435

## 9. [[Modified Binary Search Pattern]]:
- Used for problems like:
	- Searching in a nearly sorted array
	- Searching in a rotated sorting array
	- Searching in a list with unknown length
	- Searching in an array with duplicates
	- Finding the first or last occurence of an element
	- Finding the square root of a number
	- Finding a peak element
- Example - Searching in a Rotated Sorting Array
	- We use normal binary search with an additional check
	- We check which half is sorted, then check if element is in range of that sorted half, and then we search in that half
```python
def search_rotated_array(nums, target):
	left, right = 0, len(nums)-1

	while left <= right:
		mid = (left + right) // 2

		if nums[mid] == target:
			return mid
		if nums[left] <= nums[mid]:
			## this is the additional check
			if nums[left] <= target < nums[mid]:
				right = mid - 1
			else:
				left = mid + 1
		else:
			if nums[mid] < target <= nums[right]:
				left = mid + 1
			else:
				right = mid - 1

	return -1
```
- [[Leetcode Practice]] - 33, 153, 240

## 10. [[Binary Tree Traversals]]:
- Preorder, Postorder, Inorder and Level Order
- LOT is useful for level by level traversal starting from the root node, usually implemented using queue
- Goal is to identify which traversal will be best for the given situation. 
	- To retrieve the values of a binary search tree in sorted order - INORDER
	- To create a copy of a tree (serialization) - PREORDER
	- When you want to process child nodes before the parent, like in deletion - POSTORDER
	- When you need to explore all nodes at current level before moving on to the next - LOT
- [[Leetcode Practice]] for each traversal - 230, 257, 124, 107

## 11. [[Depth First Search]]:
- Used to explore all paths or branches
- Problems:
	- Finding path between 2 nodes
	- Checking if graph contains a cycle
	- Finding a topological order in a Directed Acyclic Graph
- Counting the number of connected components in a graph
- DFS can be implemented recursively, or iteratively using a stack
- Here is a generic approach to solve DFS related problems: 
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

## 12. [[Breadth First Search]]:
- Level by level graph traversal
- Problems:
	- Finding shortest path between 2 nodes in an unweighted graph
	- Printing all nodes of a tree level by level
	- Finding all connected components in a graph
	- Finding shortest transformation sequence from one word to another
```python
def bfs(self, start):
	visited = set()
	queue = deque([start])

	visited.add(start)

	while queue:
		vertex = queue.popleft()
		print(vertex, end = ' ')

		for neighbour in self.graph[vertex]:
			if neighbour not in visited:
				visited.add(neighbour)
				queue.append(neighbour)
```
- Approach Explained:
	- Add starting node to the queue
	- Set up a visited set, continue as long as the queue is not empty
	- Dequeue a node and process it
	- Enqueue unvisited neighbours
	-  Add processed nodes to the visited set
	- Continue till empty
- [[Leetcode Practice]] - 102, 994, 127

## 13. [[Matrix Traversal]]:
- Can use most graph algorithms for matrix problems as well
- Consider - Island Problem:
	- Grid of 1s and 0s. 0=water and 1=land. Group of 1s form an island
	- We check for 1s, then mark that as island. Then explore from there until we have explored everything using DFS
- [[Leetcode Practice]] -  733, 200, 130

## 14. [[Backtracking]]:
- Problems that involve exploring all potential solution paths and backtracking the paths that do not lead to a valid solution 
- Problems: 
	- Generate all possible permutations or combinations of a given set of elements
	- Solve puzzles like Sudoku or N-Queens Problem
	- Find all possible paths from start to end in a graph or a maze
	- Generate all valid parentheses of a given length
- Example - Generate all possible subsets
	- We start with an empty array. 
	- When we encounter the first element, we have a choice, we either include it in our solution or not. We take both paths and use recursion to achieve this for all elements
- [[Leetcode Practice]] - 46, 78, 51

## 15. [[Dynamic Programming]]:
- Used for solving optimization problems by breaking it down into subproblems and storing solutions to those subproblems to avoid repetitive calculation
- Problems:
	- Overlapping subproblems
	- Maximise or minimize a certain value
	- Count number of ways
- Common DP Patterns:
	- Fibonacci Numbers
	- 0/1 Knapsack
	- Longest Common Subsequence
	- Longest Increasing Subsequence
	- Subset Sum
	- Matrix Chain Multiplication
- [[Leetcode Practice]] - 70, 300, 322, 416, 1143, 312
- [20 Dynamic Programming Patterns for Leetcode](https://blog.algomaster.io/p/20-patterns-to-master-dynamic-programming)
