```
ALGORITHM SelectionSort(A[1,...,n])
//input: an array A[1,..,n] of n orderable elements
//output: array A[1,...,n] sorted in nondecreasing order
	for i <- 1 to n-1 do
		min <- i
		for j <- i+1 to n do
			if A[j] < A[min] then
				min <- j
		swap A[i] and A[min]
	
```

```c++
void selection_sort(int arr[], int n) {
    for (int i = 0; i < n - 1; ++i) {
        // Find the minimum element in the unsorted part of the array
        int minIndex = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }

        // Swap the found minimum element with the first element
        swap(arr[i], arr[minIndex]);
    }
}
```
### Runtime Analysis
Time Complexity
- Best Case (Array is already sorted in ascending order): $O(n^2)$
- Worst Case (Array is already sorted in descending order): $O(n^2)$
- The outer loop runs n-1 times and inner loop will run $(n-1) + (n-2) + ... + 1 = \frac{(n-1)(n-2)}{2} = O(n^2)$
Space Complexity
- $O(1)$

