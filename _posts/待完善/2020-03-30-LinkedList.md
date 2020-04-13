# 链表


*   链表是一种由节点（Node）组成的线性数据集合，每个节点通过指针指向下一个节点。它是一种由节点组成，并能用于表示序列的数据结构。
*   **单链表**：每个节点仅指向下一个节点，最后一个节点指向空（null）。
*   **双链表**：每个节点有两个指针p，n。p指向前一个节点，n指向下一个节点；最后一个节点指向空。
*   **循环链表**：每个节点指向下一个节点，最后一个节点指向第一个节点。
*   时间复杂度：
    *   索引：O(n)
    *   查找：O(n)
    *   插入：O(1)
    *   删除：O(1)

[TOC]

## Reverse Linked List
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\ec80ae81-7cc2-4f38-9aba-2fc86d72cf59.jpg)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode result = null;
        while(head!=null){
            ListNode temp=head.next;
            head.next=result;
            result=head;
            head=temp;
        }
        return result;
    }
}
```

-----

-----

## Reverse Linked List II
>Reverse a linked list from position m to n. Do it in-place and in one-pass.
>**For example:**
>Given 1->2->3->4->5->NULL, m = 2 and n = 4,
>return 1->4->3->2->5->NULL.
>**Note:**
>Given m, n satisfy the following condition:
>1 ≤ m ≤ n ≤ length of list.

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\bf3ab738-148c-4123-bfca-1409acf7c7a7.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\10747983-47d3-42b3-80d2-23c0df9533e6.jpg)

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        int change_len = n-m+1;
        ListNode pre_head=null;
        ListNode result = head;
        while(head!=null&&--m>0){
            pre_head=head;
            head=head.next;
        }
        ListNode modify_list_tail = head;
        ListNode new_head = null;
        while(head!=null && change_len>0){
            ListNode next = head.next;
            head.next = new_head;
            new_head = head;
            head = next;
            change_len--;
        }
        modify_list_tail.next=head;
        if(pre_head!=null){
            pre_head.next=new_head;
        }
        else{
            result=new_head;
        }
        return result;
    }
}
```

-----

-----

## Intersection of Two Linked Lists
>Write a program to find the node at which the intersection of two singly linked lists begins.
>**For example**, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗
B:     b1 → b2 → b3
```
begin to intersect at node c1.begin to intersect at node c1.
**Notes:**
- If the two linked lists have no intersection at all, return null.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.

### 方法一
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> node_set = new HashSet<>();
        while(headA!=null){
            node_set.add(headA);
            headA=headA.next;
        }
        while(headB!=null){
            if(node_set.contains(headB)){
                return headB;
            }
            headB=headB.next;
        }
        return null;
    }
}
```

### 方法二
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\b2e48c82-ca8c-414f-9797-d7a9892e0148.jpg)
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = get_list_length(headA);
        int lenB = get_list_length(headB);
        if(lenA>lenB){
            headA=forword_long_list(lenA,lenB,headA);
        }
        else{
            headB=forword_long_list(lenB,lenA,headB);
        }
        while(headA!=null&&headB!=null){
            if(headA==headB) return headA;
            headA=headA.next;
            headB=headB.next;
        }
        return null;
    }
    public ListNode forword_long_list(int long_len,int short_len,ListNode head){
        int delta = long_len - short_len;
        while(head!=null&&delta>0){
            head=head.next;
            delta--;
        }
        return head;
    }
    public int get_list_length(ListNode head){
        int length=0;
        while(head!=null){
            length++;
            head=head.next;
        }
        return length;
    }
}
```

-----

-----

## Linked List Cycle II
>Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
>**Note:** Do not modify the linked list.
>**Follow up:**
>Can you solve it without using extra space?
>![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\e3c01b75-e681-4f50-88aa-db45211201bd.jpg)

### 方法一
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\3bcbbe3d-09f8-4c0e-ab25-0f5ed4291ed1.jpg)
<p/>
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\5b1a304d-518f-4f95-817e-eb479740011a.jpg)
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        ListNode meet = null;
        while(fast!=null){
            slow = slow.next;
            fast = fast.next;
            if(fast==null){
                return null;
            }
            fast=fast.next;
            if(fast==slow){
                meet=fast;
                break;
            }
        }
        if(meet==null){
            return null;
        }
        while(head!=null&&meet!=null){
            if(head==meet)
                return head;
            head=head.next;
            meet=meet.next;
        }
        return null;
    }
}
```

-----

-----

## Partition List
>Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
>You should preserve the original relative order of the nodes in each of the two partitions.
>**For example,**
>Given 1->4->3->2->5->2 and x = 3,
>return 1->2->2->4->3->5.
>![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\bf12f4ba-ddee-4576-ae92-ecb8cf48e1be.jpg)

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\6ed5a01c-e5b9-479b-9d24-84e2259b1409.jpg)
![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\8f9f4985-73bf-4a20-aafb-2ba6ca0e86f4.jpg)
```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode less_head = new ListNode(0);
        ListNode more_head = new ListNode(0);
        ListNode less_ptr = less_head;
        ListNode more_ptr = more_head;
        while(head!=null){
            if(head.val<x){
                less_ptr.next=head;
                less_ptr = less_ptr.next;
            }
            else{
                more_ptr.next=head;
                more_ptr = more_ptr.next;
            }
            head=head.next;
        }
        less_ptr.next=more_head.next;
        more_ptr.next=null;
        return less_head.next;
    }
}
```

