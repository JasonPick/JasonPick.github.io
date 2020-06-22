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


 {% highlight c++ linenos %}
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
  {% endhighlight %}

* **capacity()** ：string的容量，通常为了效率是大于size的

* **resize()**  ：改变string的size，增加或者减少

* **length()** ：string 的length

* **shrink_to_fit()** ：减少string的容量使其等于size，当不再有元素加入的时候适用

* **迭代器 begin() end() rbengin() rend()** r是指 reverse_iterator

* **copy("char array", len, pos)** ： 函数将子字符串复制到目标字符数组中。它需要3个参数，目标char数组，要复制的长度和字符串中的开始位置。

* **swap()** ：交换两个string


{% highlight c++ linenos %}

  // C++ code to demonstrate the working of 
  // copy() and swap() 
  #include<iostream> 
  #include<string> // for string class 
  using namespace std; 
  int main() 
  { 
      // Initializing 1st string 
      string str1 = "geeksforgeeks is for geeks"; 
      // Declaring 2nd string 
      string str2 = "geeksforgeeks rocks"; 
      // Declaring character array 
      char ch[80]; 
      // using copy() to copy elements into char array 
      // copies "geeksforgeeks" 
      str1.copy(ch,13,0);
      // Diplaying char array 
      cout << "The new copied character array is : "; 
      cout << ch << endl << endl; 
      // Displaying strings before swapping 
      cout << "The 1st string before swapping is : "; 
      cout << str1 << endl; 
      cout << "The 2nd string before swapping is : "; 
      cout << str2 << endl; 
      // using swap() to swap string content 
      str1.swap(str2); 
      // Displaying strings after swapping 
      cout << "The 1st string after swapping is : "; 
      cout << str1 << endl; 
      cout << "The 2nd string after swapping is : "; 
      cout << str2 << endl; 
      return 0; 

  } 
 The new copied character array is : geeksforgeeks

The 1st string before swapping is : geeksforgeeks is for geeks
The 2nd string before swapping is : geeksforgeeks rocks
The 1st string after swapping is : geeksforgeeks rocks
The 2nd string after swapping is : geeksforgeeks is for geeks
  {% endhighlight %}


{% highlight c++ linenos %}

// C++ program to demonstrate various function string class 
#include <bits/stdc++.h> 
using namespace std; 
  
int main() 
{ 
    // various constructor of string class 
    // initialization by raw string 
    string str1("first string"); 
   
   // initialization by another string 
    string str2(str1); 
   
   // initialization by character with number of occurrence 
    string str3(5, '#'); 
   
   // initialization by part of another string 
    string str4(str1, 6, 6); //    from 6th index (second parameter) 
                             // 6 characters (third parameter)
                             
   // initialization by part of another string : iteartor version 
    string str5(str2.begin(), str2.begin() + 5); 
    cout << str1 << endl; 
    cout << str2 << endl; 
    cout << str3 << endl; 
    cout << str4 << endl; 
    cout << str5 << endl; 
    
   //  assignment operator 
    string str6 = str4; 
  
   // clear function deletes all character from string 
   str4.clear(); 
  
   //  both size() and length() return length of string and 
   //  they work as synonyms 
   int len = str6.length(); // Same as "len = str6.size();" 
  
   cout << "Length of string is : " << len << endl; 
  
   // a particular character can be accessed using at / 
   // [] operator 
   char ch = str6.at(2); //  Same as "ch = str6[2];" 
  
  
   cout << "third character of string is : " << ch << endl; 
  
   //  front return first character and back returns last character 
   //  of string 
  
   char ch_f = str6.front();  // Same as "ch_f = str6[0];" 
   char ch_b = str6.back();   // Same as below 
                               // "ch_b = str6[str6.length() - 1];" 
  
   cout << "First char is : " << ch_f << ", Last char is : "
         << ch_b << endl; 
  
   // c_str returns null terminated char array version of string 
   const char* charstr = str6.c_str(); 
   printf("%s\n", charstr); 
  
   // append add the argument string at the end 
   str6.append(" extension"); 
   //  same as str6 += " extension" 
  
   // another version of append, which appends part of other 
   // string 
   str4.append(str6, 0, 6);  // at 0th position 6 character 
  
   cout << str6 << endl; 
   cout << str4 << endl; 
  
   //  find returns index where pattern is found. 
   //  If pattern is not there it returns predefined 
   //  constant npos whose value is -1 
  
   if (str6.find(str4) != string::npos) 
       cout << "str4 found in str6 at " << str6.find(str4) 
             << " pos" << endl; 
   else
       cout << "str4 not found in str6" << endl; 
  
   //  substr(a, b) function returns a substring of b length 
   //  starting from index a 
   cout << str6.substr(7, 3) << endl; 
  
   //  if second argument is not passed, string till end is 
   // taken as substring 
   cout << str6.substr(7) << endl; 
  
   //  erase(a, b) deletes b characters at index a 
   str6.erase(7, 4); 
   cout << str6 << endl; 
  
   //  iterator version of erase 
   str6.erase(str6.begin() + 5, str6.end() - 3); 
   cout << str6 << endl; 
  
   str6 = "This is a examples"; 
  
   //  replace(a, b, str)  replaces b characters from a index by str 
   str6.replace(2, 7, "ese are test"); 
  
   cout << str6 << endl; 
  
   return 0; 
} 

  {% endhighlight %}

