### Terminology
- Root - Node without parent
- Siblings - Nodes that share a common parent
- Internal Node - Node with at least one child
- External Node (Leaf) - Nodes without children
- Ancestors - Parent, grandparent, ...etc
- Descendant - Child, grandchild,...etc
- Depth (of a node) - No. of ancestors
- Height (of the tree) - Maximum depth of any node
- Degree of a node - Number of children
- Degree of a tree - Maximum number of its node
- Subtree - Tree consisting of a node and its descendants

Two ways to traverse a tree
- Preorder - visit the root and traverse the children
	```
	preorder(node)
	  if node == null then return
	  visit(node)
	  preorder(node.left)
	  preorder(node.right)
	  // if it is not a binary tree
	  // for each child of node:
	  //  preorder(child)
	```
- Postorder - traverse the children then visit the root
	```
	postorder(node)
	  if node == null then return
	  postorder(node.left)
	  postorder(node.right)
	  // if it is not a binary tree
	  // for each child of node:
	  //  postorder(child)
	  visit(node)
	```
Time complexity for post and pre order traversal: O(n) - each node is visited once
- Inorder - Only applicable for binary trees
```
	inorder(node)
	  if node == null then return
	  inorder(node.left)
	  visit(node)
	  inorder(node.right)
```
### Binary Tree
- Each internal node has at most two children
- Children are an ordered pair
- The maximum number of nodes **at** depth $i$ of a binary tree is $2^i$ where $i \ge 0$
- The maximum number of nodes **in** a binary tree of height $k$ is $2^{k+1}-1$ where $k \ge 0$ 
		Proof by induction
		when $k = 0$ $2^{0+1}-1$ = 1 (the root node)
		Assume its true for $k = p$
		Then when $k=p+1$ total nodes at that depth is $2^{p+1}$
		therefore the total nodes = $2^{p+1}-1 + 2^{p+1} = 2^{p+2}-1$
		which satisfies the the expression

Node number properties
![[Pasted image 20240211013404.png]]
- Label by levels from top to bottom
- Within a level label from left to right
- Parent node of i is i/2 unless i = 1 (since it is the root, it has no parent)
- Left child of node i is node 2i unless 2i>n where that means i has no left child here n is the number of nodes
- Right child of a node i is 2i+1 unless 2i+1>n where that means i has no right child

**Complete Binary Tree** - A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible
**Full Binary Tree** is a binary tree in which all of the nodes have either 0 or 2 offspring. In other terms, a full binary tree is a binary tree in which all nodes, except the leaf nodes, have two offspring.

#### Applications of Binary Trees
##### Arithmetic Expression Tree
- Design
	- Internal nodes are operators
	- External nodes are operands
- Traversal
	- Inorder traversal gives the infix version of the expression
		```
		inorder(node.left)
		  print("(")
		  inorder(node.left)
		  print(node)
		  inorder(node.right)
		  print(")")
		```
	![[Pasted image 20240213180421.png]]
	- Postorder traversal gives the postfix version of the expression
- Construction
	- If a character is an operand push that into the stack
	- If a character is an operator pop two values from the stack make them its child and push the current node again.
	- In the end, the only element of the stack will be the root of an expression tree.

##### Decision Tree
- Internal nodes: yes/no type questions
- External nodes: answers

##### Binary Search Tree
Binary tree with
- All items in the left subtree are less than the root
- All items in the right subtree are greater or equal to the root
- Each subtree is itself a binary search tree
Average depth of a node is $O(logn)$
Maximum depth of a node is $O(n)$
Inorder traversal of a binary search tree produces a sequenced list
![[Pasted image 20240213180833.png]]
Right-Node-Left traversal creates a descending sequence
How to copy a binary tree
How to delete a binary tree

To find the smallest node - go to the left most node
```
FindSmallestBST(node)
	if node == null return null
	if node.left == null return node
	return FindSmallestBST(node.left)
```
To find the largest node - go to the right most node
```
FindLargestBST(node)
	if node == null return null
	if node.right == null return node
	return FindLargestBST(node.right)
```
To find requested node - binary search
recursive
```
BinarySearchBST(node, key)
	if node == null return null
	else if node == key return node
	else if key < node BinarySearchBST(node.left, key)
	else BinarySearchBST(node.right, key)
```
iterative
