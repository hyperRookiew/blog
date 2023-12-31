---
title: 'LeetCode Hot 100 刷题 [11-20]'
categories:
  - 算法
  - LeetCode
tags:
  - LeetCode
  - 算法
date: 2022-07-23 10:21:37
---

# LeetCode Hot 100 [11-20]

>刷完了《剑指Offer》，下一阶段，开始 LeetCode Hot 100。 之前一直都用思维导图记录，今天发现思维导图可以导出成markdown，简单编辑一下就可以成为文章。相比Xmind，博客文章打开和查看更加方便，所以之后每次Xmind记录之后，会再在这里同时同步一下

## 前言
HOT100，之前的文章传送门
[LeetCode Hot 100 刷题 [1-10]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot100-shua-ti-1-10.html)


## 11. 合并两个有序链表
### 题目
> - 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。
    - 输入：1->2->4, 1->3->4
    - 输出：1->1->2->3->4->4
- [题目地址](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

### 解题思路
- 和两个有序数组的合并是类似的
- 链表有迭代和递归两种实现方式

#### 迭代实现
- 思路过程
    1. 两个链表均非空，选择较小头节点进行归并
    2. 当有一个为空了，将非空的直接尾插到结果链表种

- 代码实现
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode tempHead = new ListNode(1);
        ListNode curr = tempHead;
        while(l1 != null && l2 != null){
            if(l1.val < l2.val){
                curr.next = l1;
                l1 = l1.next;
            }else{
                curr.next = l2;
                l2 = l2.next;
            }
            curr =  curr.next;
        }
        if(l1 == null){
            curr.next = l2;
        }else {
            curr.next = l1;
        }
        return tempHead.next;
    }
}
```

- 复杂度
  - 时间复杂度O(M+N)

#### 递归实现
- 思路与过程
    - 将两个链表合并的过程看作，
        1. 先选出一个较小头节点
        2. 再对剩余节点进行合并，结果赋值给较小节点的next
    - 当l1头节点较小时，l1.next = recur(l1.next, l2)
    - 当l2头节点较小时，l2.next = recur(l1，l2.next)

- 代码实现
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next,l2);
            return  l1;
        }else{
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
```

- 复杂度
    - 时间复杂度O(M+N)
    - 空间复杂度O(M+N)


## 12.括号生成
### 题目

>- 数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
- 输入：n = 3
- 输出： 
```java
[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```
- [题目地址](https://leetcode-cn.com/problems/generate-parentheses/)

### 解题思路
#### 思路一:排列组合问题
- 属于括号的排列组合, 但不是所有的排列都满足要求
- 通过DFS实现排列组合的遍历过程
- 回溯:试过左括号之后,再尝试右括号,需要回溯之前的变更
- 剪枝:当肯定不满足时,剪枝
    - 当右括号数目大于左括号数目,则必不行
- 判定括号是否匹配
    - balance
	- 左括号+1
	- 右括号-1
	- 遍历字符串过程中,balance必须大于等于0, 出现小于0,则不匹配
	- 且最终balance=0, 才能保障括号匹配
- 代码实现

```java
class Solution {
    List<String> list = new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        // 一共2n个字符
        char[] values = new char[2*n];
        dfs(values,0,2*n,0);
        return list;
    }
    
    private void dfs(char [] values,int index,int end,int balance){
        if(index == end){
            if(balance==0){
                list.add(new String(values));
            }
        }else{
            values[index] = '(';
            dfs(values,index+1,end,balance+1);
            if(balance-1 < 0) return;
            values[index] = ')';
            dfs(values,index+1,end,balance-1);
        }
    }
}
```

- 复杂度
    - 时间复杂度O(2^N)
	- 空间复杂度O(2^N)

### 思路二:动态规划问题

## 13.合并k个有序链表
### 题目
>- 合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。
- 输入:
```java
[
  1->4->5,
  1->3->4,
  2->6
]
```
- 输出: `1->1->2->3->4->4->5->6`
- [题目地址](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

### 解题思路
#### 思路1:归并合并
- k个有序链表,两两组合,进行合并
- 在对合并后的两个链表,再此进行两两合并,得到最终的结果
- 代码实现
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int index = 0;
        
        int len = lists.length;
        
        while (len > 1){
            for (int i = 0; i < len; i++) {
                int pos = 2 * i;
                if(pos+1 < len){
                    lists[index] = mergeTwoLists(lists[pos],lists[pos+1]);
                    index++;
                }else if(pos < len){
                    lists[index] = lists[pos];
                    index++;
                    break;
                }
            }
            len = index;
            index=0;
        }
        return lists.length==0?null : lists[0];
    }

    // 21题中的两两合并
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode tempHead = new ListNode(1);
        ListNode curr = tempHead;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }
        if (l1 == null) {
            curr.next = l2;
        } else {
            curr.next = l1;
        }
        return tempHead.next;
    }
}
```

- 复杂度
    - 时间复杂度O(kNlogK)

#### 思路2: 优先级队列
- 和合并两个有序链表相比,不同的地方
	- 两个链表中选择最小的头节点
    - k个链表中选择最小的头节点
- k个中选最小,可以通过最小堆来实现PriorityQueue
    - 需要提供Comparator
	- int compare(o1, o2)
	- o1 - o2 < 0 则o1优先级更高
- 代码实现
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode result = new ListNode(1);
        ListNode curr = result;
        PriorityQueue<ListNode> queue = new PriorityQueue<ListNode>(new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val - o2.val;
            }
        });
        for (int i = 0; i < lists.length; i++) {
            if(lists[i] != null){
                queue.add(lists[i]);
            }
        }
        while (!queue.isEmpty()){
            ListNode node = queue.poll();
            if(node.next != null){
                queue.add(node.next);
            }
            node.next=null;
            curr.next=node;
            curr = curr.next;
        }
        return result.next;
    }
}
```
				
