### Complexity Analysis
Recurrence Relation

Example for bianry search
T(n) = T(n/2) + O(1)
T(1) = c
![[Pasted image 20231219010405.png]]
### Tail Recursion
a recursive function in which the recursive call is the last statement that is executed by the function

tail recursive functions are considered better than non-tail recursive functions as tail-recursion can be optimized by the compiler

Tail-call optimization is where you are able to avoid allocating a new stack frame for a function because the calling function will simply return the value that it gets from the called function
### Example 1: Factorial
```cpp
int factorial(int n, int result = 1){
	if (n <= 1)
		return result;
	factorial(n-1, result*n)
}
```
Space Complexity = O(1)
## Example 2 : Power
```cpp
int pow(int a, int k){
	if (k==1) return a;

	int j = pow(a, k/2);

	if (k%2 == 1) // odd
		return j*j*a;
	else
		return j*j;
}
```

```cpp
int pow(int a, int k, int result = 1) {
    if (k == 0) {
        return result;
    } else if (k % 2 == 1) {
        // If k is odd, multiply the accumulator by a and continue with k/2
        return pow(a * a, k / 2, result * a);
    } else {
        // If k is even, just square a and continue with k/2
        return pow(a * a, k / 2, result);
    }
}
```

Memoization
Fibonacci
Before $O(n^2)$
After $O(n)$