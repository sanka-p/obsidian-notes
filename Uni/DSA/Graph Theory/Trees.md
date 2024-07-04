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
RecursiveSearchBST(node, key)
	if node == null return null
	else if node == key return node
	else if key < node BinarySearchBST(node.left, key)
	else BinarySearchBST(node.right, key)
```
iterative
```
InterativeSearchBST(node, key)
	current_node = node
	while current_node != null
	  if key == node
	    return node
	  else if key < current_node
	    current_node = current_node.left
	  else
	    current_node = current_node.right
```

Insertion
```
AddBST(root, value)
  if root == null
    root <= new node
    root->value = value
  else if value <root->key
    AddBST(root->left, value)
  else
    AddBST(root->right, value)
```
Order of insertion of elements into a BST matters
![[Pasted image 20240213182505.png]]
To avoid the imbalance use self balancing trees such as
For a BST with n nodes
Average height is $O(logn)$
Worst Case is $O(n)$
Hence come up with a balance condition that preserves O(logn) height For example:
1. left and right subtrees of the root have equal number of nodes
2. left and right subtrees of the root have equal height
3. left and right subtrees of every node have equal number of nodes
4. left and right subtrees of every node have equal height

Check if the tree is balanced in O(n) time:
```cpp
    bool isBalanced(TreeNode* root) {
        return dfsHeight (root) != -1;
    }

    int dfsHeight (TreeNode *root) {

        if (root == NULL) return 0;
        
        int leftHeight = dfsHeight (root -> left);

        if (leftHeight == -1) 
            return -1;
        
        int rightHeight = dfsHeight (root -> right);

        if (rightHeight == -1) 
            return -1;
        
        if (abs(leftHeight - rightHeight) > 1)  
            return -1;

        return max (leftHeight, rightHeight) + 1;
    }
};
```
###### AVL trees
The balance property is left and right trees of every node have heights differing by at most 1
for each node we calculate a balance factor:
balance factor (bf) = height of left subtree - height of right subtree = {-1, 0, 1}

if |bf| > 1 then do rotation on the 3 nodes

AVL find - same as BST find
AVL insert - same as BST insert but may need to fix the tree after insert. Let x be the node where the imabalance occurs:
Four cases to consider. The insertion is in the
1. left subtree of the left child of x
![[Pasted image 20240213195832.png]]
![[Pasted image 20240213200350.png]]
2. right subtree of the left child of x
![[Pasted image 20240213200001.png]]
![[Pasted image 20240213200620.png]]
3. left subtree of the right child of x
![[Pasted image 20240213200108.png]]
4. right subtree of the right child of x
![[Pasted image 20240213200219.png]]
cases 1&4 are solved by a single rotation
cases 2&3 are solved by a double rotation

**if multiple nodes gets imbalanced** perform rotation on the first imbalanced ancestor of the inserted node

**to determine the imbalance type (LL/LR/RL/RR)** start from the imbalanced node and move two steps towards the newly inserted node
![[Pasted image 20240213201035.png]]
###### Red Black trees


Deletion
if leaf remove it;
if one child, delete node and replace with subtree (since all subtrees a BSTs the BST rules will be preserved)
if two children, two options:
- replace with minimum from right subtreethis minimum value will either be a 
	- leaf - delete it 
	- node with one parent - replace it
Why the minimum from right?
- It will be greater than the value im trying to replace so it will be greater than anything on the left subtree
- Since it is the minimum from the right it will smaller than everything on the right subtree
Rules are preserved!!
- replace with max in left subtree
```
DeleteNode(root, value)
// should return the node since it might change
  if root == null return root
  else if(value < root->value) root.left  = DeleteNode(root.left)
  else root.right = DeleteNode(root.right)

  // case 1: no child
  if (root.left == null && root.right == null)
	delete root
	root = null
  // case 2.1: one child (left)
  else if (root->left == null)
	  root <= root->right
  // case 2.1: one child (left)
  else if (root->right == null)
	  root <= root->left
	  
  // case 3: two children (method 1)
  else
	  temp = FindMin(root->right)
	  root->data = temp->data
	  root->right = Delete(root->right, temp->data) 
  return root
  
```
https://gist.github.com/mycodeschool/9465a188248b624afdbf
