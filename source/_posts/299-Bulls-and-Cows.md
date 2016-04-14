title: 299. Bulls and Cows
date: 2016-04-14 19:54:10
tags:
- leetcode
---
You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

For example:

Secret number:  "1807"
Friend's guess: "7810"
Hint: 1 bull and 3 cows. (The bull is 8, the cows are 0, 1 and 7.)
Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. In the above example, your function should return "1A3B".

Please note that both secret number and friend's guess may contain duplicate digits, for example:

Secret number:  "1123"
Friend's guess: "0111"
In this case, the 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow, and your function should return "1A1B".
You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.


解题思路
水题。。。。
注意 c++中int转string，直接用"+"操作是不可行的。需要利用**stringstream**来转换。


```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        int a1[10]={0};
        int a2[10]={0};
        int bull=0;
        for(int i=0;i<secret.length();i++){
            char a=secret[i];
            char b=guess[i];
            if(a==b)bull++;
            a1[a-'0']++;
            a2[b-'0']++;
             
        }
        int cow=0;
        for(int i=0;i<10;i++){
            cow+=(a1[i]>a2[i]?a2[i]:a1[i]);
        }
        string res="";
        stringstream ss;
        ss<<bull;
        res+=ss.str()+"A";
        ss.str("");
        ss<<(cow-bull);
        res+=ss.str()+"B";
        return res;
    }
};
```


