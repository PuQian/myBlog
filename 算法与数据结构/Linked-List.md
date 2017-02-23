---
title: 【 算法 】Linked List
date: 2015-10-20 11:04:47
tags: 算法
---

### 单链表定义
```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
```

## 判断一个链表是否有环

思路：如果开始有两个指针指向头结点，一个走的快，一个走的慢，如果有环的话，最终经过若干步，快的指针总会超过慢的指针一圈从而相遇。

如何计算环的长度呢？** 可以第一次相遇时开始计数，第二次相遇时停止计数**。

如何判断环的入口点？** 碰撞点p到连接点的距离=头指针到连接点的距离 **，因此，分别从碰撞点、头指针开始走，相遇的那个点就是连接点。

当fast与slow相遇时，show肯定没有走完链表，而fast已经在还里走了n（n>=1）圈。假设slow走了s步，那么fast走了2s步。fast的步数还等于s走的加上环里转的n圈，所以有：2s = s + nr。因此，s = nr。   　

　　设整个链表长为L，入口据相遇点X，起点到入口的距离为a。因为slow指针并没有走完一圈，所以：a + x = s，带入第一步的结果，有：a + x = nr = (n-1)r + r = (n-1)r + L - a；即：a = (n-1)r + L -a -x;

　　这说明：从头结点到入口的距离，等于转了(n-1)圈以后，相遇点到入口的距离。因此，我们可以在链表头、相遇点各设一个指针，每次各走一步，两个指针必定相遇，且相遇第一点为环入口点。

<br/>

### 判断是否有环
```
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while(fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;
        if(slow == fast){
            break;
        }
    }
    if(fast == null || fast.next == null){
        return false;
    }else{
        return true;
    }
}
```

<br/>

### 计算环的入口节点
```
public ListNode detectCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while(fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;

        if(slow == fast){
            break;
        }
    }
    if(fast == null || fast.next == null){
        return null;
    }else{
        slow = head;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

<br/>

### 计算环的长度
```
public int loopLength(ListNode head){

    if(hasCycle(head) == fasle){
        return 0;
    }

    ListNode slow = head;
    ListNode fast = head;
    
    int len = 0;
    boolean begin = false;
    boolean again = false;

    while(fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;
        
        // 超两圈后停止计数，跳出循环
        if(slow == fast && again == true){
            break;
        }
        
        // 超一圈后开始计数
        if(slow == fast && again == false){
            begin = true;
            again = true;
        }

        if(begin == true){
            len++;
        }
    }
    return len;
}
```
<!-- public class Solution {
    public int loopLength(ListNode head) {
        
        if(hasCycle(head) == false) 
            return 0;

        ListNode slow = head;
        ListNode fast = head;
        
        ListNode meet = null;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;

            if(slow == fast){
                meet = slow;
                break;
            }
        }
        if(fast == null || fast.next == null){
            return 0;
        }else{
            int len = 0;
            while(slow != fast){
                slow = slow.next;
                fast = fast.next.next;
                len ++ ;
            }
            return len;
        }
    }
} -->

<br/>

## 删除链表倒数第 K 个节点

遍历两次链表，第一次从 K 值开始减，第二次在第一次遍历值得情况下增加。找到删除的第 K 个节点的前一个节点，注意判断删除的是头节点的情况。

```
public Node removeNthFromEnd(ListNode head, int n){
    if(head == null || n <= 0)
        return head;

    Node cur = head;

    while (cur != null) {
        n--;
        cur = cur.next;
    }

    // 删除的是头节点
    if (n == 0) {  
        head = head.next;
    }
    if (n < 0) {
        cur = head;

        while(++n != 0){
            cur = cur.next;     
        }
        cur.next = cur.next.next;
    }
    return head;
}
```

<br/>

## 合并有序链表

### 合并两个有序链表 -- 正常解法

使用3个指针，一个为头节点 newl，一个为连接节点 pre，一个为新节点 cur

```
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode newl = new ListNode(0);
    ListNode pre = newl;
    
    while(l1 != null || l2 !=null){
        ListNode cur = null;
        
        if(l1 == null){
            cur = l2;
            l2 = l2.next;
        }else if(l2 == null){
            cur = l1;
            l1 = l1.next;
        }else{
            if(l1.val <= l2.val){
                cur = l1;
                l1 = l1.next;
            }else{
                cur = l2;
                l2 = l2.next;
            }
        }
        pre.next = cur;
        pre = pre.next;
    }
    
    // 注意返回
    return newl.next;
}
```

<br/>

### 合并两个有序链表 -- 回溯
```
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if(l1 == null){
        return l2;   
    }
    if(l2 == null){
        return l1;   
    }
        
    ListNode mergeHead = null;
    if(l1.val <= l2.val){
        mergeHead = l1;
        mergeHead.next = mergeTwoLists(l1.next, l2);
    }else{
        mergeHead = l2;
        mergeHead.next = mergeTwoLists(l1, l2.next);
    }
    return mergeHead;
}
```

<br/>

### 合并 K 个有序链表 -- 回溯
二分合并
```
public ListNode mergeKLists(ListNode[] lists) {
    return partion(lists, 0, lists.length - 1);
}

