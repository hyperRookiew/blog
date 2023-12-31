---
title: LeetCode Hot 100 刷题 [1-10]
categories:
  - 算法
  - LeetCode
tags:
  - LeetCode
  - 算法
date: 2022-07-19 09:49:51
---

# LeetCode Hot 100 [1-10]

>刷完了《剑指Offer》，下一阶段，开始 LeetCode Hot 100。 之前一直都用思维导图记录，今天发现思维导图可以导出成markdown，简单编辑一下就可以成为文章。相比Xmind，博客文章打开和查看更加方便，所以之后每次Xmind记录之后，会再在这里同时同步一下

## 1.两数之和

### 题目

> - 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
- 示例：给定 nums = [2, 7, 11, 15], target = 9，因为 nums[0] + nums[1] = 2 + 7 = 9，所以返回 [0, 1]
- [LeetCode地址](https://leetcode-cn.com/problems/two-sum/)

	
### 解题思路

#### 思路0：排序+双指针（不可行）
- 联想到之前一个题的思路，最开始想到的便是这个思路
- 但需要返回数组下标，所以不能用排序，且复杂度比O(N)高
	
#### 思路1：暴力解法
- 两个for循环，两个数依次都搭配组合一下，直到有和target相等的
- 在没有很好的思路之前，不妨先试试暴力解法

``` java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
          for (int j = i+1; j < nums.length; j++) {
              if(nums[i] + nums[j] == target){
                  return new int[]{i,j};
              }
          }
    }
    return new int[0];
}
```

- 时间复杂度O(N^2)，空间复杂度O(1)
		
#### 思路2：HashMap快速查找
- 暴力解法优化：主要是在元素的查找上耗费，荣国HashMap O(1)的查找能力进行优化
- 思路与过程
	1. 遍历数组，将元素依次以值为key，索引作为value加入到HashMap中
	2. 在将元素加入到HashMap前，计算target-当前元素 = left
	3. 检查hashMap中是否有为left的key，如果有，则返回两个数的下标即可
- 代码实现

```java
public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> hashMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int diff = target - nums[i];
            if(hashMap.containsKey(diff)){
                return new int[]{hashMap.get(target-nums[i]),i};
            }
            hashMap.put(nums[i],i);
        }
        return new int[0];
    }
```

- 时间复杂度O(N)，空间复杂度O(N)

## 2.链表两数加法

### 题目
> - 给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
- 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字 0 之外，这两个数都不会以 0 开头
- 示例
	- 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
	- 输出：7 -> 0 -> 8
	- 原因：342 + 465 = 807
- [LeetCode地址](https://leetcode-cn.com/problems/add-two-numbers/)
	

### 解题思路

- 两个链表逆序的，低位在前，对齐，所以比较容易

- 核心就是模拟加法过程，如何写出优雅的代码

### 思路与过程

1. 遍历两个链表，依次取值参与加法计算，flag记录加法进位
2. 结果中对应位 = ( a + b + flag )% 10
3. 进位flag =  ( a+b+flag ) / 10
4. 如果有链表遍历空，则对应值赋值0，参与计算
  - 直到两个链表均遍历完
5. 链表遍历完之后，还需要检查是否还有进位，如果flag为1，则再进行依次计算

### 代码实现
- 迭代实现

{% codeblock java %}
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode resultHead = new ListNode(0);
        ListNode resCur = resultHead;
        ListNode acurr = l1, bcurr = l2;
        int a = 0, b = 0, flag = 0;
        while (acurr != null || bcurr != null) {
            if (acurr != null) {
                a = acurr.val;
                acurr = acurr.next;
            }else a=0;
            if (bcurr != null) {
                b = bcurr.val;
                bcurr = bcurr.next;
            }else b=0;
            resCur.next = new ListNode((a + b + flag) % 10);
            flag = (a + b + flag) / 10;
            resCur = resCur.next;
        }
        if(flag == 1){
            resCur.next = new ListNode(1);
        }
        return resultHead.next;
    }
{% endcodeblock %}

- 递归实现

```java
	void recur(ListNode l1, ListNode l2, ListNode resCurr,int flag){
        if(l1 == null && l2==null && flag==0) return;
        int a = 0, b = 0;
        if (l1 != null) {
            a = l1.val;
            l1 = l1.next;
        }else {
            a=0;
            l1 = null;
        }
        if (l2 != null) {
            b = l2.val;
            l2 = l2.next;
        }else {
            b=0;
            l2 = null;
        }
        resCurr.next = new ListNode((a + b + flag) % 10);
        flag = (a + b+flag) / 10;
        recur(l1,l2,resCurr.next,flag);
    }
```

### 评估
- 时间复杂度O(N)
- 空间复杂度O(1)

### 问题拓展
- 如果链表是非逆序的，即高位在前？？？
  
  - 递归不好做到逆序的感觉，还是要从低位对齐才开始加法
	
  - 可以使用队列或者栈，实现逆序的效果


##  3.无重复字符的最长子串

### 题目
>- 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
- 输入: "abcabcbb"
- 输出: 3 
    - 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
- [题目地址](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

### 解题思路
- 这个在剑指offer中有原题

#### 思路一：双指针+Set集合判断重复+遍历查找区间
- 简单，略

#### 思路二：HashMap，记录字符上次出现的索引坐标
- 在判断是否重复的同时
- 还可以快速查找到重复元素的坐标
- 代码实现
```java
class Solution {
   public int lengthOfLongestSubstring(String s) {
        char[] chars = s.toCharArray();
        HashMap<Character, Integer> hashMap = new HashMap<>();
        int left = 0, right = 0;
        int maxLength = 0;

        while (right < s.length()) {
            if (hashMap.containsKey(chars[right])) {
                if (hashMap.get(chars[right]) >= left) {
                    left = hashMap.get(chars[right]) + 1;
                }
            }
            hashMap.put(chars[right], right);
            right++;
            maxLength = Math.max(maxLength, right - left);
        }
        return maxLength;

    }
}
```


## 4.寻找两个正序数组的中位数

### 题目
>- 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。
- 请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
- 你可以假设 nums1 和 nums2 不会同时为空。
- [地址](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

### 解题思路

#### 思路一：将两个有序列表先归并成一个有序列表，然后再求中位数
- 时间复杂度O(m+n)
- 空间复杂度O(m+n)
- 不符合题意，但比较简单

#### 思路二：中位数在整个数组中最终的位置索引是已知的，所以不需要对整个数组合并排序
- 如果m+n为偶数，则中位数索引为 (m+n) / 2，以及其左边的
- 如果m+n为奇数，则中位数为(m+n) / 2
- 所以，只要从两个数组中选出前半部分即可
- 时间复杂度仍然为O(m+n)，不可行

#### 思路三：运用二分查找
- 这是运用二分查找的一个比较难的题目
- 算法复杂度要求O(log(m+n))，很容易联想到二分查找
- 思路与过程

##### 1. 查找的区间
- 即需要在两个数组中确定一个分割线
- 即，在A数组中确定分割线后，B数组中分割线也就定下来了 j = (m+n) / 2 - i
- i 和 j 表示分割线右边的索引

##### 2. 查找条件/排除条件
- 分割线左边元素与右边偶数时相等，奇数时少一个
- A左不能大于B右，A右不能小于A左
- 以上两个可以作为排除条件

##### 3. 整个查找过程即
-  在 0 - m-1 上确定 i 的取值
- 最终使得 i 和 j 满足对于的查找条件
- 难就难在有很多特殊的边界情况需要特别考虑

##### 代码实现

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
       if(nums1.length > nums2.length){
            int [] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }
        int left = 0, right = nums1.length;
        int len = nums1.length + nums2.length;
        while (left < right) {
            int i = (left + right+1) / 2; // 向上取整
            int j = len/2 - i;
            if (nums1[i - 1] > nums2[j]) {
                right = i-1;
            } else {
                left = i;
            }
        }
        int i = left;
        int j = len/2 - i;
        int maxLeftNums1 = i == 0? Integer.MIN_VALUE:nums1[i-1];
        int minRightNums1 = i== nums1.length? Integer.MAX_VALUE:nums1[i];
        int maxLeftNums2 = j == 0? Integer.MIN_VALUE:nums2[j-1];
        int minRightNums2 = j==nums2.length? Integer.MAX_VALUE:nums2[j];
        if(( len & 1) == 1){
            return (double) Math.min(minRightNums1,minRightNums2);
        }else{
            return (double) (Math.min(minRightNums1,minRightNums2) + Math.max(maxLeftNums1,maxLeftNums2)) / 2.0;
        }
    }
}
```
				
##### 复杂度
- 时间复杂度O(log(m+n))
- 空间复杂度O(1)


## 5. 最长回文子串

### 题目
>- 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
- 输入: "babad"，输出: "bab"，注意: "aba" 也是一个有效答案。
- 输入: "cbbd"，输出: "bb"

### 解题思路

#### 思路1：中间扩散
- 依次遍历每个字符，以每个字符为中心，向外扩散，直到获得以该字符为中心的最长回文子串
- 需要讨论两种情况
    - 奇数 bab：判断 i-j 和 i+j 位置是否相等
	- 偶数 baab：判断 i-j 和 i-j+1位置是否相等

#### 代码实现
```java
class Solution {
    public String longestPalindrome(String s) {
        int maxLength = 0;
        String maxString = "";

        for(int i =0; i < s.length(); i++){
            int j = 0;
            while(i-j >=0 && i+j < s.length()){
                if(s.charAt(i-j) != s.charAt(i+j)){
                    break;
                }
                j++;
            }
            j--;
            int length = 2 * j + 1;
            if( length > maxLength){
                maxLength = length;
                StringBuilder builder = new StringBuilder(); 
                for(int k = i-j; k <= i+j;k++){
                    builder.append(s.charAt(k));
                }
                maxString = builder.toString();
            }
            j = 0;
            while(i-j >=0 && i+j+1 < s.length()){
                if(s.charAt(i-j) != s.charAt(i+j+1)){
                    break;
                }
                j++;
            }
            j--;
            length = 2 * (j+1);
            if( length > maxLength){
                maxLength = length;
                StringBuilder builder = new StringBuilder(); 
                for(int k = i-j; k <= i+j+1;k++){
                    builder.append(s.charAt(k));
                }
                maxString = builder.toString();
            }
        }
        return maxString;
    }
}
```

#### 算法复杂度
- 时间：O(n^2)
- 空间:O(1)
		
### 思路2：动态规划
#### 1. 定义dp
- dp[i][j] 表示从 i 到 j 是否是回文串

#### 2. 递推方程
- dp[i][j] = dp[ i+1 ][ j-1 ]  && s.charAt(i) == s.charAt(j)

#### 3. 初始状态
- dp[ i ][ i ] = true
- dp[ i ][ i+1 ] = s.charAt(i) == s.charAt(i+1)

#### 4. 填表过程
- 需要先填 i 大的，j 小的，所以从右下角向左上角填表

### 代码实现
```java
	public String longestPalindrome(String s) {
        boolean dp[][] = new boolean[s.length()][s.length()];
        int maxLength = s.length() == 0 ? 0 : 1;
        String maxString = s.length() == 0 ? "" : s.substring(0, 1);
        for (int i = 0; i < s.length(); i++) {
            dp[i][i] = true;
        }
        // 倒三角填表
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = i + 1; j < s.length(); j++) {
                if (s.charAt(i) != s.charAt(j)) {
                    dp[i][j] = false;
                } else {
                    if(j-i == 1) dp[i][j] =true;
                    else dp[i][j] = dp[i + 1][j - 1];
                    if (dp[i][j] && j - i + 1 > maxLength) {
                        maxLength = j - i + 1;
                        maxString = s.substring(i, j + 1);
                    }
                }
            }
        }
        return maxString;
    }
