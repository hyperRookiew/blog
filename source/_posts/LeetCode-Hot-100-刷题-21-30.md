---
title: 'LeetCode Hot 100 刷题 [21-30]'
categories:
  - 算法
  - LeetCode
tags:
  - LeetCode
  - 算法
date: 2022-07-28 09:47:00
---


# LeetCode Hot 100 [11-20]

>刷完了《剑指Offer》，下一阶段，开始 LeetCode Hot 100。 之前一直都用思维导图记录，今天发现思维导图可以导出成markdown，简单编辑一下就可以成为文章。相比Xmind，博客文章打开和查看更加方便，所以之后每次Xmind记录之后，会再在这里同时同步一下

## 前言
HOT100，之前的文章传送门
[LeetCode Hot 100 刷题 [1-10]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot100-shua-ti-1-10.html)
[LeetCode Hot 100 刷题 [11-20]](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot-100-shua-ti-11-20.html)

## 21.接雨水问题
### 题目
>- 给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
- 输入: [0,1,0,2,1,0,1,3,2,1,2,1]
- 输出: 6
- [题目地址](https://leetcode-cn.com/problems/trapping-rain-water/)
- 这是一道Hard题	

### 解题思路
#### 思路1：暴力算法
- 按列，统计每个列上可以存的雨水的量
- 如何计算某个列上存水的量
    1. 遍历找出该列左边的最大高度，maxleft
	2. 遍历找出该列右边的最大高度，maxRight
	3. 水的高度取决于maxLeft和maxRight中的较小者
	4. 这个列上水的深度为水的高度 - 该列的高度
- 代码实现
```java
		public int trap(int[] height) {
        int res = 0;
        for(int i =0; i < height.length; i++){
            // find max left
            int leftMax = 0;
            for(int j = 0; j < i; j++){
                leftMax = Math.max(leftMax, height[j]);
            }
            // find max right
            int rightMax = 0;
            for(int j = i+1; j<height.length; j++){
                rightMax = Math.max(rightMax, height[j]);
            }
            int validHeight = Math.min(leftMax,rightMax);
            res += Math.max(0, validHeight-height[i]);
        }
        return res;
    }
```
- 算法复杂度
    - 时间复杂度O(N^2)

#### 思路2：动态规划
- 对思路1进行优化，思路1中查找左右最大值的过程可以通过DP优化
- 定义maxLeftDP[ i ] 为 i 左边的最大高度，同理，maxRightDp[ i ]为 i 右边的最大高度 
- maxLeftDP[ i ] = Max{ maxLeftDP[ i-1 ] , height[ i-1 ] }
- 代码实现
```java
	public int trap(int[] height) {
        int res = 0;
        int [] maxLeftDP = new int [height.length];
        int [] maxRightDP = new int [height.length];
        int size = height.length;
        for(int i = 1; i < size-1; i++){
            maxLeftDP[i] = Math.max(maxLeftDP[i-1], height[i-1]);
            maxRightDP[size-1- i] = Math.max(maxRightDP[ size-i], height[size-i]);
        }
        for(int i = 1; i < height.length-1; i++){
            int validHeight = Math.min(maxLeftDP[i],maxRightDP[i]);
            res += Math.max(0, validHeight-height[i]);
        }
        return res;
    }
```
- 复杂度分析
    - 时间复杂度O(N)
	- 空间复杂度O(N)

#### 思路3：双指针解法
- 对思路2中的空间复杂度还可以进一步优化
- DP递推方程中，maxLeft和maxRight其实只用到左右边的一个，其实可以不用数组来存储
- maxLeftDP是直接可以用变量来替代的，但是maxRightDP的求解是从右向左的，不方便直接用变量来替代
- 思路与过程
    - left、right双指针从两端进行更新
	- maxLeft、maxRight记录left和right位置对应的左右最大值
	- maxLeft和maxRight中，起作用的是他们中的较小值，所以每次left和right指针更新较小值的一端
- 如何选择更新哪一段
    - maxLeft < maxRight
	    - 可以保证maxLeft为left左边最大值，且它是起作用的
	    - left指针前进，同时更新maxleft
	maxLeft > maxRight
	    - 同理，可以保障maxRight为right右边最大值，且它比maxLeft要小，所以即便加上右边还没检查的部分，最终的maxLeft还是比maxRight大，即起作用的还是maxRight
		- 所以，right指针前进，同时更新maxRight
	- 让我想起了类似的用双指针求最大面积的题目
	    - 每次搜索高度较小的一边
- 代码实现
```java
	public int trap(int[] height) {
        int res = 0;
        int left = 0, right = height.length - 1;
        int maxLeft = 0, maxRight = 0;
        while( left <= right){
            // maxLeft 起作用
            if( height[left] < maxRight && maxLeft < maxRight ){
                if( height[left] > maxLeft ){
                    maxLeft = height[left];
                }else{
                    res += maxLeft - height[left];
                } 
                left++;
            } else { // maxRight 起作用
                if( height[right] > maxRight ){
                    maxRight = height[right];
                }else{
                    res += maxRight - height[right];
                } 
                right--;
            }
        }
        return res;
    }
```
- 复杂度分析
	- 时间复杂度O(N)
	- 空间复杂度O(1)

## 22.旋转图像
### 题目
>- 给定一个 n × n 的二维矩阵表示一个图像。
- 将图像顺时针旋转 90 度。
- 你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。
- 给定 matrix = 
```java
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
]
```
- 原地旋转输入矩阵，使其变为:
```java
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```
- [题目地址](https://leetcode-cn.com/problems/rotate-image)

### 解题思路
#### 思路1：转置+行交换
- 思路与过程
	1. 将行按照对称的方式进行交换
        - a[i][j] 与 a[ len-i-1 ][j]交换
	2. 将处理后的矩阵按对角进行转置
	    - a[i][j] 与 a[j][i]交换
- 代码实现
```java
	public void rotate(int[][] matrix) {
        // 水平交换
        int left = 0, right = matrix.length-1;

        while( left < right){
            for(int i = 0; i < matrix[0].length;i++){
                rowSwap(matrix, left, right, i);
            }
            left++;
            right--;
        }
        // 对角线对称
        for(int i = 0; i<matrix.length; i++){
            for(int j = 0; j< i; j++){
                angleSwap(matrix,i,j);
            }
        }
    }

    private void rowSwap(int [][] matrix, int left, int right , int i){
        int temp = matrix[left][i];
        matrix[left][i] = matrix[right][i];
        matrix[right][i] = temp;
    }

    private void angleSwap(int [][] matrix, int i, int j){
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
```
- 复杂度评估
    - 时间复杂度O(N^2)

## 23.字母异位词分组  
### 题目
>- 给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
- 输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
- 输出:
```java
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```		
- 所有输入均为小写字母。
- 不考虑答案输出的顺序。
- [题目地址](https://leetcode-cn.com/problems/group-anagrams/)

### 解题思路
#### 思路1：排序 + HashMap
- 对每一个字符串的字符按字典序进行排序
- 将排序后的结果作为HashMap的key，value为列表
- 最终将HashMap的values输出为列表
    - values()，返回的是数组，不是List，需要将数组转化成list
	- Java8 Stream API:`hashMap.values().stream().collect(Collectors.toList())`
	- Java8之前：`new ArrayList(hashMap.values())`
- 代码实现
```java
	public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> hashMap = new HashMap();
        for(String str : strs){
            char [] values = str.toCharArray();
            Arrays.sort(values);
            String key = new String(values);
            if(hashMap.containsKey(key)){
                hashMap.get(key).add(str);
            }else{
                List<String> list = new ArrayList();
                list.add(str);
                hashMap.put(key,list);
            }
        }
        return hashMap.values().stream().collect(Collectors.toList());
    }
```
- 复杂度分析
    - 时间复杂度O(Nklogk)
    - 空间复杂度O(NK)

#### 思路2：字符计数 + HashMap
- 与思路1基本类似，只是key的生成方式不一样
- 思路1需要排序，O(klogk)，还是比较高的
- 可以统计每个字符串中26个字母出现的次数，作为模式，模式作为hashmap的key
    - 计数模式：`int [ ] count = new int [26]`
    - 最后的模式串：`1#0#6#...`
- 代码实现
```java
	public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> hashMap = new HashMap();
        int [] count = new int [26];
        for(String str : strs){
            char [] values = str.toCharArray();
            Arrays.fill(count, 0);
            for(char ch : values){
                count[ch-'a']++;
            }
            StringBuilder builder = new StringBuilder();
            for(int num:count){
                builder.append(num);
                builder.append(',');
            }
            String key = builder.toString();
            if(hashMap.containsKey(key)){
                hashMap.get(key).add(str);
            }else{
                List<String> list = new ArrayList();
                list.add(str);
                hashMap.put(key,list);
            }
        }
        return hashMap.values().stream().collect(Collectors.toList());
    }
```
- 复杂度分析
    - 时间复杂度O(NK)
	- 空间复杂度O(NK)

## 24.跳跃游戏  
### 题目
>- 给定一个非负整数数组，你最初位于数组的第一个位置。
- 数组中的每个元素代表你在该位置可以跳跃的最大长度。
- 判断你是否能够到达最后一个位置。
- 输入: [2,3,1,1,4]
- 输出: true
- 解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
- [题目地址](https://leetcode-cn.com/problems/jump-game/)

### 解题思路
#### 思路1：排除法
- 研究什么时候跳不到终点
    1. 序列中有 0 的时候，才会可能跳不到终点
	2. 当 0 前边所有的数都无法跳过 0 ，则必定跳不过去
	    - 即该位置的值，小于等于该位置距离0的格数
	3. 最后一个为0不影响
- 思路过程
1. 遍历数组，检索所有的0
    - 最后一个位置是否为0不影响，所以不用判断
2. 对每一个0， 向前遍历，是否存在能跳跃过它的数
3. 所有的都满足，则可以到达最后一个位置
- 代码实现
```java
	public boolean canJump(int[] nums) {        
        for(int i=0; i < nums.length-1; i++){
         // 最后一个是0还是非0对结果不影响
            if(nums[i] == 0){
                int j,dis=1;
                for(j = i-1; j >= 0; j--,dis++){
                    if(nums[j] > dis) break;
                }
                if(j < 0) return false;
            }
        }
        return true;
    }
```
- 复杂度分析
    - 时间复杂度O(N^2)
	- 空间复杂度O(1)

#### 思路2：贪心算法
- 是本题的最优解法，也应该是主要的考察点
- 比较不容易想到贪心规则
- 某一位置可达，则该位置之前的位置均是可达的
- 所以，只要保障能到达的位置超过数组最后一个位置就可以了
    - 因此，每次都尽可能的向远处跳
- 思路过程
1. mostFarIndex 标识目前能到达的最远位置
2. 当前位置在最远位置或之前，则说明最远位置可能还可以更大，更新最远位置
3. 若当前位置在最远位置之后了，则最远位置再也没法更新了，也就最终无法超越最后一个位置了

- 代码实现
```java
	public boolean canJump(int[] nums) {        
        int mostFarIndex = 0;
        for(int i=0; i < nums.length-1; i++){ // 最后一个不影响
            if( i <= mostFarIndex ){
                mostFarIndex = Math.max(mostFarIndex, i + nums[i]);
                if(mostFarIndex >= nums.length-1) return true;
            } else {
                return false;
            }
        }
        return mostFarIndex >= nums.length-1;
    }
```

- 复杂度分析
    - 时间复杂度O(N)
	- 空间复杂度O(1)


## 25.合并区间
### 题目
>- 出一个区间的集合，请合并所有重叠的区间。
- 示例
- 输入: [[1,3],[2,6],[8,10],[15,18]]
- 输出: [[1,6],[8,10],[15,18]]
- 解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
- [题目链接](https://leetcode-cn.com/problems/merge-intervals)

### 解题思路
- 先将区间按照左边界的大小排序
    - lambda实现
```java
    Arrays.sort(intervals, (int [] a, int [] b)->{
            return a[0] - b[0];
    });
```
- 然后遍历区间数组，相邻的区间能合并则合并
- 代码实现
```java
	public int[][] merge(int[][] intervals) {
        if(intervals.length == 0) return new int[0][0];
        ArrayList<int []> res = new ArrayList();
        // 按第一个排序
        Arrays.sort(intervals, (int [] a, int [] b)->{
            return a[0] - b[0];
        });
        int start = intervals[0][0], end = intervals[0][1];
        for(int i = 0; i<intervals.length; i++){
            if(intervals[i][0] <= end){
                end = Math.max(end, intervals[i][1]);
            }else{
                res.add(new int[]{start, end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }
        res.add(new int[]{start, end});
        return res.toArray(new int[res.size()][2]);
    }
```
- 复杂度分析
    - O(NlogN)

## 26.不同路径
### 题目
>- 一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
- 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
- 问总共有多少条不同的路径？
- [题目链接](https://leetcode-cn.com/problems/unique-paths)

### 解题思路
- 典型的深度优先遍历，动态规划解题

#### 思路1：DFS+回溯
- 从左上角出发，分别尝试向下和向右，然后回溯
- 代码实现
```java
class Solution {
    private int count = 0;
    public int uniquePaths(int m, int n) {
        dfs(0,0,m,n);
        return count;
    }

    private void dfs(int i, int j, int m, int n){
        if(i >= m || j >= n) return;
        if( i == m-1 && j == n-1){
            count++;
            return ;
        }
        // 向右尝试
        if(j < n-1){
            dfs(i,j+1,m,n);
        }
        // 向左尝试
        if(i < m -1){
            dfs(i+1, j, m,n);
        }
    }
}
```
- 复杂度分析
    - 时间复杂度O((M+N)^2)
	- 空间复杂度O(（M+N）^2)

#### 思路2：动态规划
- dp[ i ][ j ]：到达坐标(i,j)位置有多少个路径
	- = dp[ i-1 ]dp[ j ] + dp[ i ][ j-1 ]
- 思路过程
    1. 创建二维dp数组，并进行初始化
	2. dp数组的第一行和第一类初始化为1，因为可以确定只有1个路径
	3. 对dp数组进行填表操作
- 代码实现
```java
    public int uniquePaths(int m, int n) {
        int [][] dp = new int [m][n];
        Arrays.fill(dp[0], 1);
        for(int i = 0; i < m; i++){
            dp[i][0] = 1;
        }
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
```
- 复杂度分析
    - 时间复杂度O(M*N)
	- 空间复杂度O(M*N)

#### 思路3：动态规划空间优化
- 思路2中的dp二维数组，可以继续优化成一维数组
- 因为递归关系中，只使用到了坐标左边和左边上边的位置
- 代码实现
```java    
    public int uniquePaths(int m, int n) {
        int [] dp = new int [n];
        Arrays.fill(dp, 1);
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[j] = dp[j-1] + dp[j];
            }
        }
        return dp[n-1];
    }
```

## 27. 最小路径和
### 题目
>- 给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
- 说明：每次只能向下或者向右移动一步。
- 输入:
```java
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
```
- 输出: 7
- 解释: 因为路径 1→3→1→1→1 的总和最小。
- [link](https://leetcode-cn.com/problems/minimum-path-sum)

### 解题思路
- 思路明显，二维数组动态规划
- dp[ i ][ j ]：从起点到达 (i , j)位置的路径和
    - dp[ i ][ j ] = Math.min(dp[ i ][j-1], dp[ i-1 ][ j ]) + grid[i][j];
	- 只用到了左边和上边的位置，所以可以优化到一维数组
	- 初始状态，i=0 以及 j = 0，第0行和第0列的初始化
- 代码实现
```java
	public int minPathSum(int[][] grid) {
        if(grid.length == 0) return 0;
        int dp[] = new int [grid[0].length];
        dp[0] = grid[0][0];
        for(int i = 1 ; i < dp.length; i++){
            dp[i] = dp[i-1] + grid[0][i];
        }
        for(int i = 1; i<grid.length; i++){
            dp[0] += grid[i][0]; 
            for(int j = 1; j<dp.length; j++){
                dp[j] = Math.min(dp[j-1], dp[j]) + grid[i][j];
            }
        }
        return dp[dp.length-1];
    }
```
- 复杂度分析
    - 时间复杂度O(N*M)
	- 空间复杂度O(M)

## 28. 编辑距离
### 题目
>- 给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。
- 你可以对一个单词进行如下三种操作：
    - 插入一个字符
	- 删除一个字符
	- 替换一个字符
- 输入：word1 = "horse", word2 = "ros"
- 输出：3
- 解释:
```java
    horse -> rorse (将 'h' 替换为 'r')
    rorse -> rose (删除 'r')
    rose -> ros (删除 'e')
```
- [link](https://leetcode-cn.com/problems/edit-distance)

### 解题思路
- 动态规划解题

1. 求最优解问题
2. 是可以分阶段进行的
    - 每个阶段有插入、删除、替换三个选择
	- 插入、删除、替换设定都在末尾进行
	    - 等价的，在中间插入也是可以转换到尾部操作的
3. 最优子结构
    - dp[ i ][ j ]，表示将word1的前 i 位 转换成 word2的前 j 位需要的最少操作数
	- 它可以通过三种方式获得，且取三种方式中的最小值
	1. word1末尾插入
	    - dp[ i ][ j ] = dp[ i-1 ][ j ] + 1;
		- hors -> ros 需要a步，则horse -> ros 需要 a+1 步
	2. word1末尾删除
	    - dp[ i ][ j ] = dp[ i ][ j-1 ] + 1;
		- hors -> ro需要 b 步，则 hors -> ros 需要 b+1 步
	3. word1末尾替换
		- dp[ i ][ j ] = dp[ i-1 ][ j-1 ] + 1 ，如果 i 和 j 字符不同， 则需要额外一次替换操作
		    - hors - > ro 为 c， 则horo -> ro需要额外一次替换操作
		- dp[ i ][ j ] = dp[ i-1 ][ j ] , 如果 i 和 j 字符相同，则不需要额外一次操作 
			- hor -> ro 为 d ，则 hors -> ros 为 d
- 代码实现
```java
	public int minDistance(String word1, String word2) {
        int len1 = word1.length(), len2 = word2.length();
        if(len1 == 0) return len2;
        if(len2 == 0) return len1;
        int dp[][] = new int [len1 +1 ][len2+1];
        for(int i = 0; i <= len1; i++ ){
            dp[i][0] = i;
        }
        for(int i = 0; i<= len2; i++){
            dp[0][i] = i;
        }
        for(int i = 1; i <= len1; i++){
            for(int j = 1; j <= len2; j++){
                int insert = dp[i-1][j] + 1;
                int remove = dp[i][j-1] + 1;
                int replace = 0;
                if(word1.charAt(i-1) == word2.charAt(j-1)){
                    replace = dp[i-1][j-1];
                }else{
                    replace = dp[i-1][j-1] + 1;
                }
                dp[i][j] = Stream.of(insert,remove, replace)
                .min(Comparator.comparing(Integer::valueOf)).get();
            }
        }
        return dp[len1][len2];
    }
```
- 复杂度分析
    - 时间复杂度O(M*N)
	- 空间复杂度O(M*N)

## 29. 颜色排序
### 题目
>- 给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
- 此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。
- 不能使用代码库中的排序函数来解决这道题。
- 输入: [2,0,2,1,1,0]
- 输出: [0,0,1,1,2,2]
- 进阶
    - 一个直观的解决方案是使用计数排序的两趟扫描算法。
	- 首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
	- 你能想出一个仅使用常数空间的一趟扫描算法吗？
- [题目地址](https://leetcode-cn.com/problems/sort-colors)	

### 解题思路
#### 思路1：计数排序算法
- 计数排序
    1. 数组统计各种元素的出现频数
	2.  对频数数组进行连续累加，对应的数字便是该类值插入位置的后一个
	3. 读取原数组中的数字，并查找频数数组，确定插入的位置
	    - 插入成功之后，频数数组对应的值-1
		- 即，插入位置前移一个
- 这里由于只要对值排序就可以，相同的值之间没有差异，所以得到频数数组之后，直接更改原数组即可
- 代码实现
```java
	public void sortColors(int[] nums) {
        int[] count = new int[3];
        for (int i = 0; i < nums.length; i++) {
            count[nums[i]]++;
        }

        int index = 0;
        for (int i = 0; i < count.length; i++) {
            for (int j = 0; j < count[i]; j++) {
                nums[index] = i;
                index++;
            }
        }
    }
```
- 复杂度分析
    - 时间复杂度O(K+N)
	- 空间复杂度O(K)

#### 思路2：双指针算法
- 类似三路排序的效果，能够高效处理含有大量重复元素的情况
- 一路指针标记0的插入位置，另一路标记2的插入位置，0和2排序好了，中间剩下的1便也排序好了
- 代码实现
```java
	public void sortColors(int[] nums) {
        int left = 0, right = nums.length - 1;
        int index = left;
        while (index <= right) {
            if (nums[index] == 0) {
                swap(nums, index, left);
                left++;
                index++;
            } else if (nums[index] == 2) {
                swap(nums, index, right);
                right--;
            } else {
                index++;
            }
        }
    }
```
- 复杂度分析
	- 时间复杂度O(N)
	- 空间复杂度O(1)

## 30.最小包含子串
### 题目
>- 给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。
- 输入: S = "ADOBECODEBANC", T = "ABC"
- 输出: "BANC"
- 如果 S 中不存这样的子串，则返回空字符串 ""。
- 如果 S 中存在这样的子串，我们保证它是唯一的答案。
- [link](https://leetcode-cn.com/problems/minimum-window-substring)

### 解题思路
- 滑动窗口解题的典型例题
- T字符中可能存在重复字符
    - 对字符进行计数
	- 可以使用数组统计128所有字符
	- 也可以通过HashMap，只统计出现的字符
    ```java
	    for (char c : tValues) {
            if (tMap.containsKey(c)) {
                tMap.put(c, tMap.get(c) + 1);
            } else {
                tMap.put(c, 1);
            }
        }
    ```
- 检测当前窗口是否包含T所有字符
    - distance，即窗口内容与T之间的距离
	- 当字符出现在T中，且该字符能缩小窗口与T距离的时候，distance++
	```java
    		// 字符在t中，更改distance
            if (winMap.containsKey(sValues[right])) {
                if (winMap.get(sValues[right]) < tMap.get(sValues[right])) {
                    distance++;
                }
                winMap.put(sValues[right], winMap.get(sValues[right]) + 1);
            } else {
                winMap.put(sValues[right], 1);
                distance++;
            }
        ```
- 思路与过程
1. 通过HashMap，统计T字符串中字符的出现次数
2. left，right=0，窗口从左出发
3. 先right右移，right位置元素未在TMap中，则继续右移
4.  当right位置元素出现在TMap中
    - 如果SMap中该字符数目 < TMap中该字符数目
	    - distance++
		- 说明该字符能有效降低窗口和T之间的距离
		- 若大于等于，说明该元素重复了
	- 然后SMap中该字符数目+1
5. 检测distance
    - 如果比T的长度小，则right继续右移
	- 如果和T长度相等，说明当前窗口是满足包含所有T字符的子串，但需要缩小到最小子串
6. left指针右移
    - 移动到distance不等于T长度为止
	- 判断left位置字符是否出现在TMap中
	    - 出现，若SMap中该字符频数 <= TMap中该字符频数，distance--
	- SMap中该字符频数-1
7. 最终left的位置便是子串最左位置的下一个
    - length = right -left +2
	- 更新最短长度以及其实位置

- 代码实现
```java
	public String minWindow(String s, String t) {
        char[] sValues = s.toCharArray();
        char[] tValues = t.toCharArray();
        int left = 0, right = 0;
        HashMap<Character, Integer> tMap = new HashMap<>();
        HashMap<Character, Integer> winMap = new HashMap<>();
        int distance = 0;
        int minBegin = 0, minLength = Integer.MAX_VALUE;

        for (char c : tValues) {
            if (tMap.containsKey(c)) {
                tMap.put(c, tMap.get(c) + 1);
            } else {
                tMap.put(c, 1);
            }
        }

        while (right < sValues.length) {
            // right 右移
            if (!tMap.containsKey(sValues[right])) {
                // 字符不在t中，right右移
                right++;
                continue;
            }
            // 字符在t中，更改distance
            if (winMap.containsKey(sValues[right])) {
                if (winMap.get(sValues[right]) < tMap.get(sValues[right])) {
                    distance++;
                }
                winMap.put(sValues[right], winMap.get(sValues[right]) + 1);
            } else {
                winMap.put(sValues[right], 1);
                distance++;
            }
            if (distance < t.length()) {
                right++;
                continue;
            }
            // 判断distance,等于t串长度的时候，left开始左移
            while (distance == t.length()) {
                if (winMap.containsKey(sValues[left])) {
                    if (winMap.get(sValues[left]) <= tMap.get(sValues[left])) {
                        distance--;
                    }
                    winMap.put(sValues[left], winMap.get(sValues[left]) - 1);
                }
                left++;
            }
            // 最终，left为满足条件的左边界的下一个
            if (right - left + 2 < minLength) {
                minBegin = left - 1;
                minLength = right - left + 2;
            }
            right++;
        }
        if (minLength == Integer.MAX_VALUE) {
            return "";
        }
        return s.substring(minBegin, minBegin + minLength);
    }
```
- 复杂度分析
    - 时间复杂度O(N+M)
	- 空间复杂度O(N+M)

## 下个路口见
HOT100依然在继续，写在下一篇文章中。[传送门](http://blog.zwboy.cn/suan-fa/leetcode/leetcode-hot-100-shua-ti-31-40.html)
