---
title: 'LeetCode Hot 100 刷题 [31-40]'
categories:
  - 算法
  - LeetCode
tags:
  - LeetCode
  - 算法
date: 2022-08-02 14:23:47
---

# LeetCode Hot 100 [31-40]

>刷完了《剑指Offer》，下一阶段，开始 LeetCode Hot 100。 之前一直都用思维导图记录，今天发现思维导图可以导出成markdown，简单编辑一下就可以成为文章。相比Xmind，博客文章打开和查看更加方便，所以之后每次Xmind记录之后，会再在这里同时同步一下

## 前言
HOT100，之前的文章传送门
[LeetCode Hot 100 刷题 [1-10]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot100-shua-ti-1-10.html)
[LeetCode Hot 100 刷题 [11-20]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot-100-shua-ti-11-20.html)
[LeetCode Hot 100 刷题 [21-30]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot-100-shua-ti-21-30.html)

## 31. 求所有子集
### 题目
>- 给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
- 说明：解集不能包含重复的子集。
- 输入: nums = [1,2,3]
- 输出:
```java
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
- [link](https://leetcode-cn.com/problems/subsets/)

### 解题思路
#### 解法1：集合迭代扩充
- 原理：每次向集合中新加入一个元素，新元素与已有元素组合形成新的元素
- 思路过程
1. 首先，集合内无元素，所以只有一个空子集
2. 依次将元素加入到集合中，每次加入后，新元素与已有子集成新的子集
    - 遍历已有的子集，每个子集后边追加新元素，然后加入到集合中
- 3. 直到所有的元素都加入到了集合中
- 代码实现
```java
	public List<List<Integer>> subsets(int[] nums) {
        ArrayList<List<Integer>> result = new ArrayList();
        result.add(new ArrayList<Integer>());
        for(int i = 0; i < nums.length; i++){
            int size = result.size();
            for(int j = 0; j < size; j++){
                ArrayList<Integer> ans = new ArrayList(result.get(j));
                ans.add(nums[i]);
                result.add(ans);
            }
        }
        return result;
    }
```
- 复杂度分析
    - 时间复杂度O(N* 2^N)
	- 空间复杂度O(1)

#### 解法2：DFS + 回溯
- 对不同长度的子集分别进行遍历
- `dfs(int start, int len，List path)`
	- start，子集候选元素的起始区间
	- len，子集剩余长度
	- path，目前子集中的元素
- 回溯过程
    - 依次尝试从start 开始的各个元素，加入到path中
	- 尝试后，下次dfs的start从该元素的下个开始， len-1
	- 尝试结束后，回溯
- 代码实现
```java
class Solution {
    // 回溯算法
    ArrayList<List<Integer>> result;
    public List<List<Integer>> subsets(int[] nums) {
        result = new ArrayList();
        ArrayList<Integer> ans = new ArrayList();
        for(int i = 0 ; i <= nums.length; i++ ){
            dfs(nums,0,i,ans);
        }
        return result;
    }

    private void dfs(int [] nums, int start, int len, List<Integer> ans){
        if(len==0){
            result.add(new ArrayList(ans));
            return;
        }
        for(int i = start; i < nums.length-len+1; i++){
            ans.add(nums[i]);
            dfs(nums, i+1, len-1, ans);
            ans.remove(ans.size()-1);
        }
    }
}
```
- 复杂度分析
    - 时间复杂度O(N*2^N)
	- 空间复杂度O(N*2^N)

#### 解法3：二进制位掩码法
- 通过生成从 0...0 到 1....1 的二进制掩码，掩码每一位对应该元素是否被选中
- **如何生成从 0...0 到 1...1的所有掩码**
    - 设掩码需要 N 位
	- 则，0 <= num < pow(2, N)
- **如何读取判断掩码每一位是否为1**
	- 右移
	- 与1
	```java
        while( bitMask != 0 ){
            if((bitMask&1) == 1){
                ans.add(nums[index]);
            }
            index++;
            bitMask = bitMask >> 1;
        }
    ```

- 代码实现
```java
    ArrayList<List<Integer>> result;
    public List<List<Integer>> subsets(int[] nums) {
        result = new ArrayList();
        int max = (int)Math.pow(2, nums.length);
        for(int i = 0; i < max; i++){
            ArrayList<Integer> ans = new ArrayList();
            int index =0;
            int bitMask = i;
            while( bitMask != 0 ){
                if((bitMask&1) == 1){
                    ans.add(nums[index]);
                }
                index++;
                bitMask = bitMask >> 1;
            }
            result.add(ans);
        }
        return result;
    }
