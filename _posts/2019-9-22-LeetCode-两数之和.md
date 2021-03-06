---
layout:     post
title:      LeetCode-两数之和
subtitle:   LeetCode/两数之和问题
date:       2019-9-22
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---



## 题目

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 示例

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]



## 解决方案

### 方案一：暴力解法

​    首先最容易想到的就是暴力解法，即通过两层for循环进行判断是否有相应的元素，但这种解法的效率比较低，时间复杂度很高，空间复杂度较低。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::iterator ite,ite2;
        int first=0,second=first+1;
        vector<int> result;
        for(ite=nums.begin();ite<nums.end();ite++) {
            for (ite2 = ite + 1; ite2 < nums.end(); ite2++) {
                if (*ite + *ite2 == target) {
                    result.push_back(first);
                    result.push_back(second);
                    return result;
                }
                second++;
            }
            first++;
            second=first;
        }
         return result;
    }
};
```



### 方案二：两遍map扫描

​      我们可以采用查表的方式进行查找，首先扫描一次所给的vector，对每一个元素都做以下操作：

​       将target-该元素的值作为key，该元素的索引作为value存入到map之中；然后再扫描一次所给的vector，对于每个元素查找map中是否存在与其相同的元素(因为map中存的是插值，所以此处查找的是与元素相同的目标)，如果是就是结果。

​      比如：如果vector=[2,7,1,3],target=9;那么第一遍扫描就得到`map={(7,0),(2,1),(8,2),(6,3)}`,第二遍扫描到2时候，发现map中存在key为2的元素，那么该vector中值为2的索引和，map中key为2对应的value即为所求的结果。

> 注意一种情况，就是[3,1,3]，6这种情况，我们巧妙的使用key值覆盖的方式，进行求解。如果存在两个相同的元素相加等于target，那么这相同元素中必索引值不同，所以我们在创建map的时候让后来的将先来的覆盖掉。
>
> 第二次扫描是顺序扫描，所以如果存在多个相同的元素，一定会先扫描到索引值小的那个，但此时他的索引值一定与map中那个元素的索引值不同，因为产生了覆盖。
>
> 所以我们在创建map时要采用map[key]=value格式，因为只有这样才会产生覆盖，insert()方法在插入相同值时会拒绝操作.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>::iterator ite;
        map<int,int> table;
        vector<int> result;
        int i=0;
        for(ite=nums.begin();ite<nums.end();ite++){   //先扫描一次，将所有元素与target的插值存入到table中
            table[target-*ite]=i;             //如果存在相同的值，会出现覆盖
            i++;
        }
        i=-1;
        for(ite=nums.begin();ite<nums.end();ite++){
            i++;
            if(table.find(*ite)!=table.end()){
                //因此在插入值时会出现覆盖，即如若存在相同的元素，则后面的元素会将前面的覆盖掉；
                //但是扫描时顺序扫描，所以如果下标相同则说明是同一元素，同一元素不可重用
                if(table[*ite]==i)
                    continue;
               result.push_back(i);
               result.push_back(table[*ite]);
               return result;
           }
        }
        return result;
    }
};

```



### 方案三：一边map扫描

​          就是在创建map的时候，随时判断是否已经寻找到相匹配的元素，如果找到了就停止创建，返回结果。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
            vector<int>::iterator ite;
            map<int,int> table;
            vector<int> result;
            int i=0,temp=0;
            for(ite=nums.begin();ite<nums.end();ite++){   
                temp=target-*ite;                  
                //在插入前查看是否已经存在匹配的元素
                if(table.find(temp)!=table.end()){ 
                    result.push_back(table[temp]);
                    result.push_back(i);
                    break;
                }
                table[*ite]=i;
                i++;
            }
            return result;
        
    }
};

```

