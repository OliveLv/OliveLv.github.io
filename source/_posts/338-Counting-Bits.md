title: 338. Counting Bits
date: 2016-03-22 11:25:25
tags:
- leetcode
---
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example:
For num = 5 you should return [0,1,1,2,1,2].

Follow up:

It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
Space complexity should be O(n).
Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

解题思路：
这个题目类似89. Gray Code，找规律
0 - 0
1 - 1
2 - 1
3 - 2
4 - 1
5 - 2
6 - 2
7 - 3
8 - 1
$[2^{(i-1)},2^i-1]$是由$[0,2^{(i-1)}-1]$数组+1得到的。例如上面的[4,7]的值是由[0,3]+1得到的。
```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int>res(num+1,0);
        int base=1;
        int k=1;
        while(k<=num){
        for(int j=0;j<base&&k<=num;j++)
                res[k++]=res[j]+1;
            base<<=1;
        }
        return res;
         
    }
};
```

