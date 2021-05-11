# Leetcode 题解 - 链表
<!-- GFM-TOC -->
* [Leetcode 题解 - 链表](#leetcode-题解---链表)
    * [1. 找出两个链表的交点](#1-找出两个链表的交点)
    * [2. 链表反转](#2-链表反转)
    * [3. 归并两个有序的链表](#3-归并两个有序的链表)
    * [4. 从有序链表中删除重复节点](#4-从有序链表中删除重复节点)
    * [5. 删除链表的倒数第 n 个节点](#5-删除链表的倒数第-n-个节点)
    * [6. 交换链表中的相邻结点](#6-交换链表中的相邻结点)
    * [7. 链表求和](#7-链表求和)
    * [8. 回文链表](#8-回文链表)
    * [9. 分隔链表](#9-分隔链表)
    * [10. 链表元素按奇偶聚集](#10-链表元素按奇偶聚集)
    * [11. 分隔链表crn](#11-分隔链表crn)
<!-- GFM-TOC -->


链表是空节点，或者有一个值和一个指向下一个链表的指针，因此很多链表问题可以用递归来处理。

##  1. 找出两个链表的交点

160\. Intersection of Two Linked Lists (Easy)

[Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/) / [力扣](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/description/)

例如以下示例中 A 和 B 两个链表相交于 c1：

```html
A:          a1 → a2
                    ↘
                      c1 → c2 → c3
                    ↗
B:    b1 → b2 → b3
```

但是不会出现以下相交的情况，因为每个节点只有一个 next 指针，也就只能有一个后继节点，而以下示例中节点 c 有两个后继节点。

```html
A:          a1 → a2       d1 → d2
                    ↘  ↗
                      c
                    ↗  ↘
B:    b1 → b2 → b3        e1 → e2
```

要求时间复杂度为 O(N)，空间复杂度为 O(1)。如果不存在交点则返回 null。 

设 A 的长度为 a + c，B 的长度为 b + c，其中 c 为尾部公共部分长度，可知 a + c + b = b + c + a。 

当访问 A 链表的指针访问到链表尾部时，令它从链表 B 的头部开始访问链表 B；同样地，当访问 B 链表的指针访问到链表尾部时，令它从链表 A 的头部开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。

如果不存在交点，那么 a + b = b + a（没有公共部分c），以下实现代码中 a 和 b 会同时为 null，从而退出循环。**判断条件可以是任何表达式，任何非零、或非空（null）的值均为true。**

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        a, b = headA, headB
        while a != b:#如果没有公共部分，a 和 b 会同时（各自走完对方的链表后变）为 null，从而退出循环
            a = a.next if a else headB
            b = b.next if b else headA
        return a
```
等价于
```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        a,b = headA,headB
        while a != b:
            if a :
                a = a.next 
            else:
                a = headB
            if b :
                b = b.next
            else:
                b = headA
        return a 
```

如果只是判断是否存在交点，那么就是另一个问题，即 [编程之美 3.6]() 的问题。有两种解法：

- 把第一个链表的结尾连接到第二个链表的开头，看第二个链表是否存在环；
- 或者直接比较两个链表的最后一个节点是否相同。

##  2. 链表反转

206\. Reverse Linked List (Easy)

[Leetcode](https://leetcode.com/problems/reverse-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/reverse-linked-list/description/)

迭代

![image](https://user-images.githubusercontent.com/70521393/117388602-a1e53e80-af1d-11eb-9a8f-ed76392f8798.png)

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head is None or head.next is None:
            return head
        pre = None
        cur = head
        while cur:
            temp = cur.next# 先把原来cur.next位置存起来
            cur.next = pre#注意顺序，每个赋值（右边的）后的变成下一个被赋值的（左边的）
            pre = cur
            cur = temp
        return pre
```
时间复杂度：O(n)
空间复杂度：O(1)

递归  [讲解](https://leetcode-cn.com/problems/reverse-linked-list/solution/shi-pin-jiang-jie-die-dai-he-di-gui-hen-hswxy/)

```python3
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head is None or head.next is None:
            return head
        node = self.reverseList(head.next)#递归调用的“递”，调用自己
        #下面两步是递归调用的“归”
        head.next.next = head#现在head的next的next变成head，反转
        head.next = None#变成链表的结尾
        return node

```
时间复杂度：O(n)
空间复杂度：O(n)

##  3. 归并两个有序的链表

21\. Merge Two Sorted Lists (Easy)

[Leetcode](https://leetcode.com/problems/merge-two-sorted-lists/description/) / [力扣](https://leetcode-cn.com/problems/merge-two-sorted-lists/description/)

递归：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2
```
时间复杂度：O(n + m)，其中 n 和 m 分别为两个链表的长度。因为每次调用递归都会去掉 l1 或者 l2 的头节点（直到至少有一个链表为空），函数 mergeTwoList 至多只会递归调用每个节点一次。因此，时间复杂度取决于合并后的链表长度，即 O(n+m)。

空间复杂度：O(n + m)，其中 n 和 m 分别为两个链表的长度。递归调用 mergeTwoLists 函数时需要消耗栈空间，栈空间的大小取决于递归调用的深度。结束递归调用时 mergeTwoLists 函数最多调用 n+m 次，因此空间复杂度为 O(n+m)。

迭代：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
   def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:         
        prehead = ListNode(-1)#随便赋一个值给ListNode类作为起点。
        prev = prehead
        while l1 and l2:
            if l1.val <= l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next            
            prev = prev.next

        # 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 if l1 is not None else l2

        return prehead.next#从头开始，即以随便赋值的起点的下一位开始，此位才是真正的起点。
```
时间复杂度：O(n + m)，其中 n 和 m 分别为两个链表的长度。因为每次循环迭代中，l1 和 l2 只有一个元素会被放进合并链表中， 因此 while 循环的次数不会超过两个链表的长度之和。所有其他操作的时间复杂度都是常数级别的，因此总的时间复杂度为 O(n+m)。

空间复杂度：O(1)。我们只需要常数的空间存放若干变量。

##  4. 从有序链表中删除重复节点

83\. Remove Duplicates from Sorted List (Easy)

[Leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/) / [力扣](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/description/)

```html
Given 1->1->2, return 1->2.
Given 1->1->2->3->3, return 1->2->3.
```

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return head

        cur = head
        while cur.next:
            if cur.val == cur.next.val:#注意是值相等
                cur.next = cur.next.next
            else:
                cur = cur.next

        return head
```
时间复杂度：O(n)，其中 n 是链表的长度。 
空间复杂度：O(1)。

##  5. 删除链表的倒数第 n 个节点

19\. Remove Nth Node From End of List (Medium)

[Leetcode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/) / [力扣](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/description/)

```html
Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
```

（暴力法）求链表长度：

```python3
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        def getLength(head: ListNode) -> int:#获取链表的长度
            length = 0
            while head:
                length += 1
                head = head.next
            return length
        
        dummy = ListNode(0, head)#哑节点（随便赋什么值，但要下一个结点需要是head
        length = getLength(head)#是原链表的长度
        cur = dummy
        for i in range(length - n):
            cur = cur.next
        cur.next = cur.next.next
        return dummy.next
 ```
 
 快慢指针：
 
 ```PYTHON3
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0, head)
        first = head
        second = dummy
        for i in range(n):
            first = first.next

        while first:
            first = first.next
            second = second.next
        
        second.next = second.next.next
        return dummy.next

```

##  6. 交换链表中的相邻结点

24\. Swap Nodes in Pairs (Medium)

[Leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/description/) / [力扣](https://leetcode-cn.com/problems/swap-nodes-in-pairs/description/)

```html
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

题目要求：不能修改结点的 val 值，O(1) 空间复杂度。->所以用迭代，不用递归

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(0,head)
        temp = dummy
        while temp.next and temp.next.next:#temp 的后面没有节点**或者**只有一个节点(只要有一个发生，就停止），则没有更多的节点需要交换，因此结束交换
            node1 = temp.next
            node2 = node1.next
            temp.next = node2
            node1.next = node2.next
            node2.next = node1
            temp = node1
        return dummy.next
```

##  7. 链表求和

445\. Add Two Numbers II (Medium)

[Leetcode](https://leetcode.com/problems/add-two-numbers-ii/description/) / [力扣](https://leetcode-cn.com/problems/add-two-numbers-ii/description/)

```html
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

题目要求：不能修改原始链表。

### 解题思路
链表中数位的顺序与我们做加法的顺序是相反的，为了逆序处理所有数位，我们可以使用栈：把所有数字压入栈中，再依次取出相加。计算过程中需要注意进位的情况。

### 头插法：

原状态如下： 
dummy -> dummy.next  
new_node -> new_node.next  

1:将cur当前值赋值给new_node节点   
    new_node = ListNode(cur)  

2：将new_node的下一个节点变为dummy.next  
    new_node.next = dummy.next  
状态更新：  
dummy -> dummy.next  
**new_node** -> dummy.next  

3：将dummy的下一个节点变为new_node  
    dummy.next = new_node  
状态更新： 
**dummy** -> new_node -> dummy.next  

这样new_node就插入到头（dummy）的后面啦  

### 代码

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        stack1,stack2 =[],[]
        dummy = ListNode(-1)

        while l1:
            stack1.append(l1.val)
            l1 = l1.next
        while l2:
            stack2.append(l2.val)
            l2 = l2.next

        carry = 0#保存进位的值
        while stack1 or stack2 or carry!=0:
            tmp1,tmp2 = 0 , 0
            if stack1:
                tmp1 = stack1.pop()
            if stack2:
                tmp2 = stack2.pop()
            cur = tmp1 + tmp2 + carry
            carry = cur // 10#进到下一位的值
            cur %= 10#当前值为除10的余数
            #下面三步是头插法，保证头节点不变，在头节点后不断插入
            new_node = ListNode(cur)#将当前值作为新节点的值
            new_node.next = dummy.next
            dummy.next = new_node
        
        return dummy.next
        

```

##  8. 回文链表

234\. Palindrome Linked List (Easy)

[Leetcode](https://leetcode.com/problems/palindrome-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-linked-list/description/)

题目要求：以 O(1) 的空间复杂度来求解。

python3有简单的解法，即利用列表的反转([::-1])来判断

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        val = []
        while head:
            val.append(head.val)
            head = head.next
        return val == val[::-1]
```

##  9. 分隔链表

725\. Split Linked List in Parts(Medium)

[Leetcode](https://leetcode.com/problems/split-linked-list-in-parts/description/) / [力扣](https://leetcode-cn.com/problems/split-linked-list-in-parts/description/)

```html
Input:
root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
Output: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.
```

题目描述：把链表分隔成 k 部分，每部分的长度都应该尽可能相同，排在前面的长度应该大于等于后面的。

解题思路： 
如果链表有 N 个结点，则分隔的链表中每个部分中都有 N/k 个结点，且前 N%k 部分有一个额外的结点。我们可以用一个简单的循环来计算 N。 

现在对于每个部分，我们已经计算出该部分有多少个节点：width + (i < remainder ? 1 : 0)。我们创建一个新列表并将该部分写入该列表。

```python3
class Solution(object):
    def splitListToParts(self, root, k):
        cur = root
        for N in range(1001):
            if not cur:break#在cur为None的时候跳出循环，即可得到N（链表长度）
            cur = cur.next
        width, remainder = divmod(N, k)#分隔的链表中每个部分中都有 N//k 个结点(width)，且前 N%k 部分有一个额外的结点(remainder)
        
        ans = []
        cur = root
        for i in range(k):
            dummy = ListNode(0)
            sub = dummy
            for j in range(width + (i < remainder)):
                sub.next = ListNode(cur.val)
                sub = sub.next
                if cur:
                    cur = cur.next
            ans.append(dummy.next)
        return ans
```

时间复杂度：O(N + k)。N 指的是所给链表的结点数，若 k 很大，则还需要添加许多空列表。
空间复杂度：O(max(N,k))，存储答案时所需的额外空格。

##  10. 链表元素按奇偶聚集

328\. Odd Even Linked List (Medium)

[Leetcode](https://leetcode.com/problems/odd-even-linked-list/description/) / [力扣](https://leetcode-cn.com/problems/odd-even-linked-list/description/)

```html
Example:
Given 1->2->3->4->5->NULL,
return 1->3->5->2->4->NULL.
```

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head:return head
        odd = head
        even_head = even = head.next#偶节点头单独拿出来，因为要和奇节点链表的末端相连
        while odd.next and even.next:#？？？
            odd.next = odd.next.next
            even.next = even.next.next
            odd,even = odd.next,even.next
        odd.next = even_head
        return head

```

##  11. 分隔链表crn

86\. Partition List(Medium)

[力扣](https://leetcode-cn.com/problems/partition-list/)

![image](https://user-images.githubusercontent.com/70521393/117416984-7e85b800-af4c-11eb-8ae8-f9698e64a0cf.png)
```html
Input:
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```

题目描述：给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        p=less=ListNode(0)
        q=more=ListNode(0)

        while head:
            if head.val<x:
                less.next=head
                less=less.next
            else:
                more.next=head
                more=more.next
            head=head.next

        more.next=None
        less.next=q.next
        return p.next
```