// 注意
public ListNode partion (ListNode[] lists, int s, int e){
    if(s == e) return lists[s];
    if(s < e){
        int q =(s+e)/2;
        ListNode l1 = partion(lists, s, q);
        ListNode l2 = partion(lists, q+1, e);
        return mergeTwoLists(l1, l2);
    }else{
        return null;
    }
}

// 合并两个有序链表
public ListNode mergeTwoLists(ListNode l1, ListNode l2){
    if(l1 == null) return l2;
    if(l2 == null) return l1;
    
    ListNode merge = null;
    if(l1.val < l2.val){
        merge = l1;
        merge.next = mergeTwoLists(l1.next, l2);
    }else{
        merge = l2;
        merge.next = mergeTwoLists(l1, l2.next);
    }
    return merge;
}
```

<br/>

## 求两个链表的初始交叉点

### 思路一 - 求链表环

将其中一个链表首尾相连，求另一个链表是否有环，返回环的初始点

```
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if(headA == null || headB == null){
        return null;
    }
    
    ListNode endA = headA;
    while(endA.next != null){
        endA = endA.next;
    }
    endA.next = headA;
    
    ListNode slowB = headB;
    ListNode fastB = headB;
    while(fastB.next != null && fastB.next.next != null){
        slowB = slowB.next;
        fastB = fastB.next.next;
        if(slowB == fastB){
            break;
        }
    }
    if(fastB.next == null || fastB.next.next == null){
        endA.next = null;
        return null;
    }else{
        slowB = headB;
        while(slowB != fastB){
            slowB = slowB.next;
            fastB = fastB.next;
        }
        endA.next = null;
        return fastB;
    }
}
```

<br/>

### 思路二 - 链表总长度之差

链表交叉点之后的长度相同，交叉点之前的长度之差为链表总长度之差
```
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if(headA == null || headB == null) return null;
    
    ListNode nodeA = headA;
    ListNode nodeB = headB;
    
    int lenA = 0, lenB = 0, minus = 0;
    while(nodeA != null){
        nodeA = nodeA.next;
        lenA++;
    }
    while(nodeB != null){
        nodeB = nodeB.next;
        lenB++;
    }
    
    nodeA = headA;
    nodeB = headB;
    if(lenA > lenB){
        minus = lenA - lenB;
        while(--minus >= 0){
            nodeA = nodeA.next;
        }
    }else{
        minus = lenB - lenA;
        while(--minus >= 0){
            nodeB = nodeB.next;
        }
    }
    while(nodeA != null && nodeB != null){
        if(nodeA == nodeB){
            return nodeA;
        }else{
            nodeA = nodeA.next;
            nodeB = nodeB.next;
        }
    }
    return null;
}
```

<br/>

## 反转链表

### 反转单链表
用三个节点来转换链表指向，前一个节点 pre、当前节点 now、后一个节点 next。注意：链表的最后一个节点指向 null。
```
public ListNode reverseList(ListNode head) {
    ListNode pre = null;
    ListNode now = head;
    ListNode next = null;
    
    while(now != null){
        next = now.next;
        now.next = pre;
        pre = now;
        now = next;
    }
    return pre;
}
```

<br/>

### 反转 m 至 n 的单链表
思路： 找到反转链表前一个节点 fPos，反转链表后一个节点 tPos。注意：从第一个节点开始反转的话，头节点会发生变化。
贴出我自己 A 的稍微冗余的代码，**有待改进**，更精简的代码《最优解》书上有，思路是一样的。
```
public ListNode reverseBetween(ListNode head, int m, int n) {
    ListNode bPos = null;
    ListNode ePos = null;
    
    ListNode node = head;
    int len = 0;
    while(++len < n+1){
        if(len < m){
            bPos = node;
        }
        node = node.next;
    }
    ePos = node;
     
    ListNode pre = null;
    ListNode next = null;
    ListNode now =null;
    
    // 反转头节点
    if(bPos == null){
        pre = ePos;
        now = head;
        while(now != ePos){
            next = now.next;
            now.next = pre;
            pre = now;
            now = next;
        }
        head = pre;
    }else{
        now = bPos.next;
        while(now != ePos){
            next = now.next;
            now.next = pre;
            pre = now;
            now = next;
        }
        next = bPos.next;
        bPos.next = pre;
        next.next = ePos;
    }
    return head;
}
```
<br/>

## 在单链表中删除重复的节点

### 删除所有出现重复的节点
比如链表为 1->1->2->3，结果为 2->3。**~~~**
```
public ListNode deleteDuplicates(ListNode head) {
    if(head == null) return null;
    
    ListNode FakeHead = new ListNode(0);
    FakeHead.next = head;
    ListNode pre = FakeHead;
    ListNode cur = head;
    
    while(cur != null){
        while(cur.next != null && cur.val == cur.next.val){
            cur = cur.next;
        }
        if(pre.next == cur){
            pre = pre.next;
        }
        else{
            pre.next = cur.next;
        }
        cur = cur.next;
    }
    return FakeHead.next;
}
```

<br/>

## 在链表中删除节点 node
思路：题目只给出要删除节点的索引，因此采用交换节点值而不是删除节点本身的方法。
```
public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```