```

### 思路3：manacher算法（马拉车）
- 专门用于解决最长回文子串的算法


## 6. 盛最多水的容器

### 题目
>- 给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
- [地址连接](https://leetcode-cn.com/problems/container-with-most-water/)

### 解题思路
容纳的水最多，即长乘宽的面积最大

#### 思路一：暴力算法
- 每两个整数之间两两组合，求得最大的
- 代码实现

```java
	public int maxArea(int[] height) {

        int max = 0;
        for(int i = 0; i< height.length-1; i++){
            for(int j = i+1; j<height.length;j++){
                int h = Math.min(height[i],height[j]);
                int w = j - i;
                int cap = h * w;
                if(cap > max){
                    max = cap;
                }
            }
        }
        return max;
    }
```

- 复杂度
    - 时间复杂度O(n^2)

#### 思路二：双指针算法
- 这是双指针算法的一个很好的例子
- 双指针算法能够将算法复杂度从O(N^2) 优化到O（N），因为避免了两层遍历
- 高度取决于两个整数中较小的那个，宽度取决于下标间的距离
- 思路过程
	1. 首尾双指针，left和right，从两端出发
	2. 计算当前组合的面积，并和当前最大面积比较
		- 如果大，则更新最大面积以及下标
	3. 继续遍历，缩小较小的边
	    - 因为当前是较小整数被选中作为高的最大面积了
		- 再往内部要么不会被选中，要么被选中了，但是宽度没现在大
		- 所以较小整数可以从遍历的范畴中排除了

- 代码实现

```java
	public int maxArea(int[] height) {
        int left = 0, right = height.length-1;
        int max = 0;
        while (left < right){
            int h = Math.min(height[left],height[right]);
            int w = right - left;
            int cap = h * w;
            if(cap > max) max = cap;
            if(height[left] < height[right]){
                left++;
            }else right--;
        }
        return max;
    }