- 复杂度
	- 时间复杂度 O(kNlogk)
	- 空间复杂度O(k)

## 14.下一个排列
### 题目
> - 实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
- 如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
- 必须原地修改，只允许使用额外常数空间。
- 以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
```java
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```
- [题目地址](https://leetcode-cn.com/problems/next-permutation)

### 解题思路
- 这道题需要观察总结排列数生成中的过程和规律，然后编码实现该过程
- 基础原理就是将第一个大于其前一个的数换到前边，得到稍大于当前排列的值
- 思路与过程
    1. 从右向左遍历排列，找到第一个山峰（即由递增转递减）
	    - 如果该元素为最左边元素，则直接对整个数组进行排序即可
		- 如果该元素为最右边元素，则将该元素与倒数第二个元素交换即可
		- 其他，则走一下步骤
	2. 查找到山峰索引为pos，接下来需要从[pos，end]区间中选择一个合适的元素，与 pos-1 位置的交换，（即，选出新的较大的前者）
	    - 该元素首先必须要比 pos-1 位置的要大，满足字典增序的要求
		- 该元素为[ pos, end ] 区间中满足上输条件的最小值，即最接近 pos-1 的值
	3. 将新元素和 pos-1 交换之后，对 [ pos-1 , end] 再进行依次排序
	4. 即可得到结果
- 代码实现
```java
class Solution {
    public void nextPermutation(int[] nums) {
        if(nums.length <= 1) return;
        // 从右向左，第一个增序变减序
        int pos = nums.length-1;
        while(pos > 0){
            if(nums[pos] > nums[pos-1]){
                break;
            }
            pos--;
        }
        if(pos == 0) {
            Arrays.sort(nums);
            return;
        }
        if(pos == nums.length-1) {
            swap(nums,pos,pos-1);
            return;
        }
        // 从pos之后选择大于其前边数字的最小数
        int selectIndex = pos;
        for(int i = pos+1; i < nums.length; i++){
            if(nums[i] > nums[pos-1] && nums[i] < nums[selectIndex]){
                selectIndex = i;
            }
        }
        swap(nums,selectIndex,pos-1);
        Arrays.sort(nums,pos,nums.length);
        return;
    }

    private void swap(int [] nums, int aIndex, int bIndex){
        int temp = nums[aIndex];
        nums[aIndex] = nums[bIndex];
        nums[bIndex] = temp;
    }
}
```

- 复杂度分析
    - 时间复杂度：O(NlogN)

## 15. 最长有有效括号
### 题目
>- 给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。
- 输入: "(()"
- 输出: 2
- 解释: 最长有效括号子串为 "()"
- [题目地址](https://leetcode-cn.com/problems/longest-valid-parentheses)

### 解题思路
#### 思路0. 如何判断括号串是否有效
- balance变量
    - 左括号，balance++
	- 右括号，balance--
- 遍历过程中，如果balance出现 < 0，则必不是有效的
- 遍历结束，只有当balance=0，才说明是有效的

#### 思路1. 左右指针遍历
- 思路与过程
    1. 先从左向右遍历，统计balance
	    - left、right初始均为0
		- 如果balance == 0，则计算一次length，更新maxLength
		- 如果balance < 0，则 left和right直接跳到下一个字符重新统计
	2. 再从右向左遍历，统计balance
		- 因为遍历时候，只有当balance==0的时候，才会统计length
		- 如果遍历结束，最终balance一直维持在 > 0，则不会被统计lenth
		- 但，balance > 0 不代表没有有效括号，只是很多的左括号没有右括号来匹配它就结束了
		- 所以需要再从右向左遍历一次，判断过程和从左向右恰好相反

- 代码实现
```java
	public int longestValidParentheses(String s) {
        int maxLength = 0;
        int left = 0, right =0;
        int balance = 0;
        while(right < s.length()){
            if(s.charAt(right) == '('){
                balance++;
            }else {
                balance--;
                if(balance== 0){
                    int length = right - left + 1;
                    maxLength = Math.max(maxLength,length);
                }else if(balance < 0){
                    left = right+1;
                    balance=0;
                }
            }
            right++;
        }
        // 逆序再来一次
        left = s.length() -1;
        right = left;
        balance = 0;
        while(left >= 0 ){
            if(s.charAt(left) == ')'){
                balance++;
            }else {
                balance--;
                if(balance== 0){
                    int length = right - left + 1;
                    maxLength = Math.max(maxLength,length);
                }else if(balance < 0){
                    right = left - 1;
                    balance=0;
                }
            }
            left--;
        }
        return maxLength;
    }
```

#### 思路2. 动态规划
- dp[ i ] 表示以 i 为结尾的最长有效括号的长度
- 递推方程：dp[ i ] =
    - 0 
        - 当s[ i ] == ' ( '，以左括号结尾，长度肯定为0
	- dp[ i-2 ] + 2
		- 以 ' ) ' 结尾，且，前一个括号为 ' ( '
		- 在原有长度上，续上2
		- 其实这种场景下，dp[ i-1 ] 必定为0
	- dp[ i-1 ] + 2 + dp[ i - dp[i-1] + 1 ]
	    - 以 ）结尾，且前一个括号为）
		- 跳过dp[ i-1 ]对应子串的长度，查看该子串的前一个字符是否是 （，如果是，则可以和目前的）匹配
	    - 匹配，则+2
		- 同时，匹配后，可能将（前边的子串能连续起来，所以，再加上 （ 前边位置的子串长度，即+dp[ i - dp[i-1] + 1 ]
- 代码实现
```java
	public int longestValidParentheses(String s) {
        int [] dp = new int[s.length()];
        //dp[i] 表示以下标 ii 字符结尾的最长有效括号的长度
        int maxLength = 0;

        for(int i = 1; i< s.length(); i++){
            if(s.charAt(i) == ')' && s.charAt(i-1) == ')'){
                int len = dp[i-1];
                int pre = i - len - 1; // 前一个待匹配的左括号
                if( pre < 0) continue;
                if(s.charAt( pre ) == '('){
                    int preDP = pre == 0? 0: dp[pre-1];
                    dp[i] = dp[i-1] + 2 + preDP;
                }
            }else if(s.charAt(i) == ')' && s.charAt(i-1) == '('){
                dp[i] = i==1 ? 2: dp[i-2] + 2;
            }
            maxLength = Math.max(maxLength, dp[i]);
        }
        return maxLength;
    }
```

- 复杂度分析
	- 时间复杂度O(N)
	- 空间复杂度O(N)

#### 思路3. 栈实现
- 栈中存储的元素为读取到的括号所对应的索引下标
- 思路与过程
    - 用栈来记录左右符号的匹配过程
    - 过程
	    1. 如果读取到左括号，则将该左括号的索引入栈
		2. 如果读取到右括号，则弹出栈顶元素与其进行匹配
		    - 同时计算已经匹配的子串的长度
			- length = 右括号索引 - 前一个左括号索引
			    - 前一个左括号索引，即栈中左括号的下一个元素
			- 在生成length过程中，记录最大值即可
		3. 当读取到右括号，且栈为空，将右括号索引入栈

- 代码实现
```java
	public int longestValidParentheses(String s) {
        // 用栈来实现
        Stack<Integer> stack = new Stack();
        stack.push(-1);
        int maxLength = 0;
        for(int i = 0; i<s.length(); i++ ){
            if(s.charAt(i) == '('){
                stack.push(i);
            }else{
                if(!stack.isEmpty()){
                    stack.pop();
                    if(!stack.isEmpty()){
                        int pos = stack.peek();
                        int length = i - pos;
                        maxLength = Math.max(maxLength, length);
                    }else{
                        stack.push(i);
                    }
                }
            }
        }

        return maxLength;
    }
```

- 复杂度分析
    - 时间复杂度O(N)
	- 空间复杂度O(N)

## 16. 搜索旋转排序数组
### 题目
>- 假设按照升序排序的数组在预先未知的某个点上进行了旋转。
- ( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
- 搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
- 你可以假设数组中不存在重复的元素。
- 你的算法时间复杂度必须是 O(log n) 级别。
- 输入: nums = [4,5,6,7,0,1,2], target = 0
- 输出: 4
- [题目地址](https://leetcode-cn.com/problems/search-in-rotated-sorted-array)

### 解题思路
- 排序数组、O(logN)：显而易见，基于二分查找解题
- 查找元素，所以基于模板一
- 旋转数组，需要额外考虑旋转点的位置

#### 1.思路与过程
1. 如果 mid 位置等于 target，直接返回 mid
2. 如果 mid 位置大于 target
    - 正常情况下，接下来应该选择左侧区间
	- 考虑选择右边区间的情况
		- target元素被包含在较小段，并被旋转到了数组右边了
		- 两种情况
			- 旋转点在mid右侧
                - 只有这种情况下，才是向右搜索
                - 判断条件
                    1. nums[ left ] <= nums[ mid ]
                        - 左边是单调递增的
                        - 说明旋转点在右区间
                        - 需要包括等于的情况，否则会死循环
                            - 没有左区间的时候，向右搜索，避免进入死循环
                            - 因为mid向下取整，当区间长度为2的时候，left == mid
                    2. nums[left] > target
                        - 左边最小比target大
                            - 说明更小的被旋转到右边了
			- 旋转点在mid左侧
				- 还是向左搜索，所以只要排除上一种情况即可
3. 如果 mid 位置小于 target
    - 正常情况下，接下来应该选择右侧区间
	- 考虑选择左侧区间的情况，即，旋转点位于mid左侧，target在左侧的大值区间中
	- 判断条件
	    - nums[ right ] > nums [ mid ]
		    - 右侧区间是单调的，说明旋转点在mid或者mid左侧
		- nums[ right ] < target
		    - 右侧的最大值比target小，所以右侧肯定搜索不到
	- 搜索左侧
		- right = mid - 1
- 代码实现
```java
	public int search(int[] nums, int target) {
        if (nums.length == 0) return -1;
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right-left )/2;
            if(nums[mid] == target){ 
                return mid;
            }else if(nums[mid] < target){
                if(nums[mid] < nums[left] && nums[right] < target){
                    right = mid -1;
                }else{
                    left = mid+1;
                }
            }else{
                if(nums[mid] >= nums[left] && nums[left] > target){
                    left = mid+1;
                }else{
                    right = mid -1;
                }
            }
        }
        return -1;
    }
```
- 时间复杂度O(logN)


## 17.在排序数组中查找元素的第一个和最后一个
### 题目
>- 给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
- 你的算法时间复杂度必须是 O(log n) 级别。
- 如果数组中不存在目标值，返回 [-1, -1]。
- [题目地址](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array)

### 解题思路
- 最最典型的二分查找模板二的用法
- 找第一个: 即查找大于等于target元素的下标
    - 即排除小于target的
- 找最后一个:即查找小于等于target元的的下标
    - 即排除大于target的
- 代码实现
```java
	// 找第一个大于等于
    private int findLowBound(int[] nums, int target){
        int left = 0, right = nums.length-1;
        while (left<right){
            int mid = left + (right-left) / 2;
            if(nums[mid] < target){
                left = mid + 1;
            }else {
                right = mid;
            }
        }
        return nums[left] != target? -1:left;
    }
    // 第一个小于等于
    private int findUpBound(int [] nums,int target){
        int left = 0, right = nums.length-1;
        while (left<right){
            int mid = left + (right-left+1) / 2;
            if(nums[mid] > target){
                right = mid - 1;
            }else {
                left = mid;
            }
        }
        return nums[left] != target? -1:left;
    }
```

## 18.组合总和
### 题目
>- 给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
- 所有数字（包括 target）都是正整数。
- candidates 中的数字可以无限制重复被选取。
- 解集不能包含重复的组合。 
- 输入：candidates = [2,3,6,7], target = 7,
- 所求解集为：
- ```java
[
  [7],
  [2,2,3]
]
```
- [题目地址](https://leetcode-cn.com/problems/combination-sum)
	
### 解题思路
- 是一个排列组合问题，基于DFS + 回溯为框架
- 同时，可以在DFS过程中，进行剪枝
    - 当和大于target的时候，剪枝
- 思路与过程
	1. dfs参数设计
	    - sum，记录当前累加和
		- path，记录当前路径
		- index，记录当前已经尝试到了candidates中的哪个数了
		    - 在之后的dfs中，不会再选择index之前的元素了，这样避免了重复情况
	2. dfs中，当sum == target，则说明是一个结果
	    - 基于path中的内容新创建一个list
		- 不能直接添加path到结果列表中，因为path之后还会回溯，添加做到深拷贝
	3. dfs中，让sum > target，则剪枝
	4. 否则，遍历candidats数组，依次尝试每个数
	    - sum += canditates[i]
		- path.add(canditates[i])
		- dfs(index+1，path，sum)
		- 回溯，撤销更改，继续尝试下一个
		- sum -= canditates[i]
		- path.removeLast()
- 代码实现

```java
	private void dfs(int [] candidates ,LinkedList<Integer> ans,int index,int sum, int target){
        if(sum == target){
            result.add(new LinkedList(ans));
            return;
        }
        if(sum > target){
            return;
        }
        for(int i = index; i < candidates.length; i++){
            sum += candidates[i];
            ans.addLast(candidates[i]);
            dfs(candidates,ans,i,sum,target);
            ans.removeLast();
            sum -= candidates[i];
        }
    }
```

- 复杂度分析
    - 时间复杂度,O(N^k)
	- 空间复杂度,O（1）

## 19.全排列
### 题目
>- 给定一个 没有重复 数字的序列，返回其所有可能的全排列。
- 输入: [1,2,3]
- 输出:
```java
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```
- [题目地址](https://leetcode-cn.com/problems/permutations)

### 解题思路
- 这是典型的全排列问题

#### 思路一：DFS + 回溯+解决重复问题
- 思路过程
    1. path存储一次排列结果，path长度 == length时候，添加到结果列表中
	2. 每次排列从数组中选择一个数，并添加到path中
	3. 避免重复：添加之前，需要检查path中是否已经有该元素了
	4. 回溯刚才选择的元素，尝试添加下一个
- 代码实现
```java
	private void dfs(int [] nums, List<Integer> path ){
        if(path.size() == nums.length){
            result.add(new ArrayList<>(path));
            return;
        }
        for (int num:nums) {
            if(!path.contains(num)){
                path.add(num);
                dfs(nums,path);
                path.remove(path.size()-1);
            }
        }
    }
```

- 复杂度分析
    - 时间复杂度O(N! * N)
	-   空间复杂度O(N!)

#### 思路二：基于标记的回溯
- 通过一个标记数组来决定当前数是否被选择过，以此来解决重复问题
- 每次遍历选择一个没有被标记的元素
- 和思路一基本类似，只是通过一个标记数组来判断该元素是否使用过了

#### 思路三：基于交换的回溯
- 空间复杂度最低，直接基于已有的数组
- 不过最终的排序结果不是按字典序排列的，如果需要，请使用思路一或者二
- index表示当前在尝试第几个位置的元素
- 代码实现
```java
	private void dfs(int [] nums,int index ){
        if(index == nums.length){
            result.add(IntStream.of(nums).boxed().collect(Collectors.toList()));
            return;
        }
        for(int i=index; i< nums.length; i++){
            swap(nums,index,i);
            dfs(nums,index+1);
            swap(nums,index,i);
        }
    }
```

## 20.最大子序和
### 题目
>- 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
- 输入: [-2,1,-3,4,-1,2,1,-5,4]
- 输出: 6
- 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
- [题目地址](https://leetcode-cn.com/problems/maximum-subarray)

### 解题思路
#### 思路一：动态规划
- dp[ i ] 表示以 i 位置为结尾的数列的最大子序和
- dp[ i ] = 
    - num[ i ] ，当dp[ i-1 ] <= 0
	- dp[ i-1 ] + nums[ i ] ，当dp[ i-1 ] > 0
- 代码实现
```java
    public int maxSubArray(int[] nums) {
        int [] dp = new int [nums.length];
        dp[0] = nums[0];
        int maxSum = dp[0];
        for(int i = 1; i<nums.length; i++){
            if(dp[i-1] > 0){
                dp[i] = dp[i-1] + nums[i];
            }else{
                dp[i] = nums[i];
            }
            maxSum = Math.max(maxSum, dp[i]);
        }
        return maxSum;
    }
```

#### 思路二：分治算法，线段树
- 待定

## 下个路口见
HOT100依然在继续，写在下一篇文章中。[传送门](http://blog.zwboy.cn/ji-zhu/leetcode-hot-100-shua-ti-21-30.html)