--
--
## Hash Table & Functions & Collisions
- ### Hash Collision
lies 映射同一地址
foes   
![](https://www.cs.auckland.ac.nz/software/AlgAnim/fig/dir_acc_table.gif)

- ### 解决办法：拉链表头解决碰撞(待补充)
![](https://www.cs.auckland.ac.nz/software/AlgAnim/fig/dir_acc_lists.gif)

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
Hash Map: Search -> O(1),    unsorted/disordered

Tree Map: Search -> O(logn), sorted

具体问题具体分析：当对于时间复杂度要求很高的时候选择Hash Map，如果要求有序性则喧杂Tree Map。 （Python的dict使用hash map）


## Practice - Leetcode
	

###  #242 Valid Anagram 
'tar' -> 'rat'

'cat' -> 'atc'

'anagram' -> 'nagamar'

方法：

1. 排序 对string进行排序看排序过后的结果是否一致，时间复杂度 O(NlogN)

'rat' -> 'art'

'tar' -> 'art'


```
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
 ```
2. Map and count
construct a dict {letter: count} 时间复杂度 O(N)

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

方法：

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

方法：

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

