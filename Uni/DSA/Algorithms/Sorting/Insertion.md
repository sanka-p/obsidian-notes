```
ALGORITHM InsertionSort(A[1,...,n])
//input: an array A[1,..,n] of n orderable elements
//output: array A[1,...,n] sorted in nondecreasing order
for i <- 2 to n do	
	key = A[i]
	j <- i-1
	while j > 0 and A[j] > key do
		A[j+1] <- A[j]
		j <- j - 1
	A[j+1] = key
```

### Runtime Analysis
Time Complexity
- Best Case (Array is already sorted in ascending order): $O(n)$
- Worst Case (Array is already sorted in descending order): $O(n^2)$
Space Complexity
- $O(1)$