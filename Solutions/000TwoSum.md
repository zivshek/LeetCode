# [1. Two Sum](https://leetcode.com/problems/two-sum/description/)

### Thoughts:

At first glance, I thought it is the same as the learning problem [Two Sum II](https://leetcode.com/explore/learn/card/array-and-string/205/array-two-pointer-technique/1153/), and I was gonna use the two-pointer technique. Then I realized this array is not sorted. I can sort it for sure, but then the indices would change.

It can be done easily with two loops, we just compare each element with the rest (only those haven't been compared yet, thus **j = i + 1;**), the problem is the time complexity would be n * sigma (n - 1 => 1), which basically is O(n * n). [=>Jump to solution 1](#c-solution-1)

But how do we reduce the time complexity when you have to find two related values in an unsorted array? I took a look at other people's solutions, and they are all using a hashtable or a map. I've used std::map a lot, but I've never really learnt how it works internally. To fully understand it, I checked a hashtable implementation online, and it turned out, with a key to find something in a hashtable, all it does is to do some hash operation to get the corresponding index, and thus the value. The tricky part is to avoid or solve collision of the indices, because two different keys may result in two indentical indices. To solidify my understanding, I plan to implement a naive hashtable myself and then use it for this problem.

So I wrote this simplified HashTable class(non template fancy stuff, no remove or whatnot), it can only set and get, and the hash() function is buggy, since all I did was to get the remainder of the key divided by the table size, and the table size is only 100. One caveat is that if I don't initialize all HashNodes to NULL, they won't be NULL when you check them. So, like teachers have always emphisized, initialize your variables, even to NULL! 

Btw, when testing a key that's negative, my hash() function would also return a negative index, which would be invalid, so my temp solution is to add a random large number, 123456, which is not the correct way, but I'll leave it like this for now, because the best practice is to use std::map or std::unordered_map instead. 

As to efficiency, for this simple case, since we are dealing with integers, there would be no possiblity we would ever encounter collision problem, so the look up time would be just 1 operation. But I believe even at more complex situations, a good defined HastTable class would prevent from having too many nodes in one entry. However, it the worst senario happens, the look up time would still be O(n). For this case tho, O(1). As to space complexity, the space should at least be as big as the size of the array being passed in, so O(n). [=>Jump to solution 2](#c-solution-2)


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

##### C++ Solution 2 
HashTable
```C++
class Solution {
    class HashTable {

    private:
        struct HashNode {
            int key;
            int value;
            HashNode* next = nullptr;

            HashNode(int k, int v) {
                key = k;
                value = v;
            }
        };

        const int size = 100;
        HashNode** table;
    public:
        HashTable() {
            table = new HashNode*[size];
            // Initialize them to null here, important!
            for (int i = 0; i < size; ++i)
                table[i] = NULL;
        }

        ~HashTable() {
            for (int i = 0; i < size; ++i) {
                HashNode* curr = table[i];
                while (curr != NULL) {
                    HashNode* prev = curr;
                    curr = curr->next;
                    delete prev;
                }
                table[i] = nullptr;
            }
            delete[] table;
        }

        void set(const int& k, const int& v) {
            int index = hash(k);
            HashNode* prev = nullptr;
            HashNode* curr = table[index];
            // if it's not null but it is not the key we're passing in
            // it means it has children, let's find the last child
            while (curr != NULL && curr->key != k) {
                prev = curr;
                curr = curr->next;
            }

            if (curr == NULL) {
                curr = new HashNode(k, v);
                // if prev hasn't been assigned to anything
                // meaning this is the start point of this index
                if (!prev) {
                    table[index] = curr;
                }
                else {
                    prev->next = curr;
                }
            }
        }

        bool hasKey(const int& k, int& outV) {
            int index = hash(k);
            HashNode* curr = table[index];
            while (curr != NULL) {
                // this is awkward, as we need this function to return 
                // both a bool and the value, so
                if (curr->key == k) {
                    outV = curr->value;
                    return true;
                }
                curr = curr->next;
            }
            return false;
        }

    private:
        int hash(const int& k) {
            return (k + 123456) % size;
        }
    };

    vector<int> twoSum(vector<int>& nums, int target) {
        HashTable table;
        for (int i = 0; i < nums.size(); ++i) {
            int complement = target - nums[i];
            int complementIndex;
            if (table.hasKey(complement, complementIndex)) {
                return { complementIndex, i };
            }
            table.set(nums[i], i);
        }
        return {};
    }
};
```
Time complexity: O(n) * O(1) = O(n), Space complexity: O(n)
