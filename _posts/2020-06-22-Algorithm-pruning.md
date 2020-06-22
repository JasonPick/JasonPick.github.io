---
layout: post
title: 剪枝二分和字典数专题
subtitle: 经典题目整理
tags: [algorithms, greedy]
comments: true
categories: ['algorithms']
---

## 剪枝

**经典题目**

-### # 51.52 N皇后问题

思路：

* 暴力搜索

* 剪枝和数组

```c++
```

-### #36.37数独


思路：

* DFS

* 加速的DFS



## 二分查找

适用场景：

* 排序的数组

* 有边界的

* 可以通过index来访问的

模版

```c++
while (left <= right){
  int mid = left + (right-left)/2;
  if(mid == target) return res;
  else if(array[mid] < target){
    left = mid+1;
  }else{
    right = mid+1;
  }

}

```


Leetcode 


-### #69 int sqrt(int x)

思路：

* 二分法

* 牛顿迭代法



## 字典树

{.box-warning}

适用场景：用于统计和排序大量的字符串的场景，效率比哈希表高

用边存储字母，核心在于用空间换时间，利用公共前缀减少时间开销

叶子结点才是单词，中间结点不是




-### #208 实现一个字典树

思路：

*

*


-### #79.212 单词搜索II

思路：

*

*
