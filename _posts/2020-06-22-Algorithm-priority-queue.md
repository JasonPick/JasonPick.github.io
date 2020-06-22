---
layout: post
title: 队列专题
subtitle: Part 2：优先队列
tags: [algorithms, queue]
comments: true
categories: ['algorithms']
---

在这里我们将讨论优先队列。


## 优先队列


优先队列在队列的基础上进行了扩展：


* 每个元素都有与之关联的优先级。 

* 高优先级的元素在低优先级的元素之前出队。

* 如果两个元素具有相同的优先级，则将根据它们在队列中的顺序为其提供服务。


### 优先队列的实现

通常，堆用于实现优先级队列，因为与数组或链表相比，堆可提供更好的性能。


| 堆类型 | getHighestPriority()时间复杂度 | insert()时间复杂度 |deleteHighestPriority()时间复杂度|
| :------ |:--- | :--- |
| 二进制堆 Binary| O(1) | O(logN) |O(logN)|
| 斐波那契堆 Fibonacci| O(1) | O(1) |O(logN)|


## Practice - Leetcode
	

###  #703 K largest 
int k = 3;
int[] arr = [4,5,8,2] <- ...10

切分问题，Max，每次问largest


思路：


1. 排序 k.Max -> sorted 时间复杂度 O(klogk)

2. 优先队列 mini heap 复杂度 O(N* (1 or logK))  

![](/img/heap.001.png)

``` python
import heapq
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
      self.k = k
      self.heap = []
      for n in nums:
          self.add(n) 

        

    def add(self, val: int) -> int:
        if len(self.heap) < self.k:
            heapq.heappush(self.heap,val)
        elif self.heap[0] < val:
            heapq.heappop(self.heap)
            heapq.heappush(self.heap,val)
        return self.heap[0]
	
```
        



###  #239 Sliding Window K  
找到当前窗口的最大值

int k = 3;

int[] arr = [3,1,3,-1，-3，5，3，6] 

Output: [3,3,5,5,6]


思路：

1. Priority Queue -> Max Heap  

a. 维护一个heap, insert delete

b. Max是顶端元素 

时间复杂度O(Nlogk)

2. 策略 -> 只要有一个比前面都要大的元素进来，前面的元素都不用管，优胜劣汰
queue -> deque(array)

a. 入队

b. 维护

1,[3,-1]--> [3,-1,-3]--> -1,-3,[5]-->[5,3]--> [6]

时间复杂度 O(N*1)

***传入参数判空真的很重要！！！！！***


``` c++
#include <queue>
class Solution {

public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        if (nums.size()  == 0||k==0) return vector<int>(0);
        if (k == 1) return nums;

        vector<int> res;
        deque<int> dq;
        
        for (int i =0; i<nums.size(); i++){
            // remove index not in the window;
            if (!dq.empty() && dq.front() == i-k)
                dq.pop_front();
            // remove the index that smaller than current val
            while(!dq.empty() && nums[i] > nums[dq.back()]) 
                dq.pop_back();
            dq.push_back(i);
            
           if (i>= k-1) res.push_back(nums[dq.front()]);
            
        }
        return res;
    }
};
```
