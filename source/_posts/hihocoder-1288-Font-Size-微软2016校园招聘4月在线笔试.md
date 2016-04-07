title: 'hihocoder 1288:Font Size--微软2016校园招聘4月在线笔试'
date: 2016-04-07 09:16:59
tags:
- hihocoder
---
## 1288 : Font Size
时间限制:10000ms
单点时限:1000ms
内存限制:256MB

**描述**
Steven loves reading book on his phone. The book he reads now consists of N paragraphs and the i-th paragraph contains ai characters.

Steven wants to make the characters easier to read, so he decides to increase the font size of characters. But the size of Steven's phone screen is limited. Its width is W and height is H. As a result, if the font size of characters is S then it can only show ⌊W / S⌋ characters in a line and ⌊H / S⌋ lines in a page. (⌊x⌋ is the largest integer no more than x)  

So here's the question, if Steven wants to control the number of pages no more than P, what's the maximum font size he can set? Note that paragraphs must start in a new line and there is no empty line between paragraphs.

**输入**
Input may contain multiple test cases.

The first line is an integer TASKS, representing the number of test cases.

For each test case, the first line contains four integers N, P, W and H, as described above.

The second line contains N integers a1, a2, ... aN, indicating the number of characters in each paragraph.



For all test cases,

1 <= N <= 103,

1 <= W, H, ai <= 103,

1 <= P <= 106,

There is always a way to control the number of pages no more than P.

**输出**
For each testcase, output a line with an integer Ans, indicating the maximum font size Steven can set.

**样例输入**
>2
1 10 4 3
10
2 10 4 3
10 10

**样例输出**
>3
2

**解题思路**
水题。遍历所有的字体大小，计算需要的行数，进而计算页数。。。注意，每段要分行，段首不考虑是否缩进。
```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main(){
    int t,n,p,w,h;
	while(cin>>t){
    while(t--){
        cin>>n>>p>>w>>h;
		int flag=0;
        int *a=new int[n];
        for(int i=0;i<n;i++)cin>>a[i];
        int ans=w>h?h:w;
        for(int s=ans;s>=1;s--){
            int k=0;
			int tmp=w/s;
            for(int i=0;i<n;i++){
                k+=a[i]/tmp;
                if(a[i]%tmp)k++;
            }
			tmp=h/s;
			int kk=k/tmp;
			if(k%tmp)kk++;
            if(kk<=p){
                cout<<s<<endl;
				flag=1;
                break;
            }
        }
		if(!flag)cout<<0<<endl;
    }
	}
    return 0;
    
}
```