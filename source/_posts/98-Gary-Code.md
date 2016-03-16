title: 89. Gray Code
date: 2016-03-16 16:28:47
tags:
- leetcode
---
The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:

00 - 0
01 - 1
11 - 3
10 - 2
Note:
For a given n, a gray code sequence is not uniquely defined.

For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

解题思路：
当n=3时，假设前4个元素使用n=2的gray code，即[000,001,011,010]，那么剩下的数字都是1xx，只要将前4个元素逆序，再加上4即可，即[110,111,101,100]。。。构造方法就是这样的过程。。
```java
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int>res;
        res.push_back(0);
        if(n==0)return res;
        res.push_back(1);
        if(n==1)return res;
        int tmp=2;
        int num=(1<<n);
        while(tmp!=num){
            for(int i=tmp-1;i>=0;i--)res.push_back(res[i]+tmp);
            tmp<<=1;
        }
        return res;
    }
};
```