~~~c++
// C++ program to demonstrate uses of some string function 
#include <bits/stdc++.h> 
using namespace std; 

// this function returns floating point part of a number-string 
string returnFloatingPart(string str) 
{ 
	int pos = str.find("."); 
	if (pos == string::npos) 
		return ""; 
	else
		return str.substr(pos + 1); 
} 

// This function checks whether a string contains all digit or not 
bool containsOnlyDigit(string str) 
{ 
	int l = str.length(); 
	for (int i = 0; i < l; i++) 
	{ 
		if (str.at(i) < '0' || str.at(i) > '9') 
			return false; 
	} 
	// if we reach here all character are digits 
	return true; 
} 

// this function replaces all single space by %20 
// Used in URLS 
string replaceBlankWith20(string str) 
{ 
	string replaceby = "%20"; 
	int n = 0; 

	// loop till all space are replaced 
	while ((n = str.find(" ", n)) != string::npos ) 
	{ 
		str.replace(n, 1, replaceby); 
		n += replaceby.length(); 
	} 
	return str; 
} 

// driver function to check above methods 
int main() 
{ 
	string fnum = "23.342"; 
	cout << "Floating part is : " << returnFloatingPart(fnum) 
		<< endl; 

	string num = "3452"; 
	if (containsOnlyDigit(num)) 
		cout << "string contains only digit" << endl; 

	string urlex = "google com in"; 
	cout << replaceBlankWith20(urlex) << endl; 

	return 0;	 
} 

~~~


## 字符串习题列表

* 简单问题


