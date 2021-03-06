---
layout:     post
title:      LeetCode-最长公共前缀
subtitle:   LeetCode/各种方法
date:       2019-9-22
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---



## 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。



## 示例

```
输入: ["flower","flow","flight"]
输出: "fl"

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

所有输入只包含小写字母 `a-z` 。



## 解决方案

### 方案一：垂直扫描    

​     首先考虑特殊情况，当一个数组为空时肯定返回空；当数组内只有一个元素时返回这个元素；当数组有大于等于两个元素的时候就需要进行遍历判断。

​    其实我更愿意称其为暴力解法，首先我们找出数组中长度最短的那个，然后依次判断其它元素中的第i位是否和最短元素的第i位相同，一直到出现一个不相同或者将最短元素遍历完就结束了。

```c++
class Solution {
public:

    bool max(string a,string b){
        return a.size()<b.size();
    }
    string longestCommonPrefix(vector<string>& strs) {

        string result="";
        //处理特殊情况
        if(strs.size()==0)
            return result;
        else if(strs.size()==1)
            return strs[0];
        else{
            bool flag;
            //求出最短的那个字符串
            sort(strs.begin(),strs.end(),[](string x,string y){return x.length()<y.length();});
            string minLength=strs[0];
            //遍历
            for(int i=0;i<minLength.size();i++){
                flag=true;
                for(int j=1;j<strs.size();j++){
                    if(strs[j].at(i)!= minLength.at(i)){
                        flag=false;
                    }

                }
                if(flag)
                    result+=minLength.at(i);
                else
                    return result;

            }
            return result;

        }
    }
};
```

### 方案二：水平扫描法

​      这种方法和我的方法差不多，但是之所以写这种方法是因为：官方的这种解法很巧妙的将我的解法颠倒了过来，它**不是从某个字符串的第一个开始比，相反的它先将第一个字符串作为公共子串进行验证**，如果不是那么就进行截取，后继续进行判断，直到出现了空串或者遍历完就结束了。

![](https://i.loli.net/2019/09/22/wnEAYyePctg4RSB.png)

```c++
class Solution {
public:

    bool max(string a,string b){
        return a.size()<b.size();
    }
    string longestCommonPrefix(vector<string>& strs) {


        if(strs.size()==0)
            return "";
        else if(strs.size()==1)
            return strs[0];
        else{
            bool flag;
            sort(strs.begin(),strs.end(),[](string x,string y){return x.length()<y.length();});
            string minLength=strs[0];
            for(int i=1;i<strs.size();i++){
                while(strs[i].find(minLength)!=0){
                    minLength=minLength.substr(0,minLength.size()-1);
                    if(minLength.size()==0) return "";
                }

            }
            return minLength;

        }
    }
};
```



### 方案三：分治法

​      我们将求(S1,S2,S3,S4.....Sn)的公共前缀转化为求(S1,S2,S3,Smid)和(Smid+1.....Sn)的公共前缀，其中mid=(left+right)/2。其实就是像利用二分排序的方法，我们不断的递归缩小范围，一直到递归的终止条件：left=right，即只有一个元素的时候，那么该元素与自己的最大公共前缀就是自己。

![](https://i.loli.net/2019/09/23/I2irJGPQd1fHURn.png)

  就像上面的这幅图片一样，分而治之。

```c++
class Solution {
public:

    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size()==0) return "";
        return longestCommonPrefix(strs,0,strs.size()-1);
    }

    string longestCommonPrefix(vector<string> &strs,int left,int right){
        if(left==right)
            return strs[left];
        else{
            int mid=(left+right)/2;
            string left_com=longestCommonPrefix(strs,left,mid);
            string right_com=longestCommonPrefix(strs,mid+1,right);
            return commonMax(left_com,right_com);
        }
    }

    string commonMax(string a,string b){
        int min=a.size()>b.size()?b.size():a.size();
        string result="";
        for(int i=0;i<min;i++){
            if(a.at(i)!=b.at(i))
                break;
            result+=a.at(i);
        }
        return result;
    }
};
```



### 方案四：二分查找法

​       首先求出公共前缀有可能的最大长度，也就是整个数组中最短的那个字符串的长度记为minLength，然后我们将第一个元素分为两部分:`0~minLength/2`和`minLength/2+1~minLength`，先在判断第一部分是否在每个元素中都存在，有以下两种情况：

- 都存在：则`0~minLength/2`是最大前缀的组成部分，判断`0~minLength/2+1`是否是最大前缀，依次向后移动一个元素进行判断
- 不存在：则`0~minLength/2`一定不是最大公共前缀，但不排除里面含有最大前缀，所以我们求`0~minLength-1`是否为最大前缀，依次向前移动

![](https://i.loli.net/2019/09/23/YdHlPOM32Wh7tnw.png)



```c++
class Solution {
public:

    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size()==0) return "";
        int min=INT32_MAX;
        int i=0;
        //求出最短长度
        while(i<strs.size()){
            min=min<strs[i].size()?min:strs[i].size();
            i++;
        }

        string result="";
        int low=0,high=min;
        while(low<=high){
            int mid=(low+high)/2;
            if(isCommon(strs,strs[0],mid))
                low=mid+1;
            else
                high=mid-1;
        }
        return strs[0].substr(0,high);
    }

    bool isCommon(vector<string>& strs,string b,int k){
        for(int i=0;i<strs.size();i++){
            if(strs[i].find(b.substr(0,k))!=0)
                return false;
        }

        return true;
    }

};
```

