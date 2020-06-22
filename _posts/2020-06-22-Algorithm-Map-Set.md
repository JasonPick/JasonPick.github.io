---
layout: post
title: MAP & SET 专题
subtitle: 经典题目整理
tags: [algorithms, map, set]
comments: true
categories: ['algorithms']
---

## 哈希表 & 哈希函数 & 哈希碰撞


-## 哈希函数：

{.box-notes}

哈希函数：哈希函数将一个较大的数字或字符串映射为一个较小的整数，可用作哈希表中的索引。


一个好的哈希函数应具有以下属性：

* 计算的有效性
* 应均匀分配键（每个键在每个表中的位置均等）


-## 哈希表：

{.box-notes}

哈希表：一个数组，用于存储 hash(record) 的指针。如果没有哈希函数值等于该条目的index，则哈希表中的条目为NIL。


- ## 哈希碰撞

{.box-notes}

哈希碰撞：因此两个key哈希之后可能会产生相同的值。在哈希表中，新的key插入时发现index已占用的情况称为冲突。

   
![](https://www.cs.auckland.ac.nz/software/AlgAnim/fig/dir_acc_table.gif)

处理哈希碰撞的方法：

* 链式方法：使每个单元指向具有相同哈希值的记录的链表，需要在表外额外增加内存

* 开放寻址：开放寻址的所有元素都存储在哈希表本身中，本个表index都包含一条记录或者NIL。在搜索元素时


- ### 解决办法：链式方法解决碰撞
![](https://www.cs.auckland.ac.nz/software/AlgAnim/fig/dir_acc_lists.gif)

- ### 解决办法：
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2015/08/openAddressing1.png)


## Map vs Set vs List
- ### List 
特点： 元素可重复，可以通过array或者list实现

时间复杂度：Access & Search:O(N) Insert: O(1)
- ### Map 
特点： key: value pair，可以通过dict实现

- ### Set 
特点： 不允许重复，可以通过set实现，相当于map的key，时间复杂度 Search: O(1 or logn)

## Hash Map vs Tree Map -- Hash Set vs Tree Set
***经常用作查询和计数***

- ###对比
Hash Map: Search -> O(1),    unsorted/disordered   in C++ unordered_map

Tree Map: Search -> O(logn), sorted                in C++ map 

具体问题具体分析：当对于时间复杂度要求很高的时候选择Hash Map，如果要求有序性则喧杂Tree Map。 （Python的dict使用hash map）


## Practice - Leetcode
	

###  #242 Valid Anagram 
'tar' -> 'rat'

'cat' -> 'atc'

'anagram' -> 'nagamar'

思路：

1. 排序 对string进行排序看排序过后的结果是否一致，时间复杂度 O(NlogN) 空间O(1)

例子：

'rat' -> 'art'

'tar' -> 'art'


``` python
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
 ```
 
2. Map and count
使用一个字典进行计数 {letter: count} 时间复杂度 O(N)空间O(N)

{'a':3, 'n':1, 'g':1, 'r': 1, 'm':1}


```
    def isAnagram(self, s: str, t: str) -> bool:
        dic1, dic2 = [0]*26, [0]*26
        for item in s:
            dic1[ord(item)-ord('a')] += 1
        for item in t:
            dic2[ord(item)-ord('a')] +=1
        return dic1 == dic2
 ```
###  #1 Two Sum 

array=[2,7,11,15] target= 9

Output: [0,1]

思路：

1. 暴力 两层循环进行查找，时间复杂度 O(N*N)

2. set check if there is a y = 9 - x in set，时间复杂度 O(N)

```
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash_map=dict()
        for i,x in enumerate(nums):
            if target - x in hash_map:
                return [i,hash_map[target-x]]
            hash_map[x] = i
            

```
###  #15 Three Sum
array=[-1, 0, 1, 2, -1, -4] target= 0

Output: [
  [-1, 0, 1],
  [-1, -1, 2]
]

思路：

1. 暴力 三层循环，时间复杂度 O(N*N*N)

2. 枚举a, b, c = -(a+b) check if c in set 时间复杂度 O(N*N)
```
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        nums.sort()
        res = set()
        for i, v in enumerate(nums[:-2]):
            d = {}
            for x in nums[i+1:]:
                if x not in d:
                    d[-v-x] = 1
                else:
                    res.add((v, x, -v-x))
        return map(list,res)

```

3. sort, find : sort the array [-4, -1, -1, 0, 1, 2]

for loop enumerate(a):

if a + b + c > 0: c <- 移动

if a + b + c < 0: b -> 移动

 时间复杂度 O(N*N)
```
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums)<3:
            return []
        res = []
        nums.sort()
        for v in range (len(nums)-2):
            if v > 0 and nums[v] == nums[v-1]:
                continue
            l, r = v+1, len(nums)-1
            while l < r:
                s = nums[v] + nums[l] + nums[r]
                if s > 0:
                    r -= 1
                elif s < 0:
                    l += 1
                else:
                    res.append((nums[v], nums[l], nums[r]))
                    while l < r and nums[l] == nums[l+1]:
                        l = l + 1
                    while l < r and nums[r] == nums[r-1]:
                        r = r - 1
                    l += 1
                    r -= 1
        return res
```