```

- 算法复杂度
    - 时间复杂度O(N)

## 7.三数之和
### 题目

>- 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
- 注意：答案中不可以包含重复的三元组。
- [地址连接](https://leetcode-cn.com/problems/3sum/)

### 解题思路
- 想到之前的两数问题

#### 思路一：暴力算法
- 三重循环，O(N^3)
- 同时还存在严重的重复问题

#### 思路二：排序+暴力
- 重复问题解决
    - 对数组提前进行排序

- 遍历时候，跳过同样的元素
- 算法复杂度依然是O(N^3)

#### 思路三：排序+双指针
- 重复问题
    - 与思路二一样
- 排序之后，通过比较双指针值和target值，能有效缩小区间
- 思路过程
    1. 第一重循环：选取第一个数 num[ i ]
	    - 从第个数开始，到不满足targe结束
		- 因为排序后，第一个数必须小于等于targe才能满足三数和为target
	2. 第二重循环：从 num[i] 之后的区间，用双指针选择两个数
	    - 如果 num[ left ] + num[ right ] < target - num[i]，说明小了，左边界搜索
		- 反之
	3. 跳过重复元素
	    - 第一重循环，当 i 位置处理结束，要移动到下一个与 i 不一样的元素处
		- 第二重循环，当 left 和 right 移动时候，要移动到与下一个与当前值不一样的位置

- 代码实现

```java
	public List<List<Integer>> threeSum(int[] nums) {
        LinkedList<List<Integer>> list = new LinkedList();

        Arrays.sort(nums);
        int i = 0;

        while (i < nums.length && nums[i] <= 0) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                i++;
                continue;
            }
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    List res = new LinkedList();
                    res.add(nums[i]);
                    res.add(nums[left]);
                    res.add(nums[right]);
                    list.add(res);
                    while (left+1<nums.length && nums[left]==nums[left+1]){
                        left++;
                    }
                    left++;
                    while (right-1>=0 && nums[right] == nums[right-1]){
                        right--;
                    }
                    right--;
                }
            }
            i++;
        }
        return list;
    }
