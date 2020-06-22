---
layout: post
title: 位运算专题
subtitle: 经典题目
tags: [algorithms, bit]
comments: true
categories: ['algorithms']
---

## 位运算
### # 操作
与：&  或： ｜  异或： ^   非：  ～  左移： <<   右移：  >>（有符号的时候左边添加符号位0或者1）

 有用的公式
x ^ 1 = ~ x;

x & 1 == 1 OR 0 判断奇偶（x % 2 == 1）

x & (x-1) -> x, 清除x最低位的1

x & ~x 得到最低位的1
### # 更多
(1)获取x的第n位值 （x >> n）&1

(2)将第n位设置位1 x｜（1 << n）

(3)第n位设置位0 x & (~(1 << n))

(4)最右边n位设置位0 x & (~ 0 << n)

(5)第n位的幂值   x & (1 << (n-1))

(6)最高位至n位设置位0 x & ( (1 << n )- 1)

(7)将n位至0位清零 x & (~((1 << (n+1))-1))
## Practice - Leetcode
	

### #191 数1的个数
思路：
（1）if（x % 2 == 1）count++;时间复杂度O（N）
```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        uint32_t flag = 1;
        while(flag){
            if (n & flag)
                count++;
            flag <<= 1;
        }
      
        return count;
        
    }
};
```

（2）x = x & (x-1).

 while x:
     count ++;
     x = x & (x-1).
     ```
     class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while(n){
            n &= ( n-1);
            count ++;
            
        }
        return count;
        
    }
};
     ```
   



### #338 比特位计数
思路：
（1）mod and count

 (2) log2 -> int
 
 (3) 位运算 x & (x - 1) 理论：2^n 有且只有一个1
```
 ```
    class Solution:
    def countBits(self, num: int) -> List[int]:
        bits = [0 for i in range(num+1)]
        for i in range(1,num+1):
            bits[i] += bits[i & (i-1)] + 1
        return bits
    ```
```

            res[i] = res[i >>1] +(i & 1);

```
### #152 N皇后问题
位运算来解决n皇后问题，是最快的
常规解法
```
class Solution {
public:

     
    int totalNQueens(int n) {
        vector<bool> col(n,0);
        //记录撇是否已经摆放
        vector<bool> pie(2*n-1,0);
        //记录拿是否已经摆放
        vector<bool> na(2*n-1,0);
   
        return dfs(n,0,col,pie,na);
        
        
    }
    
private:
    int dfs(int n, int index, vector<bool> col,vector<bool> pie,vector<bool> na){
        int res = 0;
        if (index == n)
            return 1;
        
        //开始在index行的第i列摆放皇后
        for (int i=0; i<n; i++){
            if (!col[i] && !pie[i+index] && !na[i - index + n-1]){
                //递归
                col[i] = true;
                na[i - index + n-1] = true;
                pie[index + i] = true;
                res += dfs(n, index +1,col,pie,na);
                //恢复现场
                col[i] = false;
                na[i - index +n-1] = false;
                pie[i+index] = false;
                
            }
            
        }
```
位运算解法
```
class Solution {
public:
    
    int totalNQueens(int n) {
        if (n<1) return 0;
        dfs(0,0,0,0,n);
        return count;

    }
private:
    int count = 0;
    void dfs(int row, int col, int pie, int na, int n){
        //终止条件
        if (row >= n) {
            count++;
            return;
        }
        //获得所有可放的位置
        int bits = (~(col|pie|na) & ((1 << n) - 1 ));
        //循环
        while(bits > 0){
            int p = bits & -bits;
            dfs (row + 1, col|p, (pie|p)<<1, (na|p)>>1, n);
            bits &= bits - 1;
        }
    }
};
```
