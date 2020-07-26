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


## #5.小房子看房顶问题

题目描述：有n栋小房子拍成一行，假设第n栋房子的高度是正整数a[i],小明站在当前房子的顶部只可以看到左右小于等于当前高度的房子，直到左右出现更高的房子，
举个例子，4栋房子a,b,c,d高度分别是1,4,3,3，那么在a顶部无法看到其他房子。。。


输入：
第一行t，接下来有t个样例，

每个样例的第一行是n，代表n栋房子

下面一行是房子的高度


输出：每个样例输出一行，一个数组，站在第i个房子上可以看到左右房子的数量

--------------------
例子1:

输入：1

   4

  1，4，3，3

输出：0，3，1，1




## #6.优惠券问题

题目描述：有n张优惠券，用来买m个商品，只要优惠券面值不大于商品价值就可以用，用完不收回，求购买这些商品所花的最少钱数。

输入：
第一行是两个数n，m，表示优惠券数和商品数

第二行有n个数，表示优惠券面值

第三行有m个数，表示商品价值（均不保证按大小排序）


输出：你最少所花钱数

--------------------
例子1:

输入：3 4

   50 100 200

   99 199 200 300

输出：248


## #7.折木棍问题

题目描述：在你的面前从左到右摆放着n根长短不一的木棍，你每次可以折断一根木棍，并将折断后得到的两根木棍一左一右放在原来的位置（即若原木棍有左邻居，则两根新木棍必须放在左邻居的右边，若原木棍有右邻居，新木棍必须放在右邻居的左边，所有木棍保持左右排列）。折断后的两根木棍的长度必须为整数，且它们之和等于折断前的木棍长度。你希望最终从左到右的木棍长度单调不减，那么你需要折断多少次呢？

输入：第一行是一个数n，表示开始时有多少根木棍(1<=n<=3000)

  第二行有n个数，从第1个到第n个分别表示从左到右的木棍长度。


输出：输出一行，一个数，你最少所需的折断木棍的次数x


--------------------
例子1:

输入：5

   3 5 13 9 12

输出：1

