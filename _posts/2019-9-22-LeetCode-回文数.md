---
layout:     post
title:      LeetCode-回文数
subtitle:   LeetCode/回文数问题-注意防止溢出
date:       2019-9-22
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---



## 题目

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。



## 示例

```
输入: 121
输出: true

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```



## 解决方案



### 方案一：使用字符串解决

​    最简单的方法应该是使用字符串解决，利用`int-string`转换，之后将string类型的数据反转后看是否和原来的类型相同，如果相同就是回文数，否则就不是。

```c++
class Solution {
public:
    bool isPalindrome(int x) {
      
        string order=to_string(x);
        string niorder=order;
        reverse(niorder.begin(),niorder.end());
        return  (order.compare(niorder)==0);
    }
};
```



### 方案二：分类求解

​     我们可以知道所有的负数不可能是回文数字，所有10的倍数都不是回文数字，剩下的数字只需要求其反转序列然后判断是否相同即可，为了防止溢出，我们采用`long`类型存储反转后的数字。

```c++
class Solution {
public:
    bool isPalindrome(int x) {
      
        if(x<0||x!=0&&x%10==0)
            return false;
        else{
            long temp=0;
            int xcopy=x;
            while(xcopy!=0){
                temp=temp*10+xcopy%10;
                xcopy/=10;
            }
            return temp==x;
        }
    }
};
```

​     也可以采用反转一半的方法来防止溢出。

```c++
class Solution {
public:
    bool isPalindrome(int x) {
      
        if(x<0||x!=0&&x%10==0)
            return false;
        else{
            int temp=0;
            while(x>temp){
                temp=temp*10+x%10;
                x/=10;
            }
        /*当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
         例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12
         revertedNumber = 123，
         由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
         */
            return temp==x||x==temp/10;
        }
    }
};
```



   