```
- 复杂度分析
    - 时间复杂度O(N * 2^N)
	- 空间复杂度O(1)

## 32. 矩形中搜索单词
### 题目
>- 给定一个二维网格和一个单词，找出该单词是否存在于网格中。
- 单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
```java
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```
- 给定 word = "ABCCED", 返回 true
- 给定 word = "SEE", 返回 true
- 给定 word = "ABCB", 返回 false
- [leetcode link](https://leetcode-cn.com/problems/word-search/)

### 解题思路
- 纯种的深度优先遍历算法DFS+回溯
- 思路与过程
    - 变量设计
	    - visited矩阵判断该位置是否访问过
		- dfs(int i, int j, int index)
		    - i,j为当前字符所在坐标
			- index为当前匹配到哪个字符了
	- DFS依次尝试上、下、左、右四个方向
	    - 该方向元素匹配下一个字符，且该元素没被访问过，才会被尝试

- 代码实现
    - 查找过程
    ```java
	char [][] board;
    boolean[][] visited;
    String word;
    ArrayList<List<Integer>> direct;
    public boolean exist(char[][] board, String word) {
        this.board = board;
        this.visited = new boolean [board.length][board[0].length];
        this.word = word;
        direct = new ArrayList();
        direct.add(new ArrayList(new ))
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == word.charAt(0)){
                    boolean result = dfs(i,j, 1);
                    if(result){
                        return true;
                    }
                }
            }
        }
        return false;
    }
    ```
	- DFS过程
    ```java
	private boolean dfs(int i, int j,int index){
        if(index == word.length()){
            return true;
        }
        visited[i][j] = true;
        boolean res = false;
        if(i-1 >= 0 && board[i-1][j] == word.charAt(index) && visited[i-1][j] == false){
            res = dfs(i-1,j,index+1);
            visited[i-1][j] = false;
            if(res) return true;
        }
        if(i+1 < board.length && board[i+1][j] == word.charAt(index)&& visited[i+1][j] == false){
            res = dfs(i+1,j,index+1);
            visited[i+1][j] = false;
            if(res) return true;
        }
        if(j+1 < board[0].length && board[i][j+1] == word.charAt(index)&& visited[i][j+1] == false){
            res = dfs(i,j+1,index+1);
            visited[i][j+1] = false;
            if(res) return true;
        }
        if(j-1 >= 0 && board[i][j-1] == word.charAt(index)&& visited[i][j-1] == false){
            res = dfs(i,j-1,index+1);
            visited[i][j-1] = false;
            if(res) return true;
        }  
        visited[i][j] = false;
        return false;
    }
    ```
    - 四个方向，也可以通过一个direction数组来表示，这样四个分支可以通过一个循环搞定

- 复杂度分析
    - 时间复杂度O(N*M*K)
	- 空间复杂度O(N*M*4*K)

## 33.柱状图中最大的矩形
### 题目
>- 给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
- 求在该柱状图中，能够勾勒出来的矩形的最大面积。
- 输入: [2,1,5,6,2,3]
- 输出: 10
- [Leetcode LinK](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)


### 解题思路
- 想到接雨水、剑指Offer中的问题
    - 接雨水中，由最高的墙分割区间
	- 本题中，面积最大，由最矮的墙分割区间

#### 思路1：迭代按列计算
- 原理：依次计算以各个列作为高度的最大面积
- 高度为该列的高度
- 宽度
    - 向左找到第一个比自己小的，其右边的 left
	- 向右找到第一个比自己大的，其左边的 right
	- 之间的距离便是宽度， right - left + 1
- 代码实现
```java
	public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        for (int i = 0; i < heights.length; i++) {
            int left = i, right = i;
            // 向左寻找第一个比自己小的
            for (int j = i-1; j >= 0; j--) {
                if(heights[j] < heights[i]){
                    left = j+1;
                    break;
                }
                if(j == 0){
                    left = 0;
                }
            }
            // 向右寻找第一个比自己小的
            for (int j = i+1; j < heights.length; j++) {
                if(heights[j] < heights[i]){
                    right = j - 1;
                    break;
                }
                if(j == heights.length-1){
                    right = heights.length-1;
                }
            }
            int width = right - left + 1;
            maxArea = Math.max(maxArea, width * heights[i]);
        }
        return maxArea;
    }
