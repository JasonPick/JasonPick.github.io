---
layout: post
title: 贪心算法专题
subtitle: 经典题目整理-贪心算法与广度优先深度优先
tags: [algorithms, greedy]
comments: true
categories: ['algorithms']
---

## 贪心算法

适用于贪心算法的场景：

问题能分成子问题，子问题的最优解能推导出全局最优解

#122 买卖股票问题

每次可以持仓一股或者0股，可以买卖无数次

思路：

* DFS 买一股或者卖一股 O(2^N)

* 贪心： 如果后一天大于前一天，就在这一天买入 O(N)

    * 第一种方法找到低点和高点，profit更新低点和高点的差值
    
    * 第二种方法，只要循环，如果发现后一天大于前一天的值，加上这个差值
  

* DP 记录每一天的状态，Hode() 和利润 O(N)

[解决方案](https://github.com/JasonPick/recordings-in-Jan/blob/leecode/leecode/link/122.best_time_to_buy_and_sell.cpp)



## BFS广度优先搜索

应用场景：

在一个很大的集合中找到一个特定的结点


模版

```c++
queue<Node*> q;
unordered_set<Node*> visited;
q.push(root);
visited.insert(root);

while(!q.empty()){
  Node* node = q.top();
  q.pop();
  visited.insert(node);
  
  process(node);
   
  left_node = 子结点是否访问过;
  right_node = ;
  
  q.push(left_node);
  q.push(right_node);

}



```

## DFS深度优先搜索

模版

```c++
//recurssive
unordered_set<Node*> visited;
visited.insert(node);

for(Node* next_node: node_childeren){
  if(visited.find(next_node) == visited.end()){
    dfs(next_node,visited);
  }
}


//iterative
stack<Node*> stk;
stk.push(root);
while(!stk.empty()){
  Node* node = stk.top();
  stk.pop();
  visited.insert(node);
  
  process(node);
  
  nodes = generate_next_nodes(node);
  
  //注意栈是先入后出，先入右结点后入左结点
  stk.push(nodes);
}



```

## Practice - Leetcode

-### #102 二叉树的层序遍历

思路：

* BFS 标准的BFS遍历

* DFS helper函数，在函数中传入层的状态信息与结果res，进行递归


-### #104.111 最大深度最小深度

思路：

* DFS 直接递归

* BFS 和层序遍历一样，除了多了一个判别条件，如果左子结点和右子结点为空直接返回层数。

**104 solution**
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1+max(maxDepth(root->left),maxDepth(root->right));
  
    }
};
```
**111 solution**

```c++

class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        int left  = minDepth(root->left);
        int right = minDepth(root->right);
        return (left == 0 || right == 0) ?left+right+1:min(left,right)+1;


    }
};

class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> q;
        q.push(root);
        int i = 0;
        while(!q.empty()){
            i++;
            int sz = q.size();
            for(int j = 0; j<sz;j++){
                TreeNode* node = q.front();
                q.pop();
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
                if(node->left ==nullptr && node->right == nullptr) return i;
            }
        }
        return -1;


    }
};

```

-### #22生成括号

思路：

* 归纳法：n = 1, n = 2, n = k

* 递归法：每个位置都有两种可能 _(_ 或者 _)_,字符串长度2n，O(2^(2n))

* 改进的递归法:

  * 首先去除局部不合法比如 _）_不能出现在_(_之前
  
  * 跟踪使用的左括号的数目和右括号的数目

```c++
class Solution {
public:
    vector<string> v;
    vector<string> generateParenthesis(int n) {      
        track("",0,0,n);
        return v;
    }
    void track(string cur,int left, int right, int max){
        if(cur.length() == max*2) {
            v.push_back(cur);
            return;
        }
        
        if(left<max){
            track(cur+'(',left+1,right,max);
        }
        if(right<left){
            track(cur+')',left,right+1,max);
        }
            
    }
};
```
