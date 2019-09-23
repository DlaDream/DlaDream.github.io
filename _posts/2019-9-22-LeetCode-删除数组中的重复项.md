---
layout:     post
title:      LeetCode-删除数组中的重复项
subtitle:   LeetCode/从5.07%到97.65%需要做些什么
date:       2019-9-22
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---



## 题目

给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**原地**修改输入数组并在使用 O(1) 额外空间的条件下完成。



## 示例

```
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

```



## 解决方案

### 方案一：常规方法

​      利用原来的数组作为新的数组，遇到一个元素和前面的元素相同时，就将这个元素删掉，并将后面的元素向前移动，数组的长度减一。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int i=1;
        while(i<nums.size()){
            if(nums[i-1]==nums[i]){
                move(nums,i);
            }else{
                i++;
            }
            
        }
        return nums.size();
    }

    void move(vector<int>& nums,int i){
        int j=nums.size();
        while(i<j-1){
            nums[i]=nums[i+1];
            i++;
        }
        nums.pop_back();
    }
};
```

![](https://i.loli.net/2019/09/23/AgROvc5mNGLX8J7.png)

   慢到让我怀疑人生。

### 方案二：双指针

​           快指针`i`指向当前正遍历的元素，慢指针`j`指向新数组中的最后一个元素，如果当前数字和新数组中最后一个元素不相等，则将其增加到新数组中。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int i=1,j=0;  //i指向当前遍历的元素，j指向新数组存储的最后一个元素
        while(i<nums.size()){
            if(nums[j]==nums[i]){
                i++;
            }else{
                j++;
                nums[j]=nums[i];
                i++;
            }
        }
        
        nums.resize(j+1);
        return nums.size();
    }
};
```

![](https://i.loli.net/2019/09/23/vhMHDfBxNnZjRmr.png)

一点小小的变化，速度快到让我吃惊，让我明白了算法的整整重要性！