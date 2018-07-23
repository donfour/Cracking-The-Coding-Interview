## 1. Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

```
class Solution {
public:
    // O(n^2) runtime, O(1) space
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
    // O(n) runtime, O(n) space
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
  
## 2. Product of Array Except Self

Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

```
class Solution {
public:
    // O(n) runtime, O(n) space
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 1);
        vector<int> fromBegin(n, 1);
        vector<int> fromEnd(n, 1);
        for(int i=1; i<n; i++){
            fromBegin[i] = fromBegin[i-1]*nums[i-1];
            fromEnd[i] = fromEnd[i-1]*nums[n-i];
        }
        for(int i=0; i<n; i++){
            result[i] = fromBegin[i]*fromEnd[n-1-i];
        }
        return result;
    }
    // O(n) runtime, O(1) space if we don't count the result vector
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 1);
        int fromBegin = 1;
        int fromEnd = 1;
        for(int i=1; i<n; i++){
            fromBegin *= nums[i-1];
            result[i] *= fromBegin;
            fromEnd *= nums[n-i];
            result[n-1-i] *= fromEnd;
        }
        result[0] = fromEnd;
        result[n-1] = fromBegin;
        return result;
    }
    
};
```
First solution:

Let's take [1,2,3,4,5] as an example.

After the 1st pass,
- fromBegin = [1,1*1,2*1*1,3*2*1*1,4*3*2*1]
- fromEnd = [1,5*1,4*5*1,3*4*5*1,2*3*4*5*1]
Notice they are kind of like mirror images of one another, i.e. each vector contains half the product needed. Therefore, in order to construct the result vector, we have to combine the fromBegin and fromEnd vectors like so:

result[i] = fromBegin[i]*fromEnd[n-1-i];

More specifically,
- result[0] = fromBegin[0]*fromEnd[5-1-0] = (1) * (2*3*4*5*1) = 2*3*4*5 -> product of all elements except 1
- result[1] = fromBegin[1]*fromEnd[5-1-1] = (1*1) * (3*4*5*1) = 1*3*4*5 -> product of all elements except 2
- result[2] = fromBegin[2]*fromEnd[5-1-2] = (2*1*1) * (4*5*1) = 1*2*4*5 -> product of all elements except 3

...

Space optimization:

If we look at how we calculated the `result` vector from `fromBegin` and `fromEnd` vectors, we'll find that each element in fromBegin and fromEnd were actually only used once. This means we don't have the store the imtermediate values calculations in the fromBegin and fromEnd vectors.

More specifically, following the example with [1,2,3,4,5], we can first initialize fromBegin and fromEnd as 1, and result as [1,1,1,1,1], then:

In the first iteration,
- fromBegin = 1*1
- fromEnd = 5*1

At this instance, fromBegin can already be multiplied into result[1] and fromEnd can already be multiplied into result[5-1-1].

In the second iteration,
- fromBegin = 1*1*2
- fromEnd = 4*5*1

At this instance, fromBegin can already be multiplied into result[2] and fromEnd can already be multiplied into result[5-1-2].

After the first pass, all values in result are ready except the first and the last ones. Right now, 
- fromBegin = 1*1*2*3*4
- fromEnd = 2*3*4*5*1

Meaning fromBegin is exactly the value of result[4] and fromEnd is exactly the value of result[0]! Therefore, we can simply:
```
result[0] = fromEnd;
result[n-1] = fromBegin;
```
