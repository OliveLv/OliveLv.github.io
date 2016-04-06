title: 'hihoCoder 1135: Magic Box'
date: 2016-04-06 15:12:59
tags:
- hihocoder
---
## 1135 : Magic Box
时间限制:10000ms
单点时限:1000ms
内存限制:256MB
描述
The circus clown Sunny has a magic box. When the circus is performing, Sunny puts some balls into the box one by one. The balls are in three colors: red(R), yellow(Y) and blue(B). Let Cr, Cy, Cb denote the numbers of red, yellow, blue balls in the box. Whenever the differences among Cr, Cy, Cb happen to be x, y, z, all balls in the box vanish. Given x, y, z and the sequence in which Sunny put the balls, you are to find what is the maximum number of balls in the box ever.

For example, let's assume x=1, y=2, z=3 and the sequence is RRYBRBRYBRY. After Sunny puts the first 7 balls, RRYBRBR, into the box, Cr, Cy, Cb are 4, 1, 2 respectively. The differences are exactly 1, 2, 3. (|Cr-Cy|=3, |Cy-Cb|=1, |Cb-Cr|=2) Then all the 7 balls vanish. Finally there are 4 balls in the box, after Sunny puts the remaining balls. So the box contains 7 balls at most, after Sunny puts the first 7 balls and before they vanish.

**输入**
Line 1: x y z

Line 2: the sequence consisting of only three characters 'R', 'Y' and 'B'.

For 30% data, the length of the sequence is no more than 200.

For 100% data, the length of the sequence is no more than 20,000, 0 <= x, y, z <= 20.

**输出**
The maximum number of balls in the box ever.

**样例输入**
>1 2 3
RRYBRBRYBRY
0 0 0
RBYRRBY    

**样例输出**
>7
4

**解题思路**
题目意思是顺序将序列对应的气球放到box中，每次判断box中不同颜色气球的差是否正好等于x，y，z，若相等，则在box中的气球都爆炸。求在爆炸之前box中放的最大气球数目。
直接暴力方法就可以解决。使用数组a保存x，y，z（但数组是有序的，从小到大），使用数组b保存不同颜色气球的差，b[0]=['R']-['Y']，b[1]=['R']-['B']，b[2]=['Y']-['B']。

```c++
#include<iostream>
#include<string>
#include<vector>
#include<cmath>
#include<algorithm>
using namespace std;
bool compare(int a[],int b[]){
    int c[3]={0};
    for(int i=0;i<3;i++)c[i]=abs(b[i]);
    sort(c,c+3);
    for(int i=0;i<3;i++)
        if(a[i]!=c[i])return false;
    return true;
}
int main(){
    int a[3]={0};
    while(cin>>a[0]>>a[1]>>a[2]){
        sort(a,a+3);
        string s;
        cin>>s;
        int n=s.length();
        int m=0;
        int cur=0;
        int b[3]={0};
        for(int i=0;i<n;i++){
            if(s[i]=='R'){
                b[0]++;
                b[1]++;
            }
            else if(s[i]=='Y'){
                b[2]++;
                b[0]--;
            }
            else{
                b[2]--;
                b[1]--;
            }
            cur++;
            if(compare(a,b)){
                if(m<cur)m=cur;
                cur=0;
                for(int i=0;i<3;i++)b[i]=0;
            }
        }
        if(m<cur)m=cur;
        cout<<m<<endl;
    }
    return 0;
}
```