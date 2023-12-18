```
ALGORITHM MergeSort(A, left, right)
	if left >= right
		return
	mid <- (left + right) / 2
	MergeSort(A, left, mid)
	MergeSort(A, mid+1, right)
	Merge(A, left, mid, right)

FUNTION Merge(A, left, mid, right)
	
```


$T(n) = T(n/2) + T(n/2) + n$ = 2T(n/2) + n
n - time taken by merge function
$T(n/2) = 2T(n/4) + n/2$
$T(n) = 2^2T(n/4) + 2n$
$T(n) = 2^3T(n/8) + 3n$
....
$T(n) = 2^{logn}c + nlogn$
$T(n) = nc + nlogn$
$T(n)=O(nlogn)$


Dividing will take order of $logn$ time
Each divided level should be combined
	at each level all n elements will be looked at once
Therefore runtime is $O(nlogn)$

Space complexity