{: .box-warning}

 1.大写变小写 [709. To Lower Case ](https://leetcode.com/problems/to-lower-case/)  
 2.回退字符串对比[844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)  
 3.反转字符串[344. Reverse String](https://leetcode.com/problems/reverse-string/)  
 4.是否是回文字符串[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)  
 5.反转字符串[344. Reverse String](https://leetcode.com/problems/reverse-string/)  



* 回文问题


{: .box-warning}

 1.最长回文子串，难度中等 [5. Longest Palindromic Substring ](https://leetcode.com/problems/longest-palindromic-substring/)  
 2.最短回文串，难度困难[214. Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)  
 3.寻找最近的回文数，难度困难[564. Find the Closest Palindrome](https://leetcode.com/problems/find-the-closest-palindrome/)  
 

* 子串问题


{: .box-warning}

 1.最小覆盖子串，难度困难 [76. Minimum Window Substring ](https://leetcode.com/problems/minimum-window-substring/)  
 2.不同的子序列，难度困难[115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)  
 3.最长特殊序列II，难度中等[522. Longest Uncommon Subsequence II](https://leetcode.com/problems/longest-uncommon-subsequence-ii/)  
 4.按字典序排在最后的子串，难度困难[1163. Last Substring in Lexicographical Order](https://leetcode.com/problems/last-substring-in-lexicographical-order/)  


* 表达式求值问题


{: .box-warning}

 1.整数转罗马数字，难度中等 [ 12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)  
 2.字符串相乘，难度中等[43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)  
 3.复数乘法，难度中等[537. Complex Number Multiplication](https://leetcode.com/problems/complex-number-multiplication/)  
 4.基本计算器IV，难度困难[770. Basic Calculator IV](https://leetcode.com/problems/basic-calculator-iv/)  
 


* 语法分析问题


{: .box-warning}

 1.最长的有效括号，难度困难 [ 32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)  
 2.有效数字，难度困难[65. Valid Number](https://leetcode.com/problems/valid-number/)  
 3.简化路径，难度中等[71. Simplify Path](https://leetcode.com/problems/simplify-path/)  
 4.Lisp语法分析，难度困难[736. Parse Lisp Expression](https://leetcode.com/problems/parse-lisp-expression/)  
 

* 字符串转换和交换问题


{: .box-warning}

 1.通配符匹配，难度困难 [ 44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)  
 2.特殊的二进制排序，难度中等[791. Custom Sort String](https://leetcode.com/problems/custom-sort-string/)  
 3.有序队列，难度困难[899. Orderly Queue](https://leetcode.com/problems/orderly-queue/)  
 

* 验证IP地址问题


{: .box-warning}

 1.复原IP地址，难度中等 [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)  
 2.验证IP地址，难度中等[468. Validate IP Address](https://leetcode.com/problems/validate-ip-address/)  
 

 
* 模式匹配问题


{: .box-warning}

 1.查找和替换模式，难度中等 [890. Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern/)  
 2.戳印序列，难度困难[936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence/)  
 
 
* 正则表达式问题（匹配问题）


{: .box-warning}

 1.正则表达式匹配，难度困难 [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)  
 

 
* 递归/回溯思想求解


{: .box-warning}

 1.括号生成，难度中等 [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)  
 2.模糊坐标，难度中等 [816. Ambiguous Coordinates](https://leetcode.com/problems/ambiguous-coordinates/)  
 
 
* 特定变换


{: .box-warning}

 1.z变换，难度中等 [6. ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)  


 
* 字符串插入、删除和排序


{: .box-warning}

 1.字母异位分组，难度中等 [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)  
 2.编辑距离，难度中等 [72. Edit Distance](https://leetcode.com/problems/edit-distance/)  
 3.单词接龙II，难度中等 [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)  

 
* 字符串细节问题


{: .box-warning}

 1.文本左右对齐，难度困难 [68. Text Justification](https://leetcode.com/problems/text-justification/)  
 2.花括号展开II，难度困难 [1096. Brace Expansion II](https://leetcode.com/problems/brace-expansion-ii/)  
 

 

 

 
## Solution


* 简单问题


{: .box-error}

 1.大写变小写 [709. To Lower Case ](https://leetcode.com/problems/to-lower-case/)  
 2.回退字符串对比[844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)  
 3.反转字符串[344. Reverse String](https://leetcode.com/problems/reverse-string/)  
 4.是否是回文字符串[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)  
 5.反转字符串[344. Reverse String](https://leetcode.com/problems/reverse-string/)  


709.大写变小写

思路：

* for loop循环,逐一比较字符c  
* 关键点 
``` c++
		if (c >= 'A' && c <= 'Z') c += 32;
```
 
844. 回退字符串对比

思路：

* 使用指针法，从末尾开始扫描有两个状态变量，skip 和 pointer;   
  如果s_pointer指向一个‘#’符号那么，skip++,pointer--;  
  如果skip >0，skip-- , pointer--;  
  否则检查当前pointer的字符是否相等;  
  然后检查当前pointer是否都 >=0,不想等意味着长度不同   
  

344. 反转字符串

思路：

* 双指针法，一个指着头部，一个指着尾部，swap( char)


125.是否是回文字符串

思路：

* 因为我们只关心字符，不关心标点符号，我们先将所有字母或者数字放入res string中，将大写字母转化成小写字母；  
  接着使用双指针法，从两头进行对比，如果不想等则false



344.反转字符串

思路：

* swap()


=====================================================
待续



* 回文问题


{: .box-error}

 1.最长回文子串，难度中等 [5. Longest Palindromic Substring ](https://leetcode.com/problems/longest-palindromic-substring/)  
 2.最短回文串，难度困难[214. Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)  
 3.寻找最近的回文数，难度困难[564. Find the Closest Palindrome](https://leetcode.com/problems/find-the-closest-palindrome/)  
 
 

* 子串问题


{: .box-error}

 1.最小覆盖子串，难度困难 [76. Minimum Window Substring ](https://leetcode.com/problems/minimum-window-substring/)  
 2.不同的子序列，难度困难[115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)  
 3.最长特殊序列II，难度中等[522. Longest Uncommon Subsequence II](https://leetcode.com/problems/longest-uncommon-subsequence-ii/)  
 4.按字典序排在最后的子串，难度困难[1163. Last Substring in Lexicographical Order](https://leetcode.com/problems/last-substring-in-lexicographical-order/)  


* 表达式求值问题


{: .box-erroe}

 1.整数转罗马数字，难度中等 [ 12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)  
 2.字符串相乘，难度中等[43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)  
 3.复数乘法，难度中等[537. Complex Number Multiplication](https://leetcode.com/problems/complex-number-multiplication/)  
 4.基本计算器IV，难度困难[770. Basic Calculator IV](https://leetcode.com/problems/basic-calculator-iv/)  
 


* 语法分析问题


{: .box-error}

 1.最长的有效括号，难度困难 [ 32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)  
 2.有效数字，难度困难[65. Valid Number](https://leetcode.com/problems/valid-number/)  
 3.简化路径，难度中等[71. Simplify Path](https://leetcode.com/problems/simplify-path/)  
 4.Lisp语法分析，难度困难[736. Parse Lisp Expression](https://leetcode.com/problems/parse-lisp-expression/)  
 

* 字符串转换和交换问题


{: .box-error}

 1.通配符匹配，难度困难 [ 44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)  
 2.特殊的二进制排序，难度中等[791. Custom Sort String](https://leetcode.com/problems/custom-sort-string/)  
 3.有序队列，难度困难[899. Orderly Queue](https://leetcode.com/problems/orderly-queue/)  
 

* 验证IP地址问题


{: .box-warning}

 1.复原IP地址，难度中等 [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)  
 2.验证IP地址，难度中等[468. Validate IP Address](https://leetcode.com/problems/validate-ip-address/)  
 

 
* 模式匹配问题


{: .box-error}

 1.查找和替换模式，难度中等 [890. Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern/)  
 2.戳印序列，难度困难[936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence/)  
 
 
* 正则表达式问题（匹配问题）


{: .box-error}

 1.正则表达式匹配，难度困难 [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)  
 

 
* 递归/回溯思想求解


{: .box-error}

 1.括号生成，难度中等 [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)  
 2.模糊坐标，难度中等 [816. Ambiguous Coordinates](https://leetcode.com/problems/ambiguous-coordinates/)  
 
 
* 特定变换


{: .box-error}

 1.z变换，难度中等 [6. ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)  


 
* 字符串插入、删除和排序


{: .box-error}

 1.字母异位分组，难度中等 [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)  
 2.编辑距离，难度中等 [72. Edit Distance](https://leetcode.com/problems/edit-distance/)  
 3.单词接龙II，难度中等 [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)  

 
* 字符串细节问题


{: .box-error}

 1.文本左右对齐，难度困难 [68. Text Justification](https://leetcode.com/problems/text-justification/)  
 2.花括号展开II，难度困难 [1096. Brace Expansion II](https://leetcode.com/problems/brace-expansion-ii/)  
 

 
