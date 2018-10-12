# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)

### Thoughts:

Intuitively, I was gonna get the first number from l1, and 2nd number from l2, add them and build the result list. As to how to get them, well, I have to get the digits from the nodes, and combine them together. Maybe I can covert them to a string, and add the new val before the old string, and thus, get the correct order of the digits. Then do the calculation of the two numbers, and create a new linked list based on the result. This method should work, but it's a little bit trivial, there are many string->int and int->string conversions, and will look ugly. There must be a better way of doing it.

