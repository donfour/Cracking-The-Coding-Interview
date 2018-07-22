1. Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

```
class Solution {
public:
    // O(n^2) Solution
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        for(int j=1; j < nums.size(); j++){
            for(int i=0; i < j; i++){
                if(nums[i] + nums[j] == target){
                    result.push_back(i);
                    result.push_back(j);
                    return result;
                }
            }
        }
        return result;
    }
    // O(n) Solution
    vector<int> twoSum(vector<int>& nums, int target) {
        // maps remainder (target - current_val) to vector index (i)
        // e.g. sum[7] = 1 means 7 + nums[1] = target
        unordered_map<int, int> sum;
        vector<int> result;
        for(int i=0; i<nums.size(); i++){
            int current_val = nums[i];
            if(sum.count(current_val) > 0) { // if current_val is a key in the unordered_map
                result.push_back( sum[current_val] );
                result.push_back(i);
                return result;
            }
            sum[target - current_val] = i;
        }
        return result;
    }
};
```
Edge cases:
- 0 + 0 = 0
- -3 + 3 = 0
- (-3) + (-5) = -8

Mistakes I made:
- Assumed current_val has to be <= target
  * this is only correct if all numbers are positive, otherwise we can have -3 + 3 = 0, where 3 is > 0
