```
ALGORITHM BubbleSort(A[1,...,n])
//input: an array A[1,..,n] of n orderable elements
//output: array A[1,...,n] sorted in nondecreasing order
do
	flag <- false
	for counter <- 1 to n-1 do
		if A[counter] > A[counter+1]
			swap A[counter] and A[counter+1]
			flag <- true
while flag=false or n=1
```

### Runtime Analysis
Time Complexity
- Best Case (Array is already sorted in ascending order): $O(n)$
- Worst Case (Array is already sorted in descending order): $O(n^2)$
Space Complexity
- $O(1)$

