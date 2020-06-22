---
layout: post
title: 队列专题
subtitle: Part 2：优先队列
tags: [algorithms, queue]
comments: true
categories: ['algorithms']
---

这里我们讨论利用动态规划算法来解决问题

## 概述

动态规划的特点：

* 求一个问题的最优解
* 这个最优解依靠与子问题的最优解
* 若干小问题之间有相互重叠的更小的子问题



## 动态递推

-  ### DP


1.递归 + 记忆化 -> 递推；

2.状态的定义：opt[n], dp[n], fib[n]..so on.

3.状态转移方程：opt[n] = best_of (opt[n-1], opt[n-2], ...);

4.最优子结构；

  （1）从斐波那契数列开始，如果用递推解决问题的话，复杂度为O(2^n)；
  
  如果用记忆化的递推的话，
  ``` python
  int fib(int n, int[] memo){
    if n <= 1:
      return n
    elif !memo:
      memo[n] = fib[n-1] + fib[n-2];
    return memo[n]; 
  }
  ```
  
  
  （2）给定一张地图（二维数组），地图上有一些障碍物，问小人从start point到end point有多少种走法
  
  
``` python
path(start, end) = 
           path(A, end)                 +                 path(B, end)=
    path(C, end) + path(D, end)         +         path(C, end) + path(E, end)
    
```

{.box-warning}

关键在于每次只想下一步，不要考虑太多


``` python
递推
int countPath(boolean[] grid, int row, int col){
    if（！ValidSquare(grid, row, col)） return 0;
    if (isAtEnd(grid, row, col)) return 1;
    return countPath(grid, row+1, col) + countPath(grid, row, col+1)；
}

递归（从下到上，返向推导）
转移方程：opt[i,j] = opt[i-1, j] + opt[i, j-1]
代码：
for i: 0->m, j:0->n :
    if a[i,j] = '':#is valid
      opt....
    else:
      opt[i,j] = 0
```


{.box-warning}

回溯法 ==== 重复计算，傻傻计算；

贪心法 ==== 永远局部最优，眼前利益；

DP ===== 记录局部最优子结构；



## Practice - Leetcode
	

### #70 Climbing Stairs

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2

解释： 有两种方法可以爬到楼顶。

1.  1 阶 + 1 阶

2.  2 阶

方法：

   1.傻搜法，回溯：
   
   2.DP法：
   
        a. DP状态定义
        b. DP方程
        
        for i 2->n:
            f[n] = f[n-1] + f[n-2]
   ```
   class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2)
            return n;
        vector<int> fib{1,1};
        for(int i=2; i<=n; i++){
            fib.push_back(fib[i-1]+fib[i-2]);
        }
        return fib[n];
        
        
    }
};
   ```
   
   
### #120 Triangle

给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

例如，给定三角形：

[
     [2],
     
   [3,4],
    
   [6,5,7],
   
  [4,1,8,3]
]

自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。

方法：

   1.傻搜法，回溯：
     Traverse[i+1,j] , Traverse[i, j+1]
     
   2.DP:
   
    a.状态定义：DP[i,j],走到（i，j）的时候最小的路径之和；
    b.DP方程： 4，5，7 就是 5和7之中和较少 + 4 val
            DP[i, j] = min(DP[i+1, j], Dp[i+1, j+1]) + Triangle[i, j]
            DP[m-1, j] = Triangle[m-1, j]
            
``` c++
    class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int m = triangle.size();
        
        vector<vector<int>> dp(triangle);
        
        for (int i = m-2; i>=0; i--)
            for (int j = 0; j<triangle[i].size(); j++)
                dp[i][j] += min(dp[i+1][j],dp[i+1][j+1]);
        return dp[0][0];
        
        
    }
};
```
   
### #152 乘积最大子序列
给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。
```
示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```
方法
   1.暴力求解：
    
   2.DP法：
```     
     a.状态定义：DP[i][0]: 从0到i位置子序列的乘积最大值
     	       DP[i][1]: 从0到i位置子序列的乘积最小值
     b.DP方程：
     	2 3 -2 4
	DP[i+1] = DP[i] * a[i+1]
	if a[i+1] >= 0:
	    DP[i+1] = Max * a[i+1];
	else:
	    DP[i+1] = Min * a[i+1];

	
 ```      


```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(!nums.size()) return 0;
        vector<vector<int>> DP(2,vector<int>(2,0));
        int res;
        DP[0][0] = DP[1][0] = res = nums[0];
        for(int i = 1; i<nums.size(); i++)
        {
            int x = i % 2;
            int y = (i-1) % 2;
            
            DP[0][x] = max(DP[1][y]*nums[i],max(DP[0][y]*nums[i],nums[i]));
            DP[1][x] = min(DP[1][y]*nums[i],min( DP[0][y]*nums[i],nums[i])); 
            
            res = max(DP[0][x],res);
        }
        
        return res;
        
        
    }
};
```


### #121 股票系列


121 -> 只能买一次和卖一次；


122 -> 可以买卖无数次；


123 -> 只能买两次；


309 -> 冷冻期；


188 -> 只能买卖k次；


714 -> 买卖有手续费；


[7， 1， 5， 3， 6， 4]


#121 

思路：遍历一次数组，每次记录最小值，新的数和最小值相减看是不是最大。


```
泛化框架
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max(   选择 rest  ,           选择 sell      )

解释：今天我没有持有股票，有两种可能：
要么是我昨天就没有持有，然后今天选择 rest，所以我今天还是没有持有；
要么是我昨天持有股票，但是今天我 sell 了，所以我今天没有持有股票了。

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max(   选择 rest  ,           选择 buy         )

解释：今天我持有着股票，有两种可能：
要么我昨天就持有着股票，然后今天选择 rest，所以我今天还持有着股票；
要么我昨天本没有持有，但今天我选择 buy，所以今天我就持有股票了。

```


```c++ 
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()<=1)
            return 0;
        int Min = prices[0];
        int Max = 0;
        for (int i = 1;i<prices.size();i++){
            Max = max(Max,prices[i]-Min);
            Min = min(Min, prices[i]);
        }
        return Max;
        
    }
};
```
