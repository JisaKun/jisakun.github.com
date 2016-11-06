---
title: "Q152 Maximum Product Subarray"
date: 2015-11-16
tags: Dynamic-Programming Array
categories: Leetcode

---
#### Problem Link:[Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) 

#### Solution 1

##### Idea:
拿到问题首先考虑了暴力求解，然后不出意外地 TLE 了……
这是一个动态规划问题，重点是写出状态转移方程，先来看看 Maximum Subarray 的情况：

``` java
local[i+1] = Max(local[i]+A[i+1], A[i+1])；
globla[i+1] = Max(global[i], local[i+1]);
```

其中 local[i+1] 为局部最优，可以理解为包含 A[i+1] 元素的最优解，而 global[i+1] 为前 i+1 个元素的最优解，整个还是很好理解的。

在来看看最大乘积的情况，因为乘积可能存在负负得正，一个最小的负值乘以负数反而得到一个较大的值，所以要同时维持局部最大值和局部最小值：

``` java
int minCopy = minLocal;
minLocal = Math.min(Math.min(minLocalnums[i], maxLocalnums[i]), nums[i]);
maxLocal = Math.max(Math.max(maxLocalnums[i], minCopynums[i]), nums[i]);
global = Math.max(global, maxLocal);
```



##### Time Complexity:
O(n)

##### Space Complexity:
O(1)

##### Source code:
``` java
public int maxProduct(int[] nums) {
    int LENGTH = nums.length;
    int minLocal=nums[0];
    int maxLocal = nums[0];
    int global = nums[0];
    for(int i=1; i<LENGTH; i++){
       int minCopy = minLocal;
       minLocal = Math.min(Math.min(minLocal*nums[i], maxLocal*nums[i]), nums[i]);
       maxLocal = Math.max(Math.max(maxLocal*nums[i], minCopy*nums[i]), nums[i]);
       global = Math.max(global, maxLocal);
    }
       return global;
}
```

##### Reference:
[LeetCode：Maximum Product Subarray](http://www.cnblogs.com/bakari/p/4007368.html)

---

