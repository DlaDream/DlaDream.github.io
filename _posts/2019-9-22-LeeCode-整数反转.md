---
layout:     post
title:      LeetCode-整数反转
subtitle:   LeetCode/整数反转问题-使用long处理int溢出问题
date:       2019-9-22
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---

## 题目

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

> 注意:
>
> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为:
>
> ![](https://i.loli.net/2019/09/22/OtZ9z2hBobUMR1m.png)
>
> 请根据这个假设，如果反转后整数溢出那么就返回 0。



## 示例

```
输入: 123
输出: 321
```

```
输入: -123
输出: -321
```

```
输入: 120
输出: 21
```



## 解决方案



### 方案一：使用long类型进行溢出判断

   我在评论区看到了使用`long`类型进行判断是否溢出，我觉得十分优秀，就写了下来。

```c++
class Solution {
public:
    int reverse(int x) {
        long result=0;
        long temp=x;
        int flag=temp<0?-1:1;
        temp=temp*flag;
        while(temp!=0){
            result=result*10+temp%10;
            temp/=10;
        }
        result*=flag;
        if(result>INT32_MAX||result<INT32_MIN)
            return 0;
        return result;
    }
};
```



### 方案二：使用字符串进行反转

​    刚开始时我首先想到的是使用`int-string`格式相互转换的方式来写，我想在`string`格式进行判断是否溢出，但是我发现那样会十分的麻烦，后来我在评论区发现，可以使用`long`类型进行转换，无论是否溢出，先用`long`类型存下来，然后再进行大小判断。

```c++
class Solution {
public:
    int reverse(int x) {
    try{
        int result=0;
        int temp=x;
        int flag=temp<0?-1:1;
        temp=temp*flag;
        while(temp!=0){
            result=result*10+temp%10;
            temp/=10;
        }
        result*=flag;
        return result;
        }catch(exception e){
            return 0;
        }
    }
};
```

