---
layout:     post
title:      LeetCode-合并两个有序链表
subtitle:   LeetCode/尾插法-双指针
date:       2019-9-23
author:     Yifeng
header-img: img/post-leetcode-easy.jpg
catalog: true
tags:
     - Oj/LeetCode
---

## 题目

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 



## 示例

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



## 解决方案

​       利用双指针扫描法，第一个指针指向l1，第二个指针指向l2，每次取出指针所指向的元素，将较小的那个放入新的链表中，并更新对应链表的指针，如此循环直到有一个指针为空停止。

​     将不为空的指针(说明该链表中的元素还没有完全放入新的链表中)按照尾插法依次插入到新的链表中。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *result= nullptr,*tail,*first=l1,*second=l2;
        ListNode *temp;
        while(first!= nullptr&&second!= nullptr){
            if(first->val<=second->val)
            {
               temp=new ListNode(first->val);
               if(result== nullptr)
               {
                   result=temp;
                   tail=result;
               }else{
                   tail->next=temp;
                   tail=temp;

               }
                first=first->next;
            }else{
                temp=new ListNode(second->val);
                if(result== nullptr)
                {
                    result=temp;
                    tail=result;
                }else{
                    tail->next=temp;
                    tail=temp;
                }
                second=second->next;
            }
        }
        while(first!= nullptr){
            temp=new ListNode(first->val);
            if(result== nullptr)
            {
                result=temp;
                tail=result;
            }else{
                tail->next=temp;
                tail=temp;
            }
            first=first->next;
        }

        while(second!= nullptr){
            temp=new ListNode(second->val);
            if(result== nullptr)
            {
                result=temp;
                tail=result;
            }else{
                tail->next=temp;
                tail=temp;
            }
            second=second->next;
        }
        return result;

    }
};
```

