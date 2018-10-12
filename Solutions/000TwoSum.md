# [1. Two Sum](https://leetcode.com/problems/two-sum/description/)

### Thoughts:

At first glance, I thought it is the same as the learning problem [Two Sum II](https://leetcode.com/explore/learn/card/array-and-string/205/array-two-pointer-technique/1153/), and I was gonna use the two-pointer technique. Then I realized this array is not sorted. I can sort it for sure, but then the indices would change.

It can be done easily with two loops, we just compare each element with the rest (only those haven't been compared yet, thus **j = i + 1;**), the problem is the time complexity would be n * sigma (n - 1 => 1), which basically is O(n * n).

But how do we reduce the time complexity when you have to find two related values in an unsorted array? I took a look at other people's solutions, and they are all using a hashtable or a map. I've used std::map a lot, but I've never really learnt how it works internally. To fully understand it, I checked a hashtable implementation online, and it turned out, with a key to find something in a hashtable, all it does is to do some hash operation to get the corresponding index, and thus the value. The tricky part is to avoid or solve collision of the indices, because two different keys may result in two indentical indices. To solidify my understanding, I plan to implement a naive hashtable myself and then use it for this problem.


### Solutions:

##### C++ Solution 1
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
Time complexity: O(n * n), Space complexity: O(1)
