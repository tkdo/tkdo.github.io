## [leetcode-OfferII023-两个链表的第一个重合节点](https://leetcode.cn/problems/3u1WK4/)

    给定两个单链表的头节点 headA 和 headB，请找出并返回两个单链表相交的起始节点。
    如果两个链表没有交点，返回 null。题目数据 保证 整个链式结构中不存在环。
    注意，函数返回结果后，链表必须 保持其原始结构。

=== "c++"

    ```c++
    class Solution {
    public:
        ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            ListNode *pA = headA;
            ListNode *pB = headB;
            while(pA != pB)
            {   //A走完自己的路走B的路，B走完自己的路走A的路。一旦相等则跳出循环。
                pA = pA == nullptr ? headB : pA->next;
                pB = pB == nullptr ? headA : pB->next;
            }
            return  pA;
        }
    };
    ```

