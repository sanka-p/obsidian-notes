n/2 -> n/4 -> n/8 -> ... -> 1
$\frac{n}{2^x}=1$
$x = log_2n$

### Recursive Implementation
```cpp
int binary_search(const int arr[], int target, int left, int right) {
    if (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;
        }

        else if (arr[mid] > target) {
            return binary_search(arr, target, left, mid - 1);
        }

        else {
            return binary_search(arr, target, mid + 1, right);
        }
    }

    // Target is not present in the array
    return -1;
}
```

this is a tail recursion so compilers would not save function calls in the stack??