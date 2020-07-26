---
layout: post
title: 字节笔试题目整理
subtitle: Part 1：
tags: [algorithms]
comments: true
categories: ['algorithms']
---

参加了笔试之后觉得自己的算法部分还有许多的不足，应该继续学习。在此篇博客中整理一下我遇到的笔试题目，以此温故知新


## #1.简单数学题

题目描述：给定a,b两个数，求a/1+a/2+a/3+a/4+...+a/b的值

输入：a，b

输出：结果

--------------------
例子1:

输入：5 5

输出 10

回答：
```c++
int main(){
     int a,b;
     cin>>a >>b;
     int sum = 0;
     for(int i = 1; i<=b; i++){
         sum += a/i;
     }
     cout<<sum<<endl;
     return 0;

 }
```


## #2.单向链表，两个数问题

题目描述：输入是一行数字代表链表中的值，每个数字仅能出现两次。

输入：链表中的值

输出：链表中的值

--------------------
例子1:

输入：2 3 3 4 4 4 5

输出：2 2 3 3 4 4 5 5


```c++

struct ListNode{
    int val;
    ListNode *next;
    ListNode():val(0),next(nullptr) {}
    ListNode(int x):val(x),next(nullptr) {}
    ListNode(int x, ListNode *next):val(x),next(next){}
};

ListNode* get2LinkedList(ListNode *head){
    ListNode *prev = nullptr;
    ListNode *next = nullptr;
    ListNode *cur = head;
    while(cur != nullptr){
        next = cur->next;
        if(prev != nullptr){
            if(prev->val == cur->val){
                if(next!=nullptr && cur->val == next->val  ){
                    cur->next = next->next;
                }else{
                    continue;
                }
            }else{
                if(next!=nullptr && cur->val == next->val ) continue;
                else{
                    ListNode* tmp = new ListNode(cur->val);
                    cur->next = tmp;
                    tmp->next = next;
                }
            }
        }else{
            ListNode* tmp = new ListNode(cur->val);
            cur->next = tmp;
            tmp->next = next;
        }
        prev = cur;
        cur = next;
        
    }
    return head;
    

}

int main(){
    int a;
    ListNode* dummy = new ListNode(0);
    ListNode* cur = dummy;
    cout<<"please enter in a list of numbers"<<endl;
    cin >> a;
    ListNode* newNode = new ListNode(a);
    cur->next = newNode;
    cur = cur->next;
    while(cin.get() != '\n'){
        cin >> a;
        ListNode* newNode = new ListNode(a);
        cur->next = newNode;
        cur = cur->next;
    }
    cur = get2LinkedList(dummy->next);
    cout<<"the list we got"<<endl;;
    while(cur != nullptr){
        cout << cur->val<<" ";
        cur = cur->next;
    }
    return 0;

}


```


## #3.非零数组问题

题目描述：给定一个数组a，其子数组是a中连续的一段，[-2,1,3]中[-2], [1], [3], [-2,1], [1,3], [-2,1,3]都是子数组。
非零数组指的是一个数组的所有的子数组的和都不为0，要求找一个数组的非零数组的个数

输入：
a中元素的个数

数组a

输出：非零数组的个数

--------------------
例子1:

输入：3

  1，2，-3

输出：5




## #4.dp金币问题

题目描述：W在一个由方格组成的小岛上，最初没有金币，从岛的最左边方格开始向右边走，方格内的数字可以是正数也可以是负数，当W走到一个格子的时候，
做相应的加减法，如果W的兜里的金币为负数就被驱逐出岛。W有一次反转的机会，可以将任意方格中的值变为它的相反数，那么W最大的金币数是多少呢

输入：
n行，m列

矩阵a

输出：最大的金币数目

--------------------
例子1:

输入：3

  1，2，-3

输出：5