```

- 算法复杂度
    - 时间复杂度O(N)

## 8.电话号码的字母组合

### 题目
>- 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
- 给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
- 输入："23"
- 输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
- [题目地址](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

### 解题思路
- 属于排列组合问题
- 回溯算法遍历所有的情况，多叉树遍历
- 思路过程
    1. StringBuilder 记录从根到叶子节点的路径
	2. 对多叉树进行遍历，每个阶段将字符加入stringbuilder
	3. 回溯时候，撤销之前添加的字符

- 代码实现

```java
class Solution {
    private List<String> result = new LinkedList<>();
    private HashMap<Integer,String> map = new HashMap<>();

    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) return result;
        map.put(2,"abc");
        map.put(3,"def");
        map.put(4,"ghi");
        map.put(5,"jkl");
        map.put(6,"mno");
        map.put(7,"pqrs");
        map.put(8,"tuv");
        map.put(9,"wxyz");
        combinations(digits,0,new StringBuilder());
        return result;
    }
    private void combinations(String digits, int index, StringBuilder builder){
        if(index == digits.length()) {
            result.add(builder.toString());
            return;
        }
        String chars = map.get(digits.charAt(index) - '0');
        for (int i = 0; i < chars.length(); i++) {
            builder.append(chars.charAt(i));
            combinations(digits,index+1,builder);
            builder.deleteCharAt(builder.length()-1);
        }
    }
}
```

- 复杂度
    - 时间复杂度：O(4^n)
    - 空间复杂度：O(4^n)

## 9.有效的括号组合
### 题目
>- 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
- 有效字符串需满足：
    - 左括号必须用相同类型的右括号闭合。
	- 左括号必须以正确的顺序闭合。
- 注意空字符串可被认为是有效字符串。
- 输入: "([)]"
- 输出: false
- [题目地址](https://leetcode-cn.com/problems/valid-parentheses/)

### 解题思路
- 通过栈来暂存左括号，进行匹配
- 思路过程
    1. 读取字符
	    - 如果是左括号，则直接入栈
		- 如果是右括号，步骤2
	
    2. 如果是右括号，检查栈顶是否是该右括号的左括号
	    - 是，则弹出栈顶，读取下一个
		- 不是，则匹配失败，false
	
    3. 最终还需要检查栈是否为空，避免最后一个字符是左括号

- 代码实现

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (isLeft(s.charAt(i))) {
                stack.push(s.charAt(i));
            } else if (isRight(s.charAt(i))) {
                if(!stack.isEmpty()&&isLeftMatchRight(stack.peek(),s.charAt(i))){
                    stack.pop();
                }else { 
                    return false;
                }
            } else {
                return false;
            }
        }
        return stack.isEmpty();

    }

    private boolean isLeft(char c) {
        return c == '(' || c == '{' || c == '[';
    }

    private boolean isRight(char c) {
        return c == ')' || c == '}' || c == ']';
    }

    private boolean isLeftMatchRight(char left, char right) {
        return left == '(' && right == ')'
                || left == '[' && right == ']'
                || left == '{' && right == '}';
    }
}
```