-----

-----

## Copy List with Random Pointer
>A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
>Return a deep copy of the list.
>![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\1b6931d4-1980-45ee-953f-9c44d4da0429.jpg)

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\e852311b-45f7-47a9-a8d0-2eb316ae1226.jpg)
```java
import java.util.Vector;
public class Solution {
    public RandomListNode copyRandomList(RandomListNode head) {
        Map<RandomListNode,Integer> position_index = new HashMap<>();
        Vector<RandomListNode> index_position = new Vector<>();
        RandomListNode ptr = head;
        int i=0;
        while(ptr!=null){
            index_position.add(new RandomListNode(ptr.label));
            position_index.put(ptr,i);
            ptr = ptr.next;
            i++;
        }
        index_position.add(null);
        ptr=head;
        i=0;
        while(ptr!=null){
            index_position.get(i).next = index_position.get(i+1);
            if(ptr.random!=null){
                int id = position_index.get(ptr.random);
                index_position.get(i).random = index_position.get(id);
            }
            ptr = ptr.next;
            i++;
        }
        return index_position.get(0);
    }
}
```

-----

-----

## Merge Two Sorted Lists
>Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\3e6ef063-a1f2-4869-a07c-48fbeafd32fd.jpg)
```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null) return l2;
        if(l2==null) return l1;
        ListNode p;
        ListNode result;
        if(l1.val<l2.val){
            result=l1;
            l1=l1.next;
        }
        else{
            result=l2;
            l2=l2.next;
        } 
        p=result;
        while(l1!=null&&l2!=null){
            if(l1.val<l2.val){
                p.next=l1;
                l1=l1.next;
            } 
            else{
                p.next=l2;
                l2=l2.next;
            }
            p=p.next;
        }
        if(l1==null) p.next=l2;
        else p.next=l1;
        return result;
    }
}
```

-----

-----

## Merge k Sorted Lists
>Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.


![](D:\Users\lin.yang\Desktop\备忘录\Knowledge\Algorithm\链表\30d90d26-39ab-484e-bccb-2e8f084fe95a.jpg)
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0) return null;
        if(lists.length==1) return lists[0];
        if(lists.length==2) return mergeTwoLists(lists[0],lists[1]);
        int mid = lists.length/2;
        ListNode[] sub1_lists=new ListNode[mid];
        ListNode[] sub2_lists=new ListNode[lists.length-mid];
        for(int i=0;i<mid;i++){
            sub1_lists[i] = lists[i];
        }
        for(int i=mid;i<lists.length;i++){
            sub2_lists[i-mid]=lists[i];
        }
        ListNode l1 = mergeKLists(sub1_lists);
        ListNode l2 = mergeKLists(sub2_lists);
        return mergeTwoLists(l1,l2);
    }
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null) return l2;
        if(l2==null) return l1;
        ListNode p;
        ListNode result;
        if(l1.val<l2.val){
            result=l1;
            l1=l1.next;
        }
        else{
            result=l2;
            l2=l2.next;
        } 
        p=result;
        while(l1!=null&&l2!=null){
            if(l1.val<l2.val){
                p.next=l1;
                l1=l1.next;
            } 
            else{
                p.next=l2;
                l2=l2.next;
            }
            p=p.next;
        }
        if(l1==null) p.next=l2;
        else p.next=l1;
        return result;
    }
}
```

## Remove Nth Node From End of List
>Given a linked list, remove the _n_<sup style="box-sizing: border-box; position: relative; font-size: 12px; line-height: 0; vertical-align: baseline; top: -0.5em;">th</sup> node from the end of list and return its head.
>For example,
><pre style="box-sizing: border-box; overflow: auto; font-family: Menlo, Monaco, Consolas, &quot;Courier New&quot;, monospace; font-size: 13px; display: block; padding: 9.5px; margin: 0px 0px 10px; line-height: 1.42857; color: rgb(51, 51, 51); word-break: break-all; word-wrap: break-word; background-color: rgb(245, 245, 245); border: 1px solid rgb(204, 204, 204); border-radius: 4px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">   Given linked list: **1->2->3->4->5**, and **_n_ = 2**.
>After removing the second node from the end, the linked list becomes **1->2->3->5**.
></pre>
>**Note:**
>Given _n_ will always be valid.
>Try to do this in one pass.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null) return null;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        //指针移动n步，这个指针作为边界判断指针
        while(cur.next != null && n > 0) {
            cur = cur.next;
            n--;
        }
        ListNode tmp = dummy;
        while(cur.next != null) {
            tmp = tmp.next;
            cur = cur.next;
        }
        tmp.next = tmp.next.next;
        return dummy.next;
    }
}
```