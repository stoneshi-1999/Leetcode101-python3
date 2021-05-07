# Leetcode 题解 - 哈希表
<!-- GFM-TOC -->
* [Leetcode 题解 - 哈希表](#leetcode-题解---哈希表)
    * [1. 数组中两个数的和为给定值](#1-数组中两个数的和为给定值)
    * [2. 判断数组是否含有重复元素](#2-判断数组是否含有重复元素)
    * [3. 数组中重复的数字](#3-数组中重复的数字)
    * [4. 最长和谐序列](#4-最长和谐序列)
    * [5. 最长连续序列](#5-最长连续序列)
<!-- GFM-TOC -->

1.**哈希（Hash）**，一般翻译做散列、杂凑，或音译为哈希，是把**任意长度的输入（又叫做预映射pre-image）通过散列算法变换成固定长度的输出**，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来确定唯一的输入值。简单的说就是一种将**任意长度的消息压缩到某一固定长度的消息摘要**的函数。

2.**哈希函数（散列函数）**，能够将任意长度的输入值转变成固定长度的值输出，该值称为散列值，输出值通常为字母与数字组合。

3.**[哈希表](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E8%A1%A8/10027933)（Hash table，也叫散列表）**，是根据关键码值(Key value)而直接进行访问的[数据结构](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/1450)。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做[散列函数](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E5%87%BD%E6%95%B0/2366288)，**存放记录的[数组](https://baike.baidu.com/item/%E6%95%B0%E7%BB%84/3794097)叫做[散列表](https://baike.baidu.com/item/%E6%95%A3%E5%88%97%E8%A1%A8/10027933)。**     
                                                                       
**给定表M，存在函数f(key)，对任意给定的关键字值key，代入函数后若能得到包含该关键字的记录在表中的地址，则称表M为哈希(Hash）表，函数f(key)为哈希(Hash) 函数。**

## 1. 数组中两个数的和为给定值

1\. Two Sum (Easy)

[力扣](https://leetcode-cn.com/problems/two-sum/description/)/[题解](https://leetcode-cn.com/problems/two-sum/solution/liang-shu-zhi-he-by-leetcode-solution/) 

用查找表法，查找表有两种：哈希表和平衡二叉搜索树。因为不用保存顺序，所以用哈希表。

用hashtable(一个字典)存储数组元素和索引的映射，在访问到 nums[i] 时，判断hashtable中是否存在 target - nums[i]，如果存在说明 target - nums[i] 所在的索引和 i 就是要找的两个数。该方法的时间复杂度为 O(N)，空间复杂度为 O(N)，**使用空间来换取时间。**

dict() 函数用于创建一个字典。

>dict()# 创建空字典   
>输出：{}

> dict(a='a', b='b', t='t')# 传入关键字  
> 输出：{'a': 'a', 'b': 'b', 't': 't'}

>dict(zip(['one', 'two', 'three'], [1, 2, 3]))   # 映射函数方式来构造字典  
>输出：{'three': 3, 'two': 2, 'one': 1} 

>dict([('one', 1), ('two', 2), ('three', 3)])    # 可迭代对象方式来构造字典  
>输出：{'three': 3, 'two': 2, 'one': 1}

[enumerate() 函数](https://www.runoob.com/python/python-func-enumerate.html)用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出**数据下标和数据**，一般用在 for 循环当中。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashtable = dict()
        for i, num in enumerate(nums):
            if target - num in hashtable:#target - num的计算结果在哈希表里的键key中，所以键key为数值，值value为下标
                return [hashtable[target - num], i]#找出target - num的下标和num对应的下标（注意num和nums的区别！）
            hashtable[nums[i]] = i#最后输出的是下标，所以哈希表里键key为数值，值value为下标
        return []
```

## 2. 判断数组是否含有重复元素

217\. Contains Duplicate (Easy)

 [力扣](https://leetcode-cn.com/problems/contains-duplicate/description/)/[题解](https://leetcode-cn.com/problems/contains-duplicate/solution/cun-zai-zhong-fu-de-yuan-su-yi-yi-ti-san-ihfa/)


 [set() 函数](https://www.runoob.com/python/python-func-set.html/) 创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。

```python
class Solution:
	def containsDuplicate(self, nums: List[int]) -> bool:
	    return len(nums) != len(set(nums))

```

## 3. 数组中重复的数字

03\. 数组中重复的数字 (Easy)

 [力扣](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)/[题解](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/) 

[add() 方法](https://www.runoob.com/python3/ref-set-add.html/)用于给集合添加元素，如果添加的元素在集合中已存在，则不执行任何操作。

```python
class Solution:
    def findRepeatNumber(self, nums: [int]) -> int:
        dic = set()
        for num in nums:
            if num in dic: 
            	return num#在dic中了，则输出来
            dic.add(num)#如果num不在dic中则加入dic
        return -1
```
复杂度分析：
时间复杂度 O(N) ： 遍历数组使用 O(N) ，HashSet 添加与查找元素皆为 O(1) 。
空间复杂度 O(N) ： HashSet 占用 O(N) 大小的额外空间。


## 4. 最长和谐序列

594\. Longest Harmonious Subsequence (Easy)

[Leetcode](https://leetcode.com/problems/longest-harmonious-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/longest-harmonious-subsequence/description/)

```html
Input: [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```

和谐序列中最大数和最小数之差正好为 1，应该注意的是序列的元素不一定是数组的连续元素。

解题思路：  
利用哈希表存储数组中元素出现的次数。  
遍历哈希表，如果当前元素+1也在哈希表中，那么计算两者次数之和，保留最大值。  

字典 get() 函数返回指定键的值。
语法：
dict.get(key, default=None)
参数：
key -- 字典中要查找的键。
default -- 如果指定的键不存在时，返回该默认值。

```python
class Solution:
    def findLHS(self, nums: List[int]) -> int:
        dicts={}#用键key存num数，用值value存num数的出现次数。
        for num in nums:
            dicts[num]=dicts.get(num,0)+1#一开始dicts为空，没有元素（没有i），所以返回值为0，需要+1！
        res=0
        for num in dicts:
            if num+1 in dicts:
                res=max(res,dicts[num]+dicts[num+1])
        return res
```

## 5. 最长连续序列

128\. Longest Consecutive Sequence (Hard)

[Leetcode](https://leetcode.com/problems/longest-consecutive-sequence/description/) / [力扣](https://leetcode-cn.com/problems/longest-consecutive-sequence/description/)

```html
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.
```

要求以 O(N) 的时间复杂度求解。

 +=, 加法赋值运算符, c += a 等效于c = c + a 

```python
class Solution:
    def longestConsecutive(self, nums):
        longest_streak = 0
        num_set = set(nums)#去掉重复的元素

        for num in num_set:
            if num - 1 not in num_set:#保证开始计算序列长度时，是连续序列的起点（没有比num小1的值）
                current_num = num#满足条件时，起点即为num
                current_streak = 1#此时的序列长为1

                while current_num + 1 in num_set:#注意这里是CURRENT_NUM+1!
                    current_num += 1#序列数+1（因为是连续序列）
                    current_streak += 1#序列长度+1

                longest_streak = max(longest_streak, current_streak)

        return longest_streak
```
