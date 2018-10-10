#[1. Two Sum] (https://leetcode.com/problems/two-sum/description/)

###Thoughts:

At first glance, I thought it is the same as the learning problem [Two Sum II](https://leetcode.com/explore/learn/card/array-and-string/205/array-two-pointer-technique/1153/), and I was gonna use the two-pointer technique. Then I realized this array is not sorted. I can sort it for sure, but then the indices would change.

It can be done easily with two loops, we just compare each element with the rest (only those haven't been compared yet, thus ***j = i + 1;***), the problem is the time complexity would be n * /sigma (n - 1 => 1), which basically is O(n * n).



###Solutions:

1. 
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int l = nums.size();
        for (int i = 0; i < l - 1; ++i) {
            for (int j = i + 1; j < l; ++j) {
                if (nums[i] + nums[j] == target)
                    return {{i, j}};
            }
        }
        return {};
    }
};
```
Time complexity: O(n * n)

Space complexity: O(1)