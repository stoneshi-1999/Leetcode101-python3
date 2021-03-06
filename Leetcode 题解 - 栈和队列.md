# Leetcode101-python3
# Leetcode101 题解 - 栈和队列
<!-- GFM-TOC -->
* [Leetcode 题解 - 栈和队列](#leetcode-题解---栈和队列)
    * [1. 用栈实现队列](#1-用栈实现队列)
    * [2. 用队列实现栈](#2-用队列实现栈)
    * [3. 最小值栈](#3-最小值栈)
    * [4. 用栈实现括号匹配](#4-用栈实现括号匹配)
    * [5. 数组中元素与下一个比它大的元素之间的距离](#5-数组中元素与下一个比它大的元素之间的距离)
    * [6. 循环数组中比当前元素大的下一个元素](#6-循环数组中比当前元素大的下一个元素)
    * [7. 基本计算器II](#7-基本计算器II)
<!-- GFM-TOC -->


## 1. 用栈实现队列

232\. Implement Queue using Stacks (Easy)

[Leetcode](https://leetcode.com/problems/implement-queue-using-stacks/description/) / [力扣](https://leetcode-cn.com/problems/implement-queue-using-stacks/description/)

栈的顺序为后进先出，而队列的顺序为先进先出。使用两个栈实现队列，一个元素需要经过两个栈才能出队列，在经过第一个栈时元素顺序被反转，经过第二个栈时再次被反转，此时就是先进先出顺序。

```python
class MyQueue(object):

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def push(self, x):
        self.stack1.append(x)

    def pop(self):
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()

    def peek(self):
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2[-1]

    def empty(self):
        return not self.stack1 and not self.stack2
```

## 2. 用队列实现栈

225\. Implement Stack using Queues (Easy)

[Leetcode](https://leetcode.com/problems/implement-stack-using-queues/description/) / [力扣](https://leetcode-cn.com/problems/implement-stack-using-queues/description/)

方法1：两个队列


```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue1 = collections.deque()
        self.queue2 = collections.deque()


    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.queue2.append(x)
        while self.queue1:
            self.queue2.append(self.queue1.popleft())把之前的所有元素放的新元素的队后，让新元素在队首（先入先出）
        self.queue1, self.queue2 = self.queue2, self.queue1


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue1.popleft()


    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue1[0]


    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return not self.queue1

```

方法2：一个队列
主要是入栈的时候，先把队列之前的所有元素全部依次出队再入队（一个队列，从队前出，又进队尾，可以想象成一个环）

```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue = collections.deque()


    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        n = len(self.queue)#把加入x之前有n个元素
        self.queue.append(x)
        for _ in range(n):#把加入x之前的n个元素全部拿出再放回，保证x在队列最前，即先进先出
            self.queue.append(self.queue.popleft())


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue.popleft()


    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue[0]#队列最前最先出，最先出即为栈顶


    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return not self.queue

```


## 3. 最小值栈

155\. Min Stack (Easy)

[Leetcode](https://leetcode.com/problems/min-stack/description/) / [力扣](https://leetcode-cn.com/problems/min-stack/description/)

用一个辅助栈来存“每一次入栈push时”的最小值，所以注意每次出栈时，两个栈都要出。

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = [math.inf]

    def push(self, x: int) -> None:
        self.stack.append(x)
        self.min_stack.append(min(x, self.min_stack[-1]))#前面记得加self

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]

```

对于实现最小值队列问题，可以先将队列使用栈来实现，然后就将问题转换为最小值栈，这个问题出现在 编程之美：3.7。

## 4. 用栈实现括号匹配

20\. Valid Parentheses (Easy)

[Leetcode](https://leetcode.com/problems/valid-parentheses/description/) / [力扣](https://leetcode-cn.com/problems/valid-parentheses/description/)

```html
"()[]{}"

Output : true
```
只把左括号“{，（，[” 放入栈中，等待匹配

in和字典的操作，判断键是否存在于字典中： https://www.runoob.com/python3/python3-att-dictionary-in-html.html

```python
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {')':'(',
               ']':'[',
               '}':'{'}
        stack = []
        for i in s:#遍历s字符串中的字符
            if stack and i in dic:#如果stack不为空，且i为dic的键，即为右括号时，进行匹配
                if stack[-1] == dic[i]:
                    stack.pop()
                else:
                    return False
            else:#如果stcak为空，或，i不是dic的键，即为左括号，则入栈
                stack.append(i)
        return not stack
```

## 5. 数组中元素与下一个比它大的元素之间的距离

739\. Daily Temperatures (Medium)

[Leetcode](https://leetcode.com/problems/daily-temperatures/description/) / [力扣](https://leetcode-cn.com/problems/daily-temperatures/description/)

```html
Input: [73, 74, 75, 71, 69, 72, 76, 73]
Output: [1, 1, 4, 2, 1, 1, 0, 0]
```

单调栈：
单调栈满足从栈底到栈顶元素对应的温度递减，因此每次有元素进栈时，会将温度更低的元素全部移除，并更新出栈元素对应的等待天数，这样可以确保等待天数一定是最小的。

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        length = len(T)
        ans = [0] * length
        stack = []#单调栈，用于存储下标不存储温度（用于计算升温间隔日期），但是存储的下标对应温度递减，栈底对应温度最高。
        for i in range(length):
            temperature = T[i]
            while stack and temperature > T[stack[-1]]:#当栈不为空，且新温度大于当前栈顶温度时
                prev_index = stack.pop()#之前的最高温度对应的下标出栈
                ans[prev_index] = i - prev_index#计算升温间隔日期
            stack.append(i)#新温度小于等于当前栈顶温度，则入栈
        return ans

```

## 6. 循环数组中比当前元素大的下一个元素

503\. Next Greater Element II (Medium)

[Leetcode](https://leetcode.com/problems/next-greater-element-ii/description/) / [力扣](https://leetcode-cn.com/problems/next-greater-element-ii/description/)

```text
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2;
The number 2 can't find next greater number;
The second 1's next greater number needs to search circularly, which is also 2.
```

与 739. Daily Temperatures (Medium) 不同的是，数组是循环数组，并且最后要求的不是距离而是下一个元素。
同样使用单调栈，但是
使用取模运算 % 可以把下标 i 映射到数组 nums 长度的 0 - N 内。

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [-1] * n
        stk = list()

        for i in range(n * 2 - 1):
            value = nums[i % n]
            while stk and  value > nums[stk[-1]]:
                res[stk.pop()] = value
            stk.append(i % n)
        
        return res

```

## 7. 基本计算器II

 227\. Basic Calculator II(Medium)
给你一个字符串表达式 s ，请你实现一个基本计算器来计算并返回它的值。

整数除法仅保留整数部分。
 [力扣](https://leetcode-cn.com/problems/basic-calculator-ii/)
 
 一个运算符表达式分为三个部分，可以用下面的情况表示：

数字①， 运算符②， 数字③  

数字①，在栈中保存，为栈顶的元素； 
运算符②，用一个变量 pre_op 保存； 
数字③，用一个变量 num 保存。 

操作情况： 
运算符②，决定了现在的操作： 

如果 运算符② 为 +， - ：如果是+，是把数字③入栈；如果 -，是把 数字③取反再入栈。 
如果 运算符② 为 *，/ ，则需要计算 数字① 运算符② 数字③，然后把结果 入栈。 
这样遍历一次后，优先把所有的 *，/ 都计算出来，而且与需要做加减运算的数字一起，全都都放到了栈中，对栈求和，即为最终的结果。 

enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出**数据下标和数据**，一般用在 for 循环当中。
 
//： 取整除 - 向下取接近商的整数，-9//2=-5
但题目要求保留整数部分，所以直接用浮点除再取整数
 
 ```python
 class Solution:
    def calculate(self, s):
        stack = []
        pre_op = '+'
        num = 0
        for i, each in enumerate(s):
            if each.isdigit():
                num = 10 * num + int(each)#保证能够识别多位数（两位及两位以上的数字）
            if i == len(s) - 1 or each in '+-*/':#i循环到最后或者循环到运算符
                    stack.append(num)#如果是加号直接入栈
                elif pre_op == '-':
                    stack.append(-num)#如果是负号取反入栈
                elif pre_op == '*':
                    stack.append(stack.pop() * num)#如果是乘号，取栈顶计算，将结果入栈
                elif pre_op == '/':
                    top = stack.pop()
                    stack.append(int(top / num))#如果是除号，取栈顶计算，将结果入栈
                pre_op = each#每一步更新pre_op
                num = 0#每次将num清0
        return sum(stack)
```
