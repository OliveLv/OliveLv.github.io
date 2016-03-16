title: 153. Find Minimum in Rotated Sorted Array
date: 2016-03-16 16:31:47
tags:
- leetcode
---
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.

结题思路：
方法一：遍历数组，判断是否存在nums[i-1]>nums[i]的情况，若有则返回nums[i]，否则返回nums[0]。
```java
class Solution {
public:
    // 6ms
    int findMin(vector<int>& nums) {
        int n=nums.size();
        if(n==1)return nums[0];
        for(int i=1;i<n;i++){
            if(nums[i-1]>nums[i])return nums[i];
        }
        return nums[0];
    }
};
```
方法二：类二分法
将数组的中值与最右元素比较，若中间值大于最右元素，则说明右部分的数组存在递增递减的情况，即最小值在该区域内；若中间值小于最右元素，则最小区域在左部分。
```java
class Solution {
public:
    // 4ms
    int findMin(vector<int>& nums) {
        int n=nums.size();
        if(n==1)return nums[0];
        int l=0,r=n-1;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>nums[r])l=mid+1;
            else
            r=mid;
        }
        return nums[l];
    }
};
```

