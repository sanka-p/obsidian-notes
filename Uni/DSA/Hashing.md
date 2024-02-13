###### Overview
- Hash Tables
	- Direct Addressing
	- Hashing Function
		- Types
			- Key size known
				- Perfect hashing
			- Key size unknown
				- Division Method
				- Multiplication Method
		- Collision Resolution
			- Closed Addressing/Chaining
			- Open Addressing
				- Methods
					- Linear Probing
					- Quadratic Probing
					- Double Hashing
				- Clustering
					- Primary
					- Secondary

- Hash Table (Symbol Table/Associative Array/Map) is a collection of <key, value> pairs such that **each key appears at most once in the collection**
- We are interested in the search, insertion and deletion operations. A hashtable supports all these in O(1) time (general case)

|        | Average    | Worst |
|--------|------------|-------|
| Space  | O(n)       | O(n)  |
| Search | O(1 + n/m) | O(n)  |
| Insert | O(1)       | O(1)  |
| Delete | O(1 + n/m) | O(n)  |


## Direct Addressing
- The range of keys is 0...m-1
- Keys are distinct
- Set up an array T[0...m-1] in which
	- T[i] = x if x.key = i
	- T[i] = NULL elsewhere
- Works well when the range m of keys is relatively small otherwise use a **hash function**
- Issues when the key range is huge
	- At the start, the array entries may contain garbage, and initializing the entire array is impractical because of its size.
	- Instead on initializing it, we can have an additional stack (say S). Each object stored in the huge array will have two parts: the key value, and a pointer to an element of S, which contains a pointer back to the object in the huge array. To insert x, add an element y to the stack which contains a pointer to position x in the huge array. Update position A[x] in the huge array A to contain a pointer to y in S. To search for x, go to position x of A and go to the location stored there. If that location is an element of S which contains a pointer to A[x], then we know x is in A. Otherwise, x /∈ A. To delete x, delete the element of S which is pointed to by A[x]. Each of these takes O(1) and there are at most as many elements in S as there are valid elements in A
	- The set of keys actually stored may be so small relative to the number of keys in the range that most of the space allocated for T would be wasted



## Hash Functions
- The "ideal" hash functions (independent uniform hashing)
	- Produces an output $h(k)$ for $k$ such that it is randomly and independently chosen uniformly from the range {0, 1, ..., m-1}
		- Uniformly - each element is equally likely to hash into any of the slots
		- Where a given element hashes into is **independent** of where
	- Each subsequent call to $h$ with the same input $k$ yields the same output $h(k)$
	- The chance that any two distinct keys collide is at most 1/m
- Issue with the hash functions is that it has collisions where two keys map to the same smaller key. Solutions to this include
	- Chaining
	- Open Addressing
- Load factor is defined as the average number of keys (n) per slot (m) in the table
	$$\alpha = n/m$$
- Performance of collision resolution depends strongly on the table's load factors
- With a good hash function, the average lookup cost is nearly constant for Load factor = 0.0 ~ 0.7
Types of hash functions:
- Static Hashing - provide a single ûxed hash function that works well on any data
	- Division Method
	- Multiplication Method
- Random Hashing - provably good average-case performance for any data can be 
obtained by designing a suitable family of hash functions and choosing a hash function at random from this fa
### Division Method
Hash k into a table with m slots using the slot given by the remainder of k divided by m
$$h(k) = k\;mod\;m$$
m should be a prime number not too close to a power of 2 or 10.
- If a power of 2 is used (say $2^n$), then the n lowest significant bits are used to hash the key.  If these bits don't vary much for your key set, many keys will hash to the same value, leading to a high number of collisions.

## Multiplication Method
For a constant A, 0 < A < 1
$$h(k) = \lfloor m(kA\:mod\:1) \rfloor$$
$kA\:mod\:1$ is the fractional part of $kA$

Choose $m = 2^p$
Choose: A not too close to 0 or 1
Knuth: Good choice for $A = (\sqrt{5} - 1)/2$

Worst Case Scenario

#### Clustering
Primary clustering means that if there is a cluster and the initial position of a new record would fall anywhere in the cluster the cluster size increases. Linear probing leads to this type of clustering.

Secondary clustering is less severe, two records do only have the same collision chain if their initial position is the same. For example quadratic probing leads to this type of clustering.


### Universal Hashing
- Randomize the algorithm to combat O(n) worst case
- Universal hashing - pick a hash function randomly in a way that is independent of the keys that are actually going to be stored. This guarantees goof performance of average, even if worse case keys are chosen

## Chaining
- Puts elements that hash to the same slot in a linked list
- Mostly used when it is unknown how many and how frequently keys may be inserted or deleted
-  Cache performance of chaining is not good as keys are stored using a linked list. Open addressing provides better cache performance as everything is stored in the same table
- If the chain becomes long, then search time can become O(n) in the worst case

Also called Seperate chaining or direct chaining, open hashing (none of the objects have to be stored at the hash table i.e., your are free to leave the hash table and use a seperate list), closed addressing (location of the item is determined by its hash value)



#### Types of Chaining
1. Simple uniform hashing - each key in the table is equally likely to be hashed to any slot
	- The load factor here is n/m
	- Average cost of an unsuccessful seach for a key is $O(1+\alpha)$
	- Average cost of a successful search is $O(1+\frac{\alpha}{2}) = O(1+\alpha)$
	- Hence the cost of searching is $O(1+\alpha)$
	- If the number of keys n is proportinal to the number of slots in the table then $\alpha = O(1)$
	- In the worst case, all the hashed values will fall into the same linked list. Therefore we can use other data structures such as a Balanced BST to reduce the worst case

## Open Addressing
aka closed hashing

- Basic Idea:
	- To insert: if the slot is full, try another slot, until an open slot is found (probing)
	- To search: follow the same sequence of probes as would be used when inserting the element
		- If you reach the element with the correct key, return it
		- If you reach a NULL pointer, then the element is not in the table
- good for fixed sets (adding but no deletion)
- table needn't be much bigger than n

Types of probing:
- Linear Probing - Interval between the probes is fixed (normally 1) (H+1, H+2, H+3, ...)
	- Issues
		- Primary Clustering
		- Deletion operation in complex since when an element in a cluster is deleted, the probing sequence should be adjusted
- Quadratic Probing - Interval is increased by adding the successive output of a quadratic polynomial to the starting hash value ($H+1^2, H+2^2, H+3^2, H+4^2, ...$)

Advantages:
1. Avoids time overhead of allocating each new entry
2. Avoid indirection to access the first entry
3. Better locality of reference especially with linear probing and smaller records
4. Easier to serialize since no pointers
Disadvantages
1. Stored entries cannot exceed the number of slots
2. Poor choice for LARGE elements
3. Even with a Good Hash Function, the performance degrades with load factors above 0.7
4. Complex deletion:
   in open-address table we can’t simply mark a slot containing deleted key
as empty. Search for keys may become incorrect. The classical method to implement
deletion is to mark slots in hash table by three values: “free”, “busy”, “deleted”
![[Pasted image 20240212205116.png]]


