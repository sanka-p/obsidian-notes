Root - Node without parent
Siblings - Nodes that share a common parent
Internal Node - Node with at least one child
External Node (Leaf) - Nodes without children
Ancestors - Parent, grandparent, ...etc
Descendant - Child, grandchild,...etc
Depth - No. of ancestors
Height - Maximum depth of any node
Degree of a node - Number of children
Degree of a tree - Maximum number of its node
Subtree - Tree consistinf of a node and its descendants

Two ways to traverse a tree
- Preorder - visit the root and traverse the children
	```
	PreOrder(v)
		visit(v)
		for each child w of v
			PreOrder(w)
	```
- Postorder - traverse the children then visit the root
	```
	PostOrder(v)
		for each child w of v
			PostOrder(w)
		visit(v)
	```

Binary Tree
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

Applications of Binary Trees
1. Arithmetic Expression Tree
		Internal nodes: operators
		External nodes: operands
2. Decision Tree
		Internal nodes: yes/no type questions
		External nodes: answers

Node number properties
![[Pasted image 20240211013404.png]]
- Label by levels from top to bottom
- Within a level label from left to right
- Parent node of i is i/2 unless i = 1
- Left child of node i is node 2i unless 2i>n where that means i has no left child here n is the number of nodes
- Right child of a node i is 2i+1 unless 2i+1>n where that means i has no right child

Complete binary tree - A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible
A full binary tree is a binary tree in which all of the nodes have either 0 or 2 offspring. In other terms, a full binary tree is a binary tree in which all nodes, except the leaf nodes, have two offspring.