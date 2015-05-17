#Intersection of Two Linked Lists#

https://leetcode.com/problems/intersection-of-two-linked-lists/
，给定两个链表，确定是否有交点。要在线性时间和常量空间内完成。最naive的想法是对一条链表的每个结点，都在另一个链表中查找，但这不是常量空间。参考了http://www.2cto.com/kf/201502/374789.html
，第三种方法，先计算长度差，然后先提前移动长链表的指针，起点相同后再同时一个一个移动，真是简单却又有效的方法！

实现如下：注意链表基本一开始都要判断是否为空链表。

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = 0, lenB = 0;
        ListNode* pA = headA; ListNode* pB = headB;
        if (pA == NULL || pB == NULL)
            return NULL;
        
        //find the length difference
        while (pA->next != NULL)
        {
            pA = pA->next;
            lenA++;
        }
        while (pB->next != NULL)
        {
            pB = pB->next;
            lenB++;
        }
        
        //compensate for the difference and start traversal one by one
        pA = headA; pB = headB;
        if (lenA > lenB)
            while(lenA != lenB)
            {
                pA = pA->next;
                lenA--;
            }
        else if (lenB > lenA)
            while(lenB != lenA)
            {
                pB = pB->next;
                lenB--;
            }
        while (pA != NULL)
        {
            if (pA->val == pB->val)
                return pA;
            pA = pA->next;
            pB = pB->next;
        }
        return NULL;
    }
};
```

根据测试样例，发现这里“相交”定义为结点值相交，而不是指针地址相交，所以是判断`pA->val == pB->val`，实际应该是判断地址的。

此题还有一种解法是先构造一个环，问题就化归为检查另一个链表是否有环，可以用龟兔赛跑的方法解。参见http://blog.csdn.net/yoxibaga/article/details/41697751
，学习一下龟兔赛跑的方法，证明了找到碰撞点后，从链表头和碰撞点同时开始逐一遍历，第一个相交的结点就是环的起点。证明的时候，用“相对速度”的概念虽然好理解（fast只比slow每次多走一步而不是两步，因此不会发生走两步跳过了slow的情况），但可能是链表特别长环特别短，还是老老实实按照http://blog.csdn.net/yoxibaga/article/details/41697751
的证法比较严密。