```

- 复杂度分析
    - 时间复杂度O(N^2)
	- 空间复杂度O(1)

#### 思路2：递归分治计算
- 每个列以他为高度的矩形的宽度是由区间中最矮的列决定的
- 与接雨水问题相反，用最矮的列不断对区间进行分割
- 思路与过程
1. 找到区间中的最矮的列，其将区间分成左右两个部分
2. 继续对左右区间再查找最矮列
3. 递归计算各个子区间中的最大面积
    - 最大面积为三者中的最大值
	    - 左区间的最大面积
		- 右区间的最大面积
		- 以当前区间最矮列为高，区间长度为宽度的面积

- 代码实现
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        return recv(heights, 0, heights.length-1);
    }

    private int recv(int [] heights,int left , int right){
        if(left > right) return 0;
        int minIndex = left, index = left;
        while(index <= right){
            if(heights[index] < heights[minIndex]){
                minIndex = index;
            }
            index++;
        }
        int width = right - left + 1;
        int height = heights[minIndex];
        int area = width * height;
        area = Math.max(area, recv(heights, left, minIndex-1));
        area = Math.max(area, recv(heights, minIndex+1, right));
        return area;
    }
}
```

- 复杂度分析
    - 时间复杂度O(N^2)
	- 空间复杂度O(N)

#### 思路3：单调栈
- 是解决该类矩形面积问题的较优解法
- 原理和思路1是一样的，也是按列计算
- 以空间换时间
    - 用单调栈来减少查找左右最小值的时间复杂度
- 思路与过程

0. 栈中存储的是列的数组下标
1. 当栈为空、当前列高度大于等于栈顶下标对应的列，
    - 则，将当前列坐标入栈
	- 目的是找到第一个递减的列，出现递减，则其前边的几个列便可以确定宽度了
	- 循环，直到不满足条件
2.  当出现当前列高度小于栈顶下标对应的列
    - 设当前列的索引为 rightIndex， 它是栈顶列右边第一个小于它的
	- poll出栈顶坐标 index，获取其高度 height[ index]，作为矩形的height
	- leftIndex 为栈顶下标，栈顶下标是 index列左边第一个小于它的
	- 宽度为 (rightIndex-1) - (leftIndex+1) + 1
	- 循环，直到栈顶元素高度大于 rightIndex 的高度
	    - 然后将rightIndex入栈，即，始终要保持栈中元素式单调非减的
	- 继续 从1循环
3.  当列从左向右都遍历一遍了，栈中应该还会剩下一些元素
    1. 取出栈顶元素下标作为rightIndex
	2. 循环，直到栈为空
	    1. poll出栈顶坐标index，获得矩形高度为height[index]
		2. leftIndex = 新的栈顶元素下标，如果栈为空了，则leftIndex为0
		3. 宽度为，rightIndex - leftIndex + 1
		4. 更新maxArea

- 代码实现
```java
	public int largestRectangleArea(int[] heights) {
        if(heights.length == 0) {
            return 0;
        }
        int maxArea = 0;
        // 栈中保存的是元素的数组下标
        Deque<Integer> stack= new ArrayDeque<>();
        stack.push(0);
        int index = 1;
        while (index < heights.length){
            if(heights[index] >= heights[stack.peek()]){
                stack.push(index);
                index++;
                continue;
            }
            // 遇到第一个比栈顶小的了
            while (!stack.isEmpty() && heights[index] < heights[stack.peek()]){
                int height = heights[stack.poll()];
                int leftIndex = 0;
                if(!stack.isEmpty()){
                    leftIndex = stack.peek() + 1;
                }
                int width = index - leftIndex;
                maxArea = Math.max(maxArea, width* height);
            }
            stack.push(index);
            index++;
        }
        if(!stack.isEmpty()){
            int rightIndex = stack.peek();
            while (!stack.isEmpty()){
                int height = heights[stack.poll()];
                int leftIndex = 0;
                if(!stack.isEmpty()){
                    leftIndex = stack.peek() + 1;                    
                }
                int width = rightIndex - leftIndex + 1;
                maxArea = Math.max(maxArea, width * height);
            }
        }
        return maxArea;
    }
```
- 复杂度分析
    - 时间复杂度 O(N)
	- 空间复杂度O(N)