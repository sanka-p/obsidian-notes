### Brute Force
```
ALGORITHM BruteForce
	int ans = -inf;
	for(int i=0; i<n; i++)
		int curr_sum = 0;
		for(int j=i; j<n; j++)
			curr_sum += arr[j];
			ans = max(ans, curr_sum);
```
### Kadanes Algorithm
```cpp
class Solution {

public:

int maxSubArray(vector<int>& nums) {

int curr_max = nums[0];

int ans = curr_max;

for(int i=1; i<nums.size(); i++){

curr_max = max(nums[i], curr_max+nums[i]);

ans = max(curr_max, ans);

}

return ans;

}

};
```