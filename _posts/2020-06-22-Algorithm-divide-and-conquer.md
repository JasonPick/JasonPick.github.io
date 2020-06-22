--
--


这里我们来讨论递归和分治算法

## 概述

递归的分治由三部分组成


* 划分问题： 将问题划分为子问题

* Conquer：通过递归调用子问题直到子问题得到解决

* Combine：子问题解决后合并，找到问题的解决方案

### 分治的应用

* **二分搜索**

在每个步骤中，算法将输入元素 x 与数组中中间元素的值进行比较。如果值匹配，则返回中间的索引。否则，如果 x 小于中间元素，
则算法会重复中间元素的左侧，否则对中间元素的右侧重复出现 

* **快速排序**

该算法选择一个pivot，重新排列数组元素，使小于选取的pivot的所有元素移动到pivot的左侧，
并且所有较大的元素移动到右侧。最后，算法递归地对枢轴元素左右两色的子数组进行排序 

* **合并排序**

该算法将数组分为两半，递归地对其进行排序，
最后合并两个排序的半部分 



## Recursion 模板 
***记住***
``` python
def recursion(self,level, param1, param2):
    # 终止条件
    if level > max_level:
        return 
    
    # 在本层进行操作
    process_data()

    # drill down
    self.recursion(level+1, param1, param2)
    
    # 对这一层的操作改变了参数返回原来的状态
    reverse_state(level)
```
## Divide & Conquer 


### 将问题一分为二 -- 模板
```
def devide_con(self, problem, p1, p2):
    if problem is None:
        return 
    subproblems = split_problem(problem, data)
```


## Practice - Leetcode
	

###  #50 Power n 

pow(x, n)

方法：

1. 调用库函数 O(1)

2. 暴力 O(N)

3. 分治 

x, ..., x

even: y = x^(n/2), res = y * y

odd: y = x^(n/2), res = x * y * y

时间复杂度 O(logN)
```python
    def myPow(self, x: float, n: int) -> float:
        if not n:
            return 1
        if n < 0:
            return 1/self.myPow(x, -n)
        if n % 2 :
            return x * self.myPow(x,n-1)
        return self.myPow(x*x, n/2)
```
c++ 版本注意n可能为long long类型，要转化

4. 位运算

```python
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            x = 1/x
            n = -n
        pow_ = 1
        while n:
            if n & 1:
                pow_ *= x
            x *= x
            n >>= 1
        return pow_
```



###  #169 众数

题目描述：给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1:

输入: [3,2,3]

输出: 3

方法：

   1.暴力 for x,for count(x),return count(x)>n/2;
   
   O(N*2)
   
   2.使用map计数：O(N)
   {x : count(x)}
   
   ```
   for item in nums:
   	map[item]++;
   ```
   ``` c++
   class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int result;
        std::map<int, int> countMap;
        for (auto item:nums){
            if (countMap.find(item) != countMap.end())
                countMap[item]++;
            countMap.insert(std::make_pair(item,1));
        }
        map<int,int>::iterator iter = countMap.begin();
        while(iter != countMap.end()) {
        if(iter->second > nums.size()/2) 
            result = iter->first;
        iter++;
        }
        return result;
        
    }
};
   ```
  
   
   3.Sort:   O(NlogN)
   [1, 2 , 3, 3, 3, 3]
   从头到尾开始看重复多少次>n/2
   
 
   
   4.分治 O(NlogN)
   [1, 1, 1, 0, 2];
   左边的众数 右边的众数
   
   ```
   if left == right://left
   return count(L) > count(R);
   
   ```

   ``` c++
   class Solution {
public:
    int majorityElement(vector<int>& nums) {
        return countMajority(nums,0,nums.size()-1);
        
    }
private:
    int countMajority(vector<int>& nums,int L,int R){
        if(L==R)
            return nums[L];
        int mid = L+(R-L)/2;
        int left = countMajority(nums,L,mid);
        int right = countMajority(nums,mid+1,R);
        if (left == right)
            return left;
        int leftCount = Count(nums,left,L,mid);
        int rightCount = Count(nums,right,mid+1,R);
        return (leftCount>rightCount ? left: right);
        
    }
    int Count(vector<int>& nums,int num,int L,int R){
        int count = 0;
        for(auto item:nums){
            if (item == num)
                ++count;
        }
                
        return count;
    }
        
};
   ```
 

