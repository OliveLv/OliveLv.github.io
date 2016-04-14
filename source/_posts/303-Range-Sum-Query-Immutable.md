title: 303. Range Sum Query - Immutable
date: 2016-04-14 21:25:46
tags:
- leetcode
---
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

Example:
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3

解题思路
1、树状数组的思想。。这个耗时大,17ms

```java
public class NumArray {
    int a[];
    int N=1000005;
    private int lowbit(int x){
        return x&(-x);
    }
    private void update(int i,int x){
        while(i<N){
            a[i]+=x;
            i+=lowbit(i);
        }
    }
    private int sum(int i){
        int s=0;
        while(i!=0){
            s+=a[i];
            i-=lowbit(i);
        }
        return s;
    }
    public NumArray(int[] nums) {
        a=new int[N];
        int n=nums.length;
        for(int i=1;i<=n;i++)update(i,nums[i-1]);
    }
 
    public int sumRange(int i, int j) {
        return sum(j+1)-sum(i);
    }
}
```



2、用数组s记录前i个数的和。。。。s长度为sizeof(nums)+1，则cost 4ms，若长度为sizeof(nums)，则cost 3ms。

```java
public class NumArray {
    int a[];
    public NumArray(int[] nums) {
        int n=nums.length;
        if(n==0)return ;
        a=new int[n];
        a[0]=nums[0];
        for(int i=1;i<n;i++)a[i]=a[i-1]+nums[i];
    }
 
    public int sumRange(int i, int j) {
        int n=a.length;
        if(n==0)return 0;
        return (i==0?a[j]:a[j]-a[i-1]);
    }
}

```
