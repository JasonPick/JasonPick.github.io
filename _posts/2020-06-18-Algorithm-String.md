---
layout: post
title: 字符串专题
subtitle: Part 1：
tags: [algorithms, string]
comments: true
categories: ['algorithms']
---

# 字符串


字符串定义为字符数组。
![](https://media.geeksforgeeks.org/wp-content/uploads/finnnal.png)


## C++ 中的string类



**string 与 char[]**


* 字符数组只是字符数组，可以以空字符结尾。字符串是一个类，用于定义表示为字符流的对象。
 

* 字符数组的大小必须静态分配，如果需要，则无法在运行时分配更多内存。如果使用字符数组，则会浪费未使用的分配内存。对于字符串，内存是动态分配的。可以在运行时按需分配更多内存。由于没有预先分配内存，因此不会浪费任何内存。


* 字符数组的实现比字符串要快。
 

* 字符数组没有提供很多内置函数来操作字符串。字符串类定义了许多功能，这些功能允许对字符串进行多种操作。


## 字符串的操作


* **getline()** ：该功能用于将用户输入的字符流存储在对象存储器中

* **push_back()** ：在字符串的末尾加入字符

* **pop_back()** ：移除字符串末尾的字符

  ~~~
    #include<iostream> 
    #include<string> // for string class 
    using namespace std; 
    int main() 
    { 
        // Declaring string 
        string str; 

        // Taking string input using getline() 
        // "geeksforgeek" in givin output 
        getline(cin,str); 

        // Displaying string 
        cout << "The initial string is : "; 
        cout << str << endl; 

        // Using push_back() to insert a character 
        // at end 
        // pushes 's' in this case 
        str.push_back('s'); 

        // Displaying string 
        cout << "The string after push_back operation is : "; 
        cout << str << endl; 

        // Using pop_back() to delete a character 
        // from end 
        // pops 's' in this case 
        str.pop_back(); 

        // Displaying string 
        cout << "The string after pop_back operation is : "; 
        cout << str << endl; 

        return 0; 

    } 
  ~~~