- 复杂度
    - 时间复杂度O(N)
	- 空间复杂度O(N)

## 10. 删除链表的倒数第N个节点
### 题目
>- 给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
- 示例：给定一个链表: 1->2->3->4->5, 和 n = 2.
    - 当删除了倒数第二个节点后，链表变为 1->2->3->5.

- 给定的 n 保证是有效的。
- 你能尝试使用一趟扫描实现吗？
- [题目地址](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 解题思路
#### 思路一：两次遍历
- 第一次遍历，统计链表个数
- 第二次遍历，删除倒数第N个节点
- 算法复杂度：O(N)

#### 思路二：HashMap遍历+一次遍历
- 遍历链表，将位置和节点映射关系存入hashmap，同时计数
- 计算倒数第N个节点的索引，直接删除

#### 思路三：递归实现
- 只需要遍历一次即可
- 思路过程
	1. 递归返回值表示当前倒数第几个节点
	2. 如果head == null， ruturn 0；
	3. 如果 返回值恰好为N，则删除该节点
	    - head.next = head.next.next;
	4. 为了方便删除头节点，适用一个哨兵
	    - ListNode tempHead = new ListNode(1);
        - tempHead.next = head;

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode tempHead = new ListNode(0);
        tempHead.next = head;
        recur(tempHead,n);
        return tempHead.next;
    }

    private int recur(ListNode head, int n){
        if(head == null) return 0;
        int num = recur(head.next,n);
        if(num == n){
            // 删除
            head.next = head.next!=null ? head.next.next : null;
        }
        return num+1;
    }
}
```

- 复杂度
    - 时间复杂度O(N)
	- 空间复杂度O(N)

## 下个路口见
HOT100依然在继续，写在下一篇文章中。[传送门](http://blog.zwboy.cn/ji-zhu/leetcode-hot-100-shua-ti-11-20.html)