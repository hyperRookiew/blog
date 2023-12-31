---
title: 剑指offer刷题全过程
categories:
  - 算法
  - 题解
tags:
  - 算法
date: 2022-07-18 21:58:36
---

> 背景知识
# 剑指Offer刷题全过程

## 1. 基础算法

### 二分查找算法

#### 基础算法

- 输入
	- 必须是排序好的数组
	- 二分区间起点start
	- 二分区间终点end
			- 闭区间
			- 包括end的值
			- arrays.length - 1

		- 查找值

	- 返回值

		- 返回查找元素所在的下标 int
		- 不存在，则返回下标 -1

	- 实现过程

		- 递归实现

			- 1. 重要，判断边界：if(start > end) return -1;
			- 2. 计算middle = （start + end）/2；
			- 3. 比较middle的值

				- 相等，则return middle
				- 比target大，则查找左侧区间

					- 起点：start
					- 终点：middle -1

				- 比target小，则查找右侧区间

					- 起点：middle + 1
					- 终点：end

		- 循环实现

			- 1. 初始化 start 和 end 循环遍历
			- 2. while循环

				- 边界判断条件

					- start <= end
					- 和递归中的一样

				- 计算middle = （start + end）/2
				- middle与target相等，return middle
				- middle 大于 target，更新右边界

					- end = middle -1

				- middle 小于 target， 更新左边界

					- start = middle + 1

- 变种算法

	- 1. 将二分查找中的相等判断变为其他判断

		- 比如 题3 中拓展题的解法2

	- 2. 用于划分有序数组的边界

		- 如果数组存在target，直接返回坐标
		- 如果数组不存在target，则返回比它小的第一个值

### 子主题 2

## 2. 排序算法

### 快速排序

- 关于
- 算法思想

	- 分治

		- 基准值：pivot

			- 选出一个基准值（一般为第一个元素）
			- 依据基准值的大小，将分段分两个部分

				- 左边比自己小
				- 右边比自己大

		- 分区：partition

			- 一个分区运行过分区算法后，将被基准值分成两个子分区

	- 递归

		- 再各个分区上，再此选出一个基准值，再此分段
		- 每次执行分区算法，都能确定基准值的中间位置
		- 递归执行

- 算法步骤

	- 1. 实现分区算法

		- 1. 参数：分区start，分区end
		- 2. 选取 start 作为基准值pivot
		- 3. 初始middle为start+1

			- 表示下一个用于交换的位置
			- 最终基准值所在的位置为 middle-1

		- 4. 依次将区间中的每个元素和基准值比较
		- 5. 重点是分区内值的交换

			- 不是直接拿 基准值 和 被比较值 交换，这样不好处理
			- 而是在基准值之后的区间中，将小于基准值的向前交换，将大于基准值的向后交换
			- 最终遍历完，分区分为两部分了，再将基准值交换过去
			- 引入一个index，记录大于基准值的数和小于基准值的数之间的界限，每次拿这个界限交换

		- 6. 最终将基准值和中间位置交换，中间位置为middle - 1
		- 7. 返回基准值所在的中间坐标
		- int partition(nums, start, end) {
      int pivot = start;
      int middle = start + 1;
      for( int i = middle; i < end; i++) { 
              if( num[i] < num[pivot] ) {
                       swap( num, i, middle );
                       middle++;
              }
      }
      swap( num, pivot, middle-1 );
      return middle - 1;
}

	- 2. 实现递归总方法

		- 1. 先执行分区算法，返回分区后的中间下标

			- 即上一次分区结束基准值所在的位置
			- 分成两个分区

				- start到middle

					- 不包括middle

				- middle

					- 已经确定位置了，不用再参与分区了

				- middle+1到end

					- 不包括end

		- 2. 中间下标将分区分成两个子分区，继续在两个子分区上递归执行快排

			- 左边分区快排
			- 右边分区快排

		- void quickSort(num, start, end){
        if( start < end ) {
                int middle = partition(num, start, end);
                quickSort(num, start, middle);
                quickSort(num, middle+1, end);
        }
}

- 评估

	- 时间复杂度：Ο(nlogn) 

		- 最坏运行情况是 O(n²)
		- 最坏：顺序数列的快排

	- 相比其他排序算法，比较好
	- 处理大数据最快的排序算法之一

### 冒泡排序

### 子主题 3

## 3. 数据重复数字

### 题目

- 一个长度为 n 的数组 nums 

	- 所有数字都在 0～n-1 的范围内
	- 数组中存在数值是重复的
	- 不知道有几个重复，也不知道会重复几次

- 找出任意一个重复的数值

### 解题方法

- 1. 暴力两个循环(实在不会)

	- 时间复杂度O(n2)
	- 空间复杂度O(1)
	- 不推荐

- 2. 先排序+查找

	- 思路

		- 1. 先排序 O(nlogn)

			- 快排算法

		- 2. 再查找 O(n)

			- 循环遍历

	- 评估

		- 时间复杂度 O(nlogn) + O(n)
		- 空间复杂度 O(1)
		- 空间复杂度优先

- 3. 哈希表/哈希集（冲突检查）

	- 思路

		- 扫描数组 O(n)

			- 边扫描，边判断是否哈希表中是否已经存在
			- 存在，则查找成功了
			- 不存在，则将该值存入哈希表，继续下一个

		- 判断哈希表中是否存在 O(1)

	- 评估

		- 时间复杂度 O(n)
		- 空间复杂度 O(n)
		- 以空间换时间，时间复杂度优先

- 4. 鸽巢原理（最优）

	- 充分利用数值在 1到n-1之间的条件，数组下标与值对应关系
	- 类似哈希表，假设数组无重复，通过冲突来检测重复
	- 思路

		- 0. 假设数组无重复元素，则每个元素的值与他的下表应该是相等的
		- 1. 遍历数组每个元素，下标为i, 值为m
		- 2. 比较 m 是否和 i 相等

			- 相等，说明当前数值在正确的位置上，检查下一个下标

		- 3. m 和 i 不相等，说明不在正确的位置上

			- 检查 下标 m 位置的值是否已经是 m 了
			- 如果是 m ，说明m值重复了，查找结束
			- 如果不是m，则将m值交换过去，另一个值交换回来

		- 3. 继续遍历

	- 评估

		- 时间复杂度 O(n)
		- 空间复杂度 O(1)
		- 最优的算法了吧

- 5. 基于桶计数

	- 能支持更多的重复次数
	- 对各个数值出现的次数做统计
	- 当出现次数大于等于2，则结束
	- 类似哈希表解法的方式

### 拓展：不允许更改数组

- 1. 数组复制查找

	- 思路

		- 1. 新建一个n长度的数组
		- 2. 将原数组中值为m的元素复制到辅助数组下标为m的位置
		- 3. 如果m位置已经有元素了，则查找重复成功

	- 评估

		- 时间复杂度： O(n)
		- 空间复杂度：O(n)

- 2. 二分查找变种

	- 思路

		- 将数值范围分为 [ 0, n\2 ]  和 [ n\2+1 ，n-1]
		- 如果 1 到 n\2 出现的次数大于 n\2，那么 1 到 n\2 中一定存在一个重复
		- 是二分查找的变种，只是将查找的判断变为数数值出现的次数是否大于区间长度

	- 评估

		- 时间复杂度：O(nlogn)
		- 空间复杂度：O(1)

## 4. 二维数组中的查找

### 题目

- 一个n*m的数组

	- 每一行都按照从左到右递增的顺序排序
	- 每一列都按照从上到下递增的顺序排序

- 输入一个元素value，查找该元素是否在该数组中
- 链接

### 接替思路

- 1. 从右上角二分查找

	- 思路过程

		- 注意边界条件判断

			- 空数组
			- 维度为0

		- 1. 先在最右侧的列上进行二分查找，查找目标值，或则确定边界
		- 2. 排除上方较小的行，在边界行上再做二分查找
		- 3. 查找到目标支或者边界值，排除掉部分右侧的列
		- 4. 再继续在新的列上进行二分查找

	- 评估

		- 时间复杂度 O(nlogn + mlogm)
		- 是自己考虑到和实现的算法
		- 原本以为使用二分查找比较高效，其实时间复杂度更大了
		- 因为按行列挨个排除只需要在行列上进行O(n)，而二分查找在每次缩小区间之后，还要再此二分查找，整体O(nlogn + mlogm)

- 1. 从右上角依次行列查找

	- 思路

		- 1.  也是从右上角开始
		- 2. 判断右上角的值是否等于查找值
		- 3. 比查找值大，则排除该列，列--
		- 4. 比查找值小，则排除该行，行++
		- 5. 直到最终查找成功，或者数组下标突破边界条件

	- 评估

		- 时间复杂度 O(n+m)
		- 剑指Offer上推荐的算法
		- 比较简单高效

## 5. 替换字符串空格字符

### 题目

- URL编码函数
- 将字符串数组中的每个空格字符替换成%20
- 比如 “Happy Birthday” 替换为 “Happy%20Birthday”
- 注意，需要在已有的字符数组上进行操作，不能拷贝新的字符串数组

### 关于字符串存储

- Java中，字符串是用常量表示的，为了复用
- 所以，无法更改字符串，只能用新的字符串替代已有的字符串
- StringBuilder，用于组装字符串，每次向字符串追加字符，不会造成新的字符串常量的生成

### 解题思路

- 暴力，不可取

	- 遍历该字符串中的所有字符，当检测到空格，则
	- 将其后的所有字符依次右移3个位置
	- 评估

		- 时间复杂度 O(n2)

- 从后向前替换

	- 过程

		- 1. 遍历字符串，计数空格的个数
		- 2. 基于空格个数，计算最终字符串的总长度，即字符串结尾
		- 3. 从后向前再此遍历字符串，并将其移动到最终的位置

	- 评估

		- 时间复杂度O(n)

## 7.重建二叉树

### 题目

- 输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

- 比如

	- 前序遍历 preorder = [3,9,20,15,7]
	- 中序遍历 inorder = [9,3,15,20,7]

- 结果

	-     3
   / \
  9  20
    /  \
   15   7

### 接替思路

- 1. 前序遍历中最先得到的是根，而根能够将中序遍历由根分为左右两个部分
- 2. preorder和inorder因此可以被继续划分成更小的遍历对
- 3. 形成一个递归问题
-     private TreeNode build(int[]preorder,int[] inorder,int start1,int end1,int start2,int end2){
        if(end2 < start2 || start1>end1 || start2>end2){
            return null;
        }else {
            TreeNode node = new TreeNode(preorder[start1]);
            int pos = findPos(inorder,preorder[start1]);
            node.left = build(preorder,inorder,start1+1,start1+pos-start2,start2,pos-1);
            node.right= build(preorder,inorder,start1+pos-start2+1,end1,pos+1,end2);
            return node;
        }
    }

## 8. 两个栈实现队列

### 题目

- 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

- 输入：

	- ["CQueue","appendTail","deleteHead","deleteHead"]
	- [[],[3],[],[]]

- 输出：[null,null,3,-1]

### 解题思路

- 1. 两个栈，左栈和右栈
- 2. 入队列一律都是加入到左栈中
- 3. 出队列

	- 如果都在左栈，右栈为空，则全部push pop到右栈，pop右栈栈顶
	- 如果右栈不空，则直接pop右栈即可，左栈继续作为入队列

## 9.斐波拉契数列计算

### 题目

- 给一个n，计算斐波拉契数列中n位置的数值

### 解题思路

- 递归实现

	- 十分简单，但是会超时
	- 因为存在很多的重复计算子问题

- 递归+备忘录

	- 在递归的基础上，记录以及计算过的f(n)
	- 将计算过的数据存储到数组中
	- 使用之前先到备忘录中查找
	- 备忘录可以使用一维数组或者Map实现

- 动态规划

	- 存在重复子问题
	- 无后效性
	- 递推方程已知道

## 10.青蛙跳台阶问题

### 其实是斐波拉契数列问题的变种

### 类似的，整钱破成零钱的组合问题

## 11. 旋转数组的最小值查找

### 题目

- 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  
- 输入：[3,4,5,1,2]
- 输出：1

### 解题思路

- 看到排序数组的查找，首先要想到二分查找
- 二分查找能够将复杂度从n将到对数
- 关键是二分中的减治过程设计

	- array[middle] > array[right] 

		- 说明旋转点在右边
		- left = middle + 1

	- array[middle] = array[right] 

		- 无法判断旋转点在左还是右
		- 但可以确定排除不是right
		- right = right -1

	- array[middle] < array[right] 

		- 说明旋转点在左边
		- right = middle

## 12. 矩阵路径查找问题

### 题目

- 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
- [
     ["a","b","c","e"],
     ["s","f","c","s"],
     ["a","d","e","e"]
]
- 但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

### 解题思路

- 典型的矩阵中的路径搜索问题
- 可以使用 DFS + 剪枝 的解法

	- 为什么不用BFS，而是DFS
	- DFS 能够暴力遍历矩阵中的所有路径
	- BFS不适合用来遍历路径，而适合计算最短路径

- 算法流程

	- 1. 从 board数组中查找第一个  board[i][j]  == word[0];
	- 2.  visited[i][j] = true;，先暂时标记为已经访问过了
	- 3. 如果 index == word.length()，则返回true
	- 3.  遍历所有子节点，在子节点上执行DFS
	- 4.  如果所有邻接顶点经过 DFS 之后，返回false，则撤销还原 visited[i][j] = false;

## 13. 机器人运动范围问题

### 题目

- 地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

- 输入：m = 2, n = 3, k = 1
- 输出：3

### 解题思路

- 思路很清除，矩阵遍历问题，和上一题类似

	- DFS

		- 更简单一些

	- BFS

		- 使用到队列

- DFS过程中基于是否能停留的条件进行剪枝

## 14.割绳子问题

### 题目

- 给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

- 输入: 2
- 输出: 1
- 解释: 2 = 1 + 1, 1 × 1 = 1

### 解题思路

- 回溯算法+备忘录

	- 可以看作是对n个数进行划分
	- 通过回溯遍历所有可能的状态
	- 实现回溯算法
	- 添加备忘录，参照斐波拉契数列
	- F(n) = MAX{  i*F(n-i) , i*(n-i) }

- 动态规划

	- 回溯+备忘录可以解决，那么DP一定也可以
	- dp[] 表示各个长度的绳子，最大的乘积

		- 一般dp[]存储的值就是最终的目的值

	- 状态转移方程

		- dp[i] = MAX{ dp[i], MAX{  j*dp[i-j],  j*(i-j) }}
		- i = 2,3,4,5,...,n
		- j = 1,2,3,4,..., n-2

- 贪婪算法

	- 每一步选择最优解
	- 需要直到贪婪选择的条件

		- 1. 将分成以3为单位的分段越多，最后的结果越大
		- 2. 对于4，将其分为2+2，比分为3+1，结果要更好

	- 算法实现

		-  public int cuttingRope(int n) {
        if( n < 4 ){
            return n-1;
        }
        int result = 1;
        while( n > 4 ){
            result = result * 3;
            n = n - 3;
        }
        return result * n;
    }

### 拓展问题：大数溢出问题

- 题目

	- 与原题基本相同
	- 但需要对结果进行取模

		- 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

- 带来的问题

	- 之前的DP无法使用了

		- 因为DP中需要进行比较，而溢出和取模会影响大小的比较

	- 需要对结果进行取模操作

- 解题思路

	- 贪婪算法 + 循环取模

		- 循环取模原理

			- 对x的n方最终的取模，等价于乘积过程中循环取模

		- 每次计算乘积以及最后的结果都进行取模操作
		- 其余与之前一样

	- DP + BigInteger

		- 使用普通的int或long会造成溢出
		- BigInteger无限位数，解决溢出的问题
		- 缺点是，BigInteger的计算更复杂一些
		- Java提供的大数

			- BigInteger

				- 解决整数部分位数过长
				- 初始化

					- BigInteger（“111”）
					- 使用字符串进行初始化

				- 加

					- add(BigInteger val)
					- 返回计算结果

				- 减

					- subtract

				- 乘

					- multiply

				- 除

					- divide

				- 比较大

					- max
					- 返回较大数

				- 比较小

					- min
					- 返回较小数

				- 求余数

					- divideAndRemainder
					- 返回一个BigInteger

						- 0：为商
						- 1：为余数

			- BigDecimal

				- 解决小数部分位数

		- 算法实现

			- 代码

## 15. 二进制数中1个数，位运算

### 题目

- 请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

- 输入：00000000000000000000000000001011
- 输出：3
- 解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。

### 解题思路

- 数二进制中1的个数

	- n % 2, 获得末位是否为1
	- n \ 2 ，除掉末尾，即右移一位
	- 循环中计数1出现的次数，当n除成0时候，结束

- 二进制位移运算

	- 对于二进制，需要通过位运算来操作，而不是加减乘除
	- &

		- n & 1
		- 取最后一位操作

	- >>>

		- 右移操作
		- n >>> 1

			- 等价于 n / 2

	- public int hammingWeight(int n) {
        int count = 0;
        while(n!=0){
            int left = n & 1;  
            if(left == 1){
                count++;
            }
            n = n >>> 1; 
        }
        return count;
    }

- 位运算方法

	- n & （n-1）
	- n-1

		- 将二进制最右边第一个为1的位变为0 ，其后的位补为1
		- n= 10101010000, n-1为10101001111
		- n & (n-1) 为 10101000000

	- public int hammingWeight(int n) {
       int count = 0;
        while(n != 0){
            count++;
            n = n&(n-1);
        }
        return count;
    }

## 16. 数值的整数次幂（快速幂）

### 题目

- 实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

- 输入: 2.00000, 10
- 输出: 1024.00000

### 解题思路

- 快速幂算法

	- 算法复杂度 O（logN）
	- 暴力计算，需要循环计算n次，每次只乘了一个x，即只完成一步
	- 而快速幂能够先计算x的低次幂，然后以这些低次幂的乘积，通过相乘来组合最后的乘积

- 二进制角度

	- x^n次方，比如n为9
	- n的二进制为1001
	- x^5 = x^(2^3) * x^(2^0)
	- 将原本计算n次，减少到只在二进制位上计算
	- 代码实现

		- public double myPow(double x, int n) {
        if( x == 0){
            return 0;
        }
        long p = n;
        if(n< 0){
            x = 1 / x;
            p = -p;
        }
        double result = 1;
        while(p>0){
            if((p&1) == 1){
                result = result*x;
            }
            x = x*x;
            p = p >>> 1;
        }
        return result;
    }

- 二分法角度

	- 思路也是尽可能的复用计算出来的幂次，而且是优先高次

		- 思路和二进制是一样的，只是角度不同

	- 对于 x^n
	- 若n位偶数，则等价于 

		-  result = x^(n/2)
		- result = result * result

	- 若n为奇数，则等价于

		- result  = x^(n/2)
		- result = result * result * x

	- 不断对n进行二分，直到为0

		- result = 1

	- 算法实现

		- private double pow(double x, long n){
        if( n == 0){
            return 1;
        }
        if( n == 1){
            return x;
        }

        if( (n&1) == 0){
            double result = pow(x,n/2);
            return result * result;
        }else{
            double result = pow(x,n/2);
            return result * result * x;
        }

    }

## 18. 删除链表的节点

### 题目

- 给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
- 返回删除后的链表的头节点。
- 输入: head = [4,5,1,9], val = 5
- 输出: [4,1,9]
- 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.

### 解题思路

- 方法一：通过前一个节点删除

	- pre，curr
	- 1. 定位节点

		- 必须从头遍历

	- 2. 删除节点

		- pre.next = curr.next

- 方法二：将后边的节点复制到待删除的节点

	- curr.val = next.val;
curr.next = next.next;
	- 前提条件

		- 1. 待删除的节点不是尾节点，如果是尾节点，只能通过方法一

## 17.打印从1到最大的n位数

### 题目

- 输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
- 输入: n = 1
- 输出: [1,2,3,4,5,6,7,8,9]

### 解题思路

- 常规直接解决

	- 计算最大值，end = Math.pow(10, n)
	- for循环，从1到end，打印输出
	- 简单，高效，但是不是面试官的面视目的

- 用字符串模拟加法
- 回溯+大数

	- 面试官想考察的是大数解决方案

		- 当n比较大的时候，数值会特别大，用int表示会溢出
		- 用BigInteger，通过字符串方式来表达

	- 其实就是n位的全排列，然后按照顺序输出
	- 先固定高位，然后从0-9遍历低位
	- char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};

void dfs(int x) {
        if(x == n) { // 终止条件：已固定完所有位
            res.append(String.valueOf(num) + ","); // 拼接 num 并添加至 res 尾部，使用逗号隔开
            return;
        }
        for(char i : loop) { // 遍历 ‘0‘ - ’9‘
            num[x] = i; // 固定第 x 位为 i
            dfs(x + 1); // 开启固定第 x + 1 位
        }
    }

## 20. 判断字符串是否是合法数值

### 题目

- 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"、"-1E-16"及"12e+5.4"都不是。

### 解题思路

- 就是扫描字符串，模式匹配

## 21. 调整数组顺序使奇数位于偶数前面

### 题目

- 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
- 输入：nums = [1,2,3,4]
- 输出：[1,3,2,4] 
- 注：[3,1,2,4] 也是正确的答案之一。

### 解题思路

- 思路一：首位指针

	- left指针从左出发，定位第一个偶数
	- right指针从右边出发，定位第一个奇数
	- 交换right 和 left 的内容
	- 结束标志： left  > right
	- 代码实现

		- public int[] exchange(int[] nums) {
        int odd = 0;
        int even = nums.length - 1; // 收尾指针
        while (odd < even) {
            if (isEven(nums[odd]) == true && isEven(nums[even]) == false) {
                //exchange
                int temp = nums[odd];
                nums[odd] = nums[even];
                nums[even] = temp;
                odd++;
                even--;
            } else {
                if (isEven(nums[even]) == true) {
                    even--;
                }
                if (isEven(nums[odd]) == false) {
                    odd++;
                }
            }
        }
        return nums;
    }

- 思路二：快慢指针

	- fast指针：泡在前边，寻找第一个奇数

		- fast指针之前的全是偶数

	- slow指针：在后边，定位第一个偶数
	- 当fast指针找到一个奇数时候，和slow指针定位的偶数交换

		- slow++
		- fast++

	- 边界：fast指针走到最后了
	- 特殊情况：起始时候，有slow == fast 时候，说明slow位置的是奇数，需要slow++
	- 代码实现

		- public int[] exchange(int[] nums) {
        int fast = 0;
        int slow = 0;
        while (fast < nums.length) {
            if (isEven(nums[fast]) == false) { // 寻找奇数
                // fast之前，全是偶数，或则吧fast==slow
                if (slow < fast) { // 说明slow处是偶数，如果fast == slow，则都是奇数
                    int temp = nums[fast];
                    nums[fast] = nums[slow];
                    nums[slow] = temp;
                }
                slow++; // 移动到下一个待交换的偶数
            }
            fast++;
        }

        return nums;
    }

## 22. 输出链表中倒数第k个节点

### 题目

- 输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

- 给定一个链表: 1->2->3->4->5, 和 k = 2.
- 返回链表 4->5.

### 解题思路

- 双指针

	- curr = head
	- result = head

- curr 指针先提前走k步
- result 在k步之后，再从head出发
- 代码实现

	- public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode curr = head;
        ListNode result = head;
        while(curr != null){
            curr = curr.next;
            if(k > 0){ // 晚k步再出发
                k--;
            }else{
                result = result.next;
            }
        }
        return result;
    }

## 23.反转链表

### 题目

- 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
- 输入: 1->2->3->4->5->NULL
- 输出: 5->4->3->2->1->NULL

### 解题思路

- 双指针法

	- result指针，构建新链表

		- 初始为null

	- node 指针，记录原始剩余链表的首节点
	- while(head != null){
    node = head;
    head = head.next;
    node.next = result; //result 初始为null
    result = node;
}

- 递归法

	- 链表遍历可以用递归实现的
	- 逆序反转也是递归/栈的适用场合
	- 递归过程

		- 记录node 的next节点，用于回溯的时候，反向修改
		- 在复制node的next之后，将node.next设置为null，保障队首变队尾后，next为null
		- 返回新链表的首节点

	- 代码实现

		- ListNode reverse(ListNode node){
    if( node == null){
         return null;
    } else if( node.next != null ){
        ListNode next = node.next;
        node.next = null;
        ListNode result = reverse(next);
        next.next = node;
        return result;
    } else {
        return node;
    }
}

## 24. 链表合并

### 题目

- 输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
- 输入：1->2->4, 1->3->4
- 输出：1->1->2->3->4->4

### 解题思路

- 方法一：伪头节点法

	- 新链表的头节点初始位null，需要判断l1和l2的头节点来选择
	- 适用伪头节点能够大大简化这个过程，只要最后的结果返回 result.next 就可以
	- 过程

		- 1. result = new Node
		- 2. while l1 和 l2 都不为null时

			- 选择小的，插入到result尾巴

		- 3. 将l1 和 l2 中非空的，直接插入到result的尾巴

	- 代码实现

		- public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // new 一个节点，作为占位的头节点，解决头节点选择问题
        ListNode result = new ListNode(1);
        ListNode curr = result;
        // 遍历
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
        // 检查剩余的链表，直接插入到链表的结尾
        if (l1 == null) {
            curr.next = l2;
        } else {
            curr.next = l1;
        }
        return result.next;
    }

- 方法二：递归法

	- l1.next = merge( l1.next, l2) 
	- l2.next = merge( l1, l2.next )
	- 代码实现

		- private ListNode merge(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = merge(l1.next, l2);
            return l1;
        } else {
            l2.next = merge(l1, l2.next);
            return l2;
        }
    }

## 25. 判断树的子结构

### 题目

- 输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

### 解题思路

- 1. 先序遍历A树的每个节点

	- if(isSub(A,B)){
    return true;
}else{
    if(A == null) return false;
    return df(A.left, B) || df(A.right, B);
}
	- 对每个节点执行isSub( node, B)

- 2. 对A的每个节点，执行判断以该节点为根的树是否包含B树

	- if  B == null  return true;

		- B中分支可以为空，部分匹配A树

	- if( A.val ！=  B val)  return false

		- 不相等，排除

	- if A == null, B != null return false
	- if( A.val == B.val)

		- A，B根相同，则继续判断各自的左右子树
		- result  = isSub( A.left, B.left)
		- result = iresult && sSub(A.right, B.right)
		- return result

## 26.二叉树镜像

### 题目

- 请完成一个函数，输入一个二叉树，该函数输出它的镜像。

### 解题思路

- 先序遍历

	- 思路很简单，遍历每个节点，交换他的左右节点即可
	- 代码实现

		- public TreeNode mirrorTree(TreeNode root) {
        if(root != null){
            TreeNode temp = root.left;
            root.left = root.right;
            root.right = temp;
            mirrorTree(root.left);
            mirrorTree(root.right);
        }
        return root;
    }

- 递归实现

	- 利用递归的返回值，来实现左右的交换
	- 更加简介和优雅
	-     public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        TreeNode temp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(temp);
        return root;
    }

## 28. 判断是否是对称二叉树

### 题目

- 请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

### 解题思路

- 对称二叉树的定义

	- 每个节点的左右节点L和R

		- L.val = R.val
		- L.左子树 和 R.右子树对称
		- L.右子树 和 R. 左子树对称

	- 子主题 2

- 左右子树需要进行比较

	- isSymNodes(root.left, root.right)

- 代码实现

	- class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return isSymNodes(root.left, root.right);
    }

    private boolean isSymNodes(TreeNode left, TreeNode right){
        if(left == null && right == null) return true;
        if(left == null || right == null) return false;
        if(left.val != right.val)         return false;
        return isSymNodes(left.left,right.right) && isSymNodes(left.right,right.left);
    }
}

## 29.顺时针打印矩阵

### 题目

- 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
- 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
- 输出：[1,2,3,6,9,8,7,4,5]

### 解题思路

- 方法一：四个哨兵

	- 上下左右边界各一个哨兵

		- left = 0
		- right = matrix[0].length -1
		- up = 0
		- down = matrix.length -1

	- 向右走到头

		- 即，for(i=left; i<=right; i++)

			- result[index++] = matrix[up][i]

		- 向右走到头后，尝试向下走

			- up++
			- 判断能走吗

				- if（up > down） break;

		- 然后依次向下，向左，向上

	- 终止条件

		- 向右前，left  > right
		- 向下前，up > down
		- 向左之前，right < left
		- 向上之前，down < up

	- 代码实现

		- while(true){

            // 向左走
            for(int i = left; i<=right;i++){
                result[index++] = matrix[up][i];
            }

            up++; // 上限下移
            if(up > down)break;
            // 向下走
            for (int i = up;i<=down;i++){
                result[index++] = matrix[i][right];
            }

            right--;
            if(right < left) break;
            // 向左走
            for (int i= right; i>=left;i--){
                result[index++] = matrix[down][i];
            }

            down++;
            if(down<up)break;
            // 向上走
            for (int i = down; i >= up ; i--) {
                result[index++] = matrix[i][left];
            }

            left++;
            if(left > right)break;
        }

- 方法二：数圈层

	- 顺时针其实是在沿边缘向内部绕圈
	- count 标记边界距离外边界的偏移

		- 初始count=0
		- 均能访问到矩阵边界

	- 每次完成向上操作之后，count++，增加一个距离边界的距离
	- 终止条件

		- 向右之前，j >= width - count
		- 向下之前,i >= height -count
		- 向左之前，j < count
		- 向上之前，i < count

	- 代码实现

		- while (true) {
            if (j >= width - count) break; // 判断向右还有路不
            // 向右走
            while (j < width - count) {
                result[index] = matrix[i][j];
                index++;
                j++;
            }
            j--; // 走过了，回退一步
            i++; // 尝试向下走一格
            if (i >= height - count) break; // 判断尝试走的一格是否可以
            // 向下走
            while (i < height - count) {
                result[index] = matrix[i][j];
                index++;
                i++;
            }
            i--; // 走过了，回退以步
            j--;
            if (j < count) break;
            while (j >= count) {
                result[index] = matrix[i][j];
                index++;
                j--;
            }
            j++;
            count++;
            i--;
            if (i < count) break;
            while (i >= count) {
                result[index] = matrix[i][j];
                index++;
                i--;
            }
            i++;
            j++;
        }

## 30.具有min函数的栈

### 题目

- 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

- 注意，min方法只需要获取最小值，不需要最小值出栈

### 解题思路

- 思路一：将最小值记录在栈中

	- int min，记录当前的最小值
	- push(x)

		- if x <= min

			- 注意，是<=，等于min时候也要将min压栈
			- 将旧的min值，压入栈中

				- push(min)

			- 更新min值

				- min = x

			- 再压入值

				- push(x)

		- if x > min

			- 最小值没变，直接压栈

				- push(x)

	- pop

		- if pop().equls( min ) 

			- 说明最小值出栈了，min需要更新了
			- pop()第一次，获取结果
			- pop()第二次，获取剩余元素的min

				- min = pop

		- if pop() != min

			- 直接pop返回即可

	- min

		- 返回 min的值即可

	- 代码实现

		-     private Stack<Integer> stack;
    private int min = Integer.MAX_VALUE;
    public SingleStackSolution() {
        stack = new Stack<>();
    }
    public void push(int x) {
        if (x <= min) { // 最小值需要更新
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }
    public void pop() {
        if (stack.pop().equals(min)) {
            min = stack.pop(); // 最小值出栈了，更新最小值
        }
    }
    public int top() {
        return stack.peek();
    }
    public int min() {
        return min;
    }

- 思路二：辅助栈实现

	- 与思路一实际是一样的，只是把最小值记录在了辅助栈中
	- stack，正常的栈
	- helpStack，记录最小值的栈
	- push(x)

		- 当 helpStack为空，或者x比helpStack栈顶的元素 < =时，将x同时也压入到helpStack栈中
		- 否则，x只压入stack中

	- pop(x)

		- 如果stack的栈顶元素与helpStack的栈顶元素相同

			- 在stack.pop的同时
			- helpStack.pop

		- 不同，则只pop stack的栈顶

	- min()

		- 返回helpStack的栈顶元素

	- 代码实现

		-     public void push(int x) {
        if (minStack.isEmpty() || x <= minStack.peek()) {
            minStack.push(x);
        }
        stack.push(x);
    }
    public void pop() {
        if (minStack.peek().equals(stack.pop())) {
            minStack.pop();
        }
    }
    public int top() {
        return stack.peek();
    }
    public int min() {
        return minStack.peek();
    }

- 思路三：链表实现，Node中记录min

	- 其实思路一样，只是实现不同

## 31.栈的压入与弹出序列匹配

### 题目

- 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

- 输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
- 输出：true

### 解题思路

- 将pushed中的元素依次压入栈中
- 每次压入后，判断栈顶元素和 poped 中元素是否相等

	- 相等

		- 则pop栈顶元素
		- popped后移到下一个元素
		- pop之后，再循环判断栈顶元素和新的popped元素是否相等

			- 相等，继续pop
			- 不相等，退出循环，继续将pushed元素压入栈中

	- 不相等

		- 继续向栈中压入pushed的下一个元素

- 最终，pushed元素全部入栈退出循环后，检查stack是否为空，如果空，则合法
- 代码实现

	- private Stack<Integer> stack;
    public boolean validateStackSequences(int[] pushed, int[] popped) {
       stack = new Stack<>();
        int j = 0; // popped数组的当前下标
        for (int i = 0; i < pushed.length; i++) {
            stack.push(pushed[i]); // 入栈
            for (; j < popped.length; j++) {
                if (!stack.isEmpty() && stack.peek().equals(popped[j])) {
                    // 如果与栈顶相等，则pop
                    stack.pop();
                } else {
                    break; // 否则，继续push元素
                }
            }
        }
        return stack.isEmpty();//最终栈为空，则ok

    }

## 32. 二叉树的分层遍历

### 题目

- 基础：二叉树的层序遍历
- 拓展1：每个层单独输出
- 拓展2：奇偶层逆向输出

### 解题思路

- 基础分层遍历

	- BFS，通过队列解决

		- 原理思路

			- 将元素按层依次加入到队列中
			- 然后再在队列中遍历每个元素，将他们的下层的元素再依次添加到队列中
			- 最终的访问顺序就是分层访问的效果

		- 过程

			- 初始将root加入到队列中
			- 循环处理队列中的元素，直到队列元素为空

				- 1. 取队列首元素，node
				- 2. 输出node.val
				- 3. 如果左节点非空，则将左节点加入队列
				- 4. 如果右节点非空，则将右节点加入队列
				- 循环往复

		- 代码实现

			- public int[] levelOrder(TreeNode root) {
        if(root == null) return new int[0];
        LinkedList<TreeNode> queue = new LinkedList<>();
        ArrayList<Integer> result = new ArrayList<>();
        queue.addLast(root); // 默认将root加入
        while(!queue.isEmpty()){
            TreeNode node = queue.pollFirst();
            result.add(node.val);
            if(node.left !=null){
                queue.addLast(node.left); // 将左子树加入
            }
            if(node.right != null){
                queue.addLast(node.right); // 将右子树加入
            }
        }
        return result;
    }

	- 其他方法不是很好搞定

- 拓展1，每层单独输出

	- BFS

		- 基础还是基于BFS来遍历
		- 对每一层，从队列首取元素进行技术，该层遍历结束了，则进入下一层
		- 代码实现

			- while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> values = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                values.add(node.val);
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            result.add(values);
        }

	- DFS

		- 在先序遍历的时候，带上遍历的深度即可
		- 代码实现

			- private void dfs(TreeNode root, int depth){
        if(root == null) return;

        if(depth >= res.size()){ // 第一次遍历到该层
            res.add(new ArrayList<>());
        }
        res.get(depth).add(root.val);

        dfs(root.left,depth+1);
        dfs(root.right,depth+1);
    }

- 拓展2，奇偶层逆序输出

	- 每一层的遍历结果暂存在一个双端队列中

		- 奇数层，addFirst
		- 偶数层，addLast

	- BFS

		- 代码实现

			- while (!queue.isEmpty()) {
            int size = queue.size();
            LinkedList<Integer> values = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if((result.size()&1) == 0){
                    values.addLast(node.val);
                }else {
                    values.addFirst(node.val);
                }
                if (node.left != null) queue.add(node.left);
                if (node.right != null) queue.add(node.right);
            }
            result.add(values);
        }

	- DFS

		-     private void dfs(TreeNode root, int depth){
        if(root == null) return;
        if(depth >= res.size()) res.add(new LinkedList<>());
        LinkedList<Integer> nodes = (LinkedList<Integer>) res.get(depth);
        if((depth&1) == 1) nodes.addFirst(root.val);
        else nodes.addLast(root.val);
        dfs(root.left,depth+1);
        dfs(root.right,depth+1);
    }
		- 错误的写法

			- 并不是只要交换左右子树的顺序就满足逆序了
			- 因为交换了左右子树的顺序，会造成子树的子树顺序也乱了
			-  if((depth&1) == 1){
            dfs(root.left,depth+1);
            dfs(root.right,depth+1);
        }else{
            dfs(root.right,depth+1);
            dfs(root.left,depth+1);
        }

## 33.二叉搜索树的后序遍历序列

### 题目

- 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

### 解题思路

- 递归思路

	- 后续遍历的组成： 【左子树，右子树，根】
	- 从左向右遍历，第一个比根大的节点，便能找到左右子树的分界

		- 【i，j】

			- m为第一个值大于 j 的位置
			- 左子树：【i,m-1】
			- 右子树：【m，j-1】
			- 根：j

	- 判断是否满足后序遍历的条件

		- 左子树节点值都小于根

			- 这个遍历找m肯定的

		- 右子树节点值都大于根

			- 这个需要继续遍历判断

	- 递归三要素

		- Base Case

			- i = j，则只有一个节点，为true
			- i > j，区间为空，为true

		- 拆解

			- 左子树满足后序遍历吗

				- recur(seq, i, m-1)

			- 右子树满足后序遍历吗

				- recur(seq, m, j-1)

		- 组合

			- return 当前满足 && recur(左子树) && recur（右子树）

	- 代码实现

		- private boolean recur(int[] postorder, int i, int j) {
        if (i >= j) return true;
        int m = i;// 找到左右子树的分界点
        while (postorder[m] < postorder[j]) {
            m++;
        }
        boolean result = true;
        for (int k = m; k < j - 1; k++) {
            if (postorder[k] < postorder[j]) {
                result = false;
                break;
            }
        }
        return result && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
    }

- 单调栈思路

	- 还没搞懂

## 34. 二叉树中和为指定长的路径

### 题目

- 输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

### 解题思路

- 基于先序遍历，在遍历过程中，记录路径和长度
- 当到达叶子节点，判断是否等于指定值
- 难点在于代码实现和细节处理
- 路径通过一个共享的path List来存储

	- 在回溯完一个节点之后，将当前节点从path中移除

- 代码实现

	- private List<List<Integer>> result;
    private LinkedList<Integer> path;
    public List<List<Integer>> 
    private void df(TreeNode root,int target){
        if(root == null){
            return;
        }
        path.add(root.val);
        target -= root.val;
        if(root.left == null && root.right==null){
            if(target == 0) result.add(new ArrayList<>(path));
            return;
        }
        df(root.left,target);
        df(root.right,target);
        path.removeLast();
    }

## 35.复杂链表的复制

### 题目

- 请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

### 解题思路

- 方法一：HashMap

	- HashMap<Node, Node>

		- 实现源节点和复制节点的映射
		- 在设置random指针时，基于原节点查找复制节点

	- 两次循环

		- 第一次循环，沿着next方向遍历，创建新的节点，并将节点对应关系存储到HashMap中
		- 第二次循环，遍历设置每个新节点的random指针

	- 代码实现

		- public Node copyRandomList(Node head) {
        HashMap<Node,Node> hashMap = new HashMap<>();
        hashMap.put(null,null);
        Node curr = head;
        while (curr != null){
            hashMap.put(curr,new Node(curr.val));
            curr = curr.next;
        }
        curr = head;
        while (curr!= null){
            hashMap.get(curr).next = hashMap.get(curr.next);
            hashMap.get(curr).random = hashMap.get(curr.random);
            curr = curr.next;
        }
        return hashMap.get(head);
    }

- 方法二：一个链表

	- 3次循环

		- 第一次循环，在每个节点之后插入一个复制节点

			- 复制节点在每个原节点的后继
			- 不需要使用HashMap来映射了

		- 第二次循环，更新复制节点的random指针

			- 把原节点的ramdom.next赋值给复制节点的random

		- 第三次循环，拆分成两个链表

	- 代码实现

		- public Node copyRandomList(Node head) {
        // 拷贝节点，放在每个原节点的下一个节点
        Node find = head;
        while (find!=null){
            Node cloneNode = new Node(find.val);
            cloneNode.next = find.next;
            find.next = cloneNode;
            find = cloneNode.next;
        }
        // 整理随机指针
        find = head;
        while (find != null){
            if(find.random == null){
                find.next.random = null;
            }else{
                find.next.random = find.random.next;
            }
            find = find.next.next;
        }
        // 拆分链表
        Node result = new Node(0); // 哨兵
        Node curr = result;
        find=head;
        while (find != null){
            Node cloneNode = find.next;
            curr.next = cloneNode;
            find.next = cloneNode.next;
            find = find.next;
            curr = curr.next;
        }
        return result.next;
    }

	- 复杂度

		- 空间O(1)
		- 时间O(n)

- 方法三：当作图的DFS和BFS

	- 对原始的复杂链表按照图的方式进行DFS或者BFS遍历
	- 也是需要使用HashMap来进行源节点和复制节点之间的映射
	- 代码实现

		- private Node dfs(Node head) {
        if (head == null) return null;
        if (nodes.get(head) == null) { // 未复制过
            Node cloneNode = new Node(head.val);
            nodes.put(head, cloneNode);
            cloneNode.next = dfs(head.next);
            cloneNode.random = dfs(head.random);
            return cloneNode;
        } else {
            return nodes.get(head);
        }
    }

## 36.二叉搜索树与双向链表

### 题目

- 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

### 解题思路

- 方法一：中序遍历思路

	- 对二叉搜索树进行中序遍历，输出的结果是增序排序的
	- 所以可以在中序遍历的过程中构造双向链表
	- 最终将双向链表组织成循环链表
	- 代码实现

		- private Node head = new Node(1);
    private Node curr = head;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        inorder(root);
        curr.right = head.right;
        head = head.right;
        head.left = curr;
        return head;
    }

    private void inorder(Node root){
        if(root == null) return;
        inorder(root.left);
        // 修改节点指针
        curr.right = root;
        root.left = curr;
        curr = curr.right;
        inorder(root.right);
    }

- 方法二：递归思路

	- 递归三要素

		- BaseCase

			- root为null，则返回null

		- 拆解

			- Node leftHead = recur(root.left)
			- Node rightHead = recur(root.right)

		- 组合

			- 将左子树循环链表、右子树循环链表、以及root节点组织成一个循环链表

	- 代码实现

		- public Node treeToDoublyList(Node root) {
        if (root == null) return null;
        Node leftHead = treeToDoublyList(root.left);
        Node rightHead = treeToDoublyList(root.right);
        if (leftHead == null && rightHead == null) {
            root.left = root;
            root.right = root;
            return root;
        } else if (leftHead == null) {
            root.right = rightHead;
            root.left = rightHead.left;
            rightHead.left.right = root;
            rightHead.left = root;
            return root;
        } else if (rightHead == null) {
            root.left = leftHead.left;
            root.right = leftHead;
            leftHead.left.right = root;
            leftHead.left = root;
            return leftHead;
        } else {
            root.left = leftHead.left;
            root.right = rightHead;

            leftHead.left.right = root;
            leftHead.left = rightHead.left;

            rightHead.left.right = leftHead;
            rightHead.left = root;
            return leftHead;
        }
    }

## 37.二叉树的序列化和反序列化

### 题目

- 请实现两个函数，分别用来序列化和反序列化二叉树。
- 序列化为 "[1,2,3,null,null,4,5]"

### 二叉树序列化

- 层序遍历，StringBuilder，字符串输出
- 需要判断什么时候终止输出，因为null也要输出
- 代码实现

	- while (!queue.isEmpty()){
            int size = queue.size();
            boolean haxNext = false;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.pollFirst();
                if(node == null){
                    builder.append(",null");
                }else{
                    builder.append(","+node.val);
                    queue.addLast(node.left);
                    queue.addLast(node.right);
                    if(node.left != null || node.right!=null){
                        haxNext = true;
                    }
                }
            }
            if(!haxNext)break;
        }

### 二叉树反序列化

- 输入 String字符串

	- 从String中解析出节点值数组

- 输出 roo 节点
- 依次遍历节点值

	- 头节点先加入队列
	- 队列首读取节点

		- 读取节点值
		- 分别设置为左右节点

	- 直到节点值数组遍历结束

- 代码实现

	- TreeNode root = new TreeNode(Integer.valueOf(values[0]));
        queue.addLast(root);
        int index = 1;
        while (!queue.isEmpty()){
            TreeNode node = queue.pollFirst();
            if(index < values.length){
                if(values[index].equals("null")){
                    node.left = null;
                }else{
                    node.left = new TreeNode(Integer.valueOf(values[index]));
                    queue.addLast(node.left);
                }
                index++;
            }else break;
            if(index < values.length){
                if(values[index].equals("null")){
                    node.right = null;
                }else{
                    node.right = new TreeNode(Integer.valueOf(values[index]));
                    queue.addLast(node.right);
                }
                index++;
            }else break;
        }

## 38. 字符全排列组合

### 题目

- 输入一个字符串，打印出该字符串中字符的所有排列。
- 你可以以任意顺序返回这个字符串数组，但里面不能有重复元素
- 输入：s = "abc"
- 输出：["abc","acb","bac","bca","cab","cba"]

### 全排列解题思路

- DFS解决全排列问题的解题过程

	- 1. 先画出全排列问题的递归树

		- 是一个隐形树
		- 节点如何生成：在剩余字符中选择
		- 结果位置：结果在叶子节点生成，结果值为path路径

	- 2. 设计递归树的状态参数，决定递归方法参数
	- 3. 通过 DFS 来对递归树进行遍历

		- 注意，是对一个隐身树进行遍历

			- 如何实现对隐身树的遍历

		- 回溯：在最后记得恢复现场

	- 4. 能否剪枝

		- 剪枝条件是什么

- 转化输入

	- 字符串不好处理
	- s.toCharArray()，转化为字符数组

- DFS + 回溯 + 剪枝

	- values = char []

		- 输入的字符数组

	- result = new ArrayList<String>()

		- 记录结果

	- path = new LinkedList<Charactor>()

		- 记录DFS过程中的路径，也是用于生成最终的排列

	- used = boolean [ ]

		- 标记该字符是否已经被选在在path里了，在则不能选了

	- dfs(int depth)

		- 参数depth代表目前在哪一个位置选择字符，同时也是递归树的深度
		- 当 depth  = values.length

			- 说明完成一次排列，到达叶子节点
			- path内容添加到result

				- 注意，需要拷贝path，而不是直接添加

		- 否则，从剩余字符中选择一个，for循环，把每个可能都尝试以下

			- 避免重复剪枝

				- 通过Set来解决
				- 将尝试过的字符都加入到set中
				- 尝试下一个字符时，如果发现字符再set中已存在，则说明重复了，跳过这个字符，剪枝

			- if used[i] == true  continue;

		- 选择一个字符之后，DFS 进入下一层

			- path.add(values[i])
			- used[i] = true;
			- dfs( depth + 1 )

		- 回溯恢复现场

			- path.removeLast();
			- used[i] = false;
			- 该位置这个尝试完了，换下一个再DFS

	- 最终结果

		- return result.toArray(new String(result.size()))

	- 复杂度

		- 时间 O( N * N！)
		- 空间 O（N * N！）

	- 代码实现

		- private void dfs(int depth){
        if(depth == values.length){
            result.add(path.toString());
            return;
        }
        HashSet<Character> set = new HashSet<>();
        for(int i = 0; i<values.length;i++){
            if(!used[i] && !set.contains(values[i])){ // 标记是否在path中
                used[i] = true;
                set.add(values[i]);
                path.append(values[i]);
                dfs(depth +1);
                path.deleteCharAt(depth);
                used[i] = false;
            }
        }
    }

- DFS + 交换 + 剪枝

	- values = char []

		- 输入的字符数组

	- result = new ArrayList<String>()

		- 记录结果

	- 不需要 used，path等辅助
	- dfs(int depth)

		- 当 depth  = values.length

			- 将value内容添加到result
			- 而之前是将path添加到result

		- 否则，从剩余字符中选择一个，for循环，把每个可能都尝试以下

			- 避免重复剪枝

				- 通过Set来解决
				- 与之前算法一样

			- for 循环只在depth之后位置进行，并且通过交换来实现尝试选择不同得值

		- 选择一个字符之后，DFS 进入下一层

			- set.add(values[i])
			- swap( values, depth, i )

				- 交换将i位置的值拿来尝试

			- dfs( depth + 1 )

		- 回溯恢复现场

			- swap( values, depth, i)

				- 回溯再交换回来

			- 该位置这个尝试完了，换下一个再DFS

	- 代码实现

		- private void dfs(int depth) {
        // 问题的解在path上，在叶子节点结束
        if (depth == values.length) {
            // 将values数组中的字符串存到结果中
            result.add(new String(values));
            return;
        }
        // 记录当前位置是否和之前的某次重复
        Set<Character> set = new HashSet<>();
        // for循环是分支的生成，从剩余的选择一个
        for (int i = depth; i < values.length; i++) {
            // 未重复，则继续。该字符试过了，则剪枝
            if (!set.contains(values[i])) {
                set.add(values[i]); // 将尝试字符加入set
                // 交换位置
                char temp = values[depth];
                values[depth] = values[i];
                values[i] = temp;
                dfs(depth + 1);
                // 回溯交换回来
                temp = values[depth];
                values[depth] = values[i];
                values[i] = temp;
            }
        }
    }

## 39. （众数）数组中出现次数超过一半的数字

### 题目

- 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

- 你可以假设数组是非空的，并且给定的数组总是存在多数元素
- 输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
- 输出: 2

### 解题思路

- 排序取中间位置

	- 复杂度O（NlogN）

- HashMap 频率统计

	- hashMap = new HashMap<Integer, Integer>
	- 将每个数字出现的频率映射到hashMap
	- 当出现频率超过一半的，返回

- 摩尔投票法

	- 特别适用于查找众数 
	- 人海战术

		- 众数代表我方 + 1，非众数代表敌方  -1，结果vote为两方势力

			- 两方单兵相遇实力抵消 ，主要拼数量

		- 两方同归于尽之后 vote == 0 

			- 之前的都不算了
			- 重新指定众数兵，再PK

		- 最终要么剩下一个众数，要么多个众数

			- 最后一次指定的一定属于众数
			- 因为如果是非众数，肯定会被后边的众数抵消掉

	- 思路

		- 初始态

			- vote = 0
			- halfNum = array[0]

		- 遍历array

			- 如果 array[i] == halfNum

				- vote++

			- 否则

				- vote--

			- 如果 vote == 0 

				- 更新 halfNum = array[i]

		- 最终，return halfNum

	- 代码实现

		- public int majorityElement(int[] nums) {
        int halfNum = 0;
        int vote = 0; // 初始假设第一个是众数
        for (int i = 0; i < nums.length; i++) {
            if(vote == 0){ //抵消
                halfNum = nums[i];
            }
            if(nums[i] == halfNum){
                vote++;
            }else{
                vote--;
            }
        }
        return halfNum;
    }

## 40. 最小的前K个数（TOPK问题）

### 题目

- 输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
- 输入：arr = [3,2,1], k = 2
- 输出：[1,2] 或者 [2,1]

### 解题思路

- 思路一：二叉堆

	- 思路

		- Java中的PriorityQueue，内部基于最小堆实现
		- 但实际要使用最大堆，即将前K小数的最大的放在根

			- 传入Comparator，重写compare
			- 注意 PriorityQueue是最小堆实现，所以数值小的优先级高
			- public int compare(Integer o1, Integer o2) {
  // PriorityQueue为最小堆，值小的优先级高
                return -(o1 - o2);
 }

	- 过程

		- 1. 初始化将K个元素加入二叉堆中
		- 2. 遍历剩余的元素，如果比堆顶小，则替换堆顶
		- 3. 遍历结束，堆中存储的就是前K小

	- 代码实现

		- for (int i = 0; i < k; i++) {
            queue.add(arr[i]);
        }
        for (int i = k; i <arr.length ; i++) {
            if(arr[i] < queue.peek()){
                // 替换最小值中的最大的
                queue.poll();
                queue.add(arr[i]);
            }
        }

- 思路二：类快速排序的快速查找

	- 快速排序中，每次partition都能确定基准值的位置

		- 左边的比基准值小
		- 右边的比基准值大

	- 我们只要确定K位置的基准值就可以了，这样K左边的都比自己小
	- 改造快排算法

		- if(middle == k) return
		- 最终返回结果的前K项

	- 复杂度 O（N）

- 思路三：排序，取前K

	- O（NlogN）

## 42.连续子数组的最大和问题

### 题目

- 输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
- 要求时间复杂度为O(n)。
- 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
- 输出: 6

	- 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

### 解题思路

- 动态规划

	- dp[i] 表示到 i 位置截至的最大和
	- 递推方程

		- 如果 dp[ i-1 ] > 0，则 dp[i] = num[i] + dp[i-1]
		- 如果 dp[ i-1 ] <= 0，则dp[i] = num[i]

	- 由于只需要使用到前后两个dp的值，所以可以通过两个变量替代dp数组
	- 代码实现

		- public int maxSubArray(int[] nums) {
        if(nums.length == 0)return 0;
        int dpPre = nums[0];
        int dpCur = nums[0];
        int maxSub = dpCur;
        for (int i = 1; i <nums.length; i++) {
            if(dpPre > 0){
                dpCur = dpPre + nums[i];
            }else{
                dpCur = nums[i];
            }
            dpPre = dpCur;
            if(dpCur > maxSub) maxSub = dpCur;
        }
        return maxSub;
    }

	- 复杂度 O(N)

- 分治法

	- 不断对数组进行二分，取左，右，中最大的
	- 递归实现分治

		- 递归方法 int recur(int [] nums，int start，int end)
		- BaseCase

			- start >= end，return nums[start];

		- 拆解

			- int left = recur(nums, start, middle -1)

				- 左边的最大和

			- int right = recur(nums, middle+1, end)

				- 右边的最大和

			- 横跨中点的最大和

				- 计算左边以middle为后缀的最大和
				- 计算右边以middle为起始的最大和
				- 计算整个的最大和

		- 组合

			- 从 left、right 以及横跨中点的最大和中选择最大的

	- 代码实现

		- private int recur(int[] nums, int start, int end) {
        if (start >= end) {
            return nums[start];
        }
        // 中间位置
        int middle = (start + end) / 2;

        int left = recur(nums, start, middle-1);
        int right = recur(nums, middle + 1, end);

        int leftSub = 0, maxLeftSub = 0, rightSub = 0, maxRightSub = 0;
        for (int i = middle - 1; i >= start; i--) {
            leftSub += nums[i];
            maxLeftSub = Math.max(leftSub,maxLeftSub);
        }
        for (int i = middle + 1; i <= end; i++) {
            rightSub += nums[i];
            maxRightSub = Math.max(rightSub,maxRightSub);
        }
        int middleSub = nums[middle];
        if (maxLeftSub > 0) middleSub += maxLeftSub;
        if (maxRightSub > 0) middleSub += maxRightSub;
        return Math.max(Math.max(left, right), middleSub);
    }

## 43.1～n整数中1出现的次数

### 题目

- 输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。
- 例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

### 解题思路

- 递归法

	- BaseCase

		- 

	- 拆解

		- 设定

			- 最高位 high
			- 余下值 last
			- power，对应最高位上10的幂次

		- recur(last)

	- 组合

		- 如果high == 1

			- 比如1222

				- high=1
				- power=1000
				- last = 222

			- result

				- recur( power-1 )

					- high 为0 的时候，0-999中1的个数

				- recur(last)

					- high为1时，last部分1的个数

				- last +1

					- high为1时，最高位上1的个数

		- 如果high > 1

			- 比如 2333

				- high=2
				- power = 1000
				- last = 333

			- result

				- high * recur( power -1)

					- hight 为0,1，第三位0-999中1的个数

				- power

					- 最高位为1的个数，即high取1

				- last + 1

					- high整后，零头last中1的个数

	- 代码实现

		- public int countDigitOne(int n) {
        if(n == 0) return 0;
        String num = String.valueOf(n);
        int high = num.charAt(0) - '0';
        int highPow = (int)Math.pow(10,num.length()-1);
        int last = n - high * highPow;
        if(high== 1){
            return countDigitOne(highPow-1) // high位取0的
                    + last +1 // high位取1，high位上的1个数
                    + countDigitOne(last); // high取1，last中的1个数
        }else{
            return high * countDigitOne(highPow-1) // 0 - high-1 ，低位中1的个数
                    + highPow // high以1开头的
                    + countDigitOne(last); // high为最大时候，余下的last有多少个1
        }
    }

	- 复杂度

		- O(log10N)

- 按位统计

	- 分为3个部分

		- high curr last

			- curr，当前位上的值
			- high，高位数字组成的值
			- last，低位数字组成的值

		- 123456

			- 如果curr == 4
			- high = 123
			- last = 56

	- 统计过程

		- 遍历n的每一位 curr
		- 设定

			- digit 为高位对应的10的幂
			- 比如上边的digit = 1000

		- 如果curr == 0

			- 该位上1的个数，high*digit
			- 即 100 - 123100

		- 如果curr == 1

			- high*digit + last + 1
			- 在上边的基础上，多一个last+1的零头

		- 如果curr > 1

			- （high+1）*digit
			- 整体的high*digit个，还多出一个

	- 代码实现

		- public int countDigitOneNew(int n) {
        int digit = 1, res = 0;
        int high = n / 10, cur = n % 10, low = 0;
        while(high != 0 || cur != 0) {
            if(cur == 0) res += high * digit;
            else if(cur == 1) res += high * digit + low + 1;
            else res += (high + 1) * digit;
            low += cur * digit;
            cur = high % 10;
            high /= 10;
            digit *= 10;
        }
        return res;
    }

	- 复杂度

		- O(N)

## 44.  数字序列中某一位的数字

### 题目

- 数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。
- 输入：n = 11
- 输出：0

### 解题思路

- 数字序列可以组成一棵树

	- 第一层为 0

		- 1

	- 第二层为 1-9

		- 9x1x1

	- 第三层为 第二层每个节点末尾添加 0-9

		- 9x10x2

	- 9x100x3
	- 9x digit x i

		- digit：10的位数次
		- i：几位数

- 过程

	- 1. 定位到在树的哪一层

		- 循环减去 9*digit*i

	- 2. 定位到该层的哪一个位置

		- digit + (n-1) / i

	- 3. 定位到数字的位上的数字

		- offset = (n-1) % i

- 代码实现

	- public int findNthDigit(int n) {
        int i = 1;
        long digit = 1;
        while (n > 9*digit*i){
            n -= 9*digit*i;
            i++;
            digit *= 10;
        }
        int last = (n-1) / i;
        int offset = (n-1) % i;
        long num = digit + last;
        String numStr = Long.toString(num);
        return numStr.charAt(offset)-'0';
    }

- 复杂度

	- O（log10N）

## 45.把数组排成最小的数

### 题目

- 输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个
- 输入: [10,2]
- 输出: "102"

### 解题思路

- 定义数字大小比较

	- x + y > y + x，则 x > y
	- 其中加法为字符串加法

- 所以，只要基于此对数组按照规则进行排序就可以

	- 基于快速排序

- 最终按升序输出即可

### 代码实现

- public String minNumber(int[] nums) {
        String[] values = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            values[i] = String.valueOf(nums[i]);
        }
        fastSort(values, 0, values.length - 1);
        StringBuilder builder = new StringBuilder();
        for (String str : values) {
            builder.append(str);
        }
        return builder.toString();
    }

## 46. 把数字翻译成字符串

### 题目

- 给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。
- 输入: 12258
- 输出: 5

	- 解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"

### 解题思路

- 方法一：回溯算法

	- 1. 划出递归树

		- 树的分支为下一个字符的选择问题
		- 有一个分支或者两个分支

	- 2. 状态变量

		- left，即当前还有多少个数字字符没翻译

	- 3. 剪枝条件

		- 如果数值是0，3...9，则肯定无法和后一个元素组合
		- 如果数值是2，但后一个数 > 5，也无法组合

	- 4. dfs过程

		- private void dfs(int left){
        if(left == 0){ // 数字使用完，count++
            count++;
            return;
        }
        dfs(left-1); // 用一个数字来翻译
        int len = value.length();
        if(left > 1){ // 用两个数字来翻译，需要满足10-25的条件
            if(value.charAt(len-left) == '1'
                    || value.charAt(len-left) == '2' && value.charAt(len-left + 1) < '6'){
                dfs(left-2);
            }
        }
    }

- 方法二：动态规划

	- 1. 递推方程

		- dp[i]，表示以 i 位置数字结尾的翻译有多少种
		- dp[i]

			- dp[ i-1 ]，当 i 无法和 i-1 进行组合
			- dp [ i-1] + dp[ i-2 ] ，当 i 可以和 i-1 进行组合

	- 2. 代码实现

		- int preDP = 1;
        int currDP = 1;
        for(int i = 1; i<value.length(); i++){
            if(value.charAt(i-1) == '1'
                    || value.charAt(i-1) == '2' && value.charAt(i)<'6'){
                int temp = currDP;
                currDP = currDP + preDP;
                preDP = temp;
            }else{
                preDP = currDP;
                currDP = currDP;
            }
        }
        return currDP;

## 47.礼物的最大价值

### 题目

- 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

### 解题思路

- 动态规划

	- 这是典型的动态规划的题目变形
	- 定义 dp

		- dp[i][j] 表示到达该位置的最大价值 
		- 这里比较只会用到左边和上边的元素，所以一个一维数组也可以搞定， 即dp[i]

	- 递推方程

		- dp[ i ][ j ] = dp[ i ][ j-1 ] + grid[ i ][ j ]

			- 当左边比上边大

		- dp[ i ][ j ] = dp[ i-1 ][ j ]  + grid[ i ][ j ]

			- 当上边比左边大

	- 代码实现

		- public int maxValue(int[][] grid) {
        if(grid.length == 0 || grid[0].length == 0) return 0;
        int [] dp = new int[grid[0].length];
        dp[0] = grid[0][0];
        for (int i = 1; i < grid[0].length; i++) {
            dp[i] = dp[i-1]+grid[0][i];
        }
        for (int i = 1; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if(j-1 <0){
                    dp[j] = dp[j] + grid[i][j];
                }else {
                    if(dp[j-1] > dp[j]){
                        dp[j] = dp[j-1] + grid[i][j];
                    }else{
                        dp[j] = dp[j]+grid[i][j];
                    }
                }
            }
        }
        return dp[grid[0].length-1];
    }

	- 评估

		- 时间复杂度 O(M*N)
		- 空间复杂度O（M）

- DFS + 备忘录

	- 从起点到终点是一个DFS遍历的过程
	- 分支生成规则

		- 可以向下、向右
		- 右边缘和下边缘只能有一个方向，即一个分支

	- 剪枝规则

		- 备忘录记录各个点已经计算的最大价值
		- 当再此来到这个位置时候，计算最大值，并与已有最大值比较，若小，则直接剪枝

	- 代码实现

		- private void dfs(int i, int j, int value) {
        if (i == M - 1 && j == N - 1) {
            if (value > maxValue) {
                maxValue = value;
                return;
            }
        }else if(i == M-1){
            dfs(i,j+1,value + grid[i][j+1]);
        }else if(j == N-1){
            dfs(i+1,j,value+grid[i+1][j]);
        }else{
            dfs(i,j+1,value + grid[i][j+1]);
            dfs(i+1,j,value+grid[i+1][j]);
        }
    }

	- 评估

		- 时间复杂度O( 2^(M+N))

## 48.最长不含重复字符的子字符串

### 题目

- 请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。
- 输入: "abcabcbb"
- 输出: 3 
- 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

### 解题思路

- 方法一：滑动窗口 + HashSet

	- 思路

		- 滑动窗口指定当前的子序列范围

			- i，左边缘
			- j，右边缘

		- 通过HashSet记录子序列中是否有该元素，判定是否存在重复字符

	- 过程

		- 1. j 遍历整个字符串，尝试加入到子序列中

			- 检查hashSet中是否有该字符
			- 无，则j++

		- 2. hashSet有重复字符，则定位重复字符的位置，将其之前的字符从set中移除，并右移 i 的位置
		- 3. 用maxLength记录最大的长度

	- 优化

		- 1. 子序列也可以通过一个队列来保存

			- 每次检查队列中是否存在该元素
			- 有，则poll，直到把重复元素移除出来

		- 2. 子序列中重复符号位置的查找

			- 以上是用遍历的放hi
			- 也可以通过HashMap直接定位，见方法二

	- 代码实现

		- public int lengthOfLongestSubstring(String s) {
        char[] values = s.toCharArray();
        if (values.length == 0) return 0;
        HashSet<Character> set = new HashSet();
        int i = 0, j = 0;
        int maxLength = 0;
        while (j < values.length) {
            if (set.contains(values[j])) {
                while (i < j) {
                    set.remove(values[i]);
                    if (values[i] == values[j]) {
                        i++;
                        break;
                    }
                    i++;
                }
            }
            set.add(values[j]);
            j++;
            maxLength = Math.max(maxLength, set.size());
        }
        return maxLength;
    }

	- 评估

		- 时间复杂度 O（N）
		- 空间复杂度O(1)

			- 因为字符的数目是有限的，最多256

- 方法二：动态规划 + HashMap

	- 思路

		- HashMap用来保存字符最近一次出现的位置索引

			- preIndex

		- 递推方程：dp[ i ] = 

			- dp [ i-1 ] + 1

				- 当字符第一次出现，即hashMap中无访问记录
				- 当字符非第一次出现，但preIndex并不在子序列区间中

					- if  i - preIndex  >= length

			- dp [ i-1 ] - preIndex

				- 当字符非第一次出现，且preIndex在子序列区间中

					- if  i - preindex < length

		- 查看递推方程，仅和 dp[ i-1 ]相关，所以可以通过一个变量替代一维数组

			- length

	- 过程

		- 1. length = 0，HashMap<Charactor, Integer>
		- 2. i = 0，开始遍历所有字符

			- 判断 hashMap中是否有该字符的key

				- 有则说明之前已经访问过，读取该字符的preIndex
				- 无，第一次访问，将value设置为 i

		- 3. 如果获取到preIndex

			- if  i - preIndex >= 0

				- 说明重复字符不在当前子序列中
				- length++

			- else

				- 重复字符在子序列中
				- length = i - preIndex
				- length 发生变更，更新maxLength

		- 4. return maxLength

	- 代码实现

		- public int lengthOfLongestSubstring(String s) {
        char [] values = s.toCharArray();
        if(values.length == 0) return 0;
        // map 记录元素最近一次出现的索引
        HashMap<Character,Integer> map = new HashMap<>();
        int i = 0, j = 0;
        int length = 0;
        int maxLength = 0;
        while (j < values.length) {
            if(map.containsKey(values[j])){
                int leftIndex = map.get(values[j]);
                if(j - leftIndex <= length){
                    // 说明重复字符位于当前子串中
                    maxLength = Math.max(maxLength, length);
                    length = j - leftIndex;
                }else{
                    length++;
                }
            }else{
                length++;
            }
            map.put(values[j],j);
            j++;
        }
        maxLength = Math.max(maxLength, length);
        return maxLength;
    }

	- 评估

		- 时间复杂度 O（N）
		- 空间复杂度O（1）

## 49.丑数

### 题目

- 我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

- 输入: n = 10
- 输出: 12
- 解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

### 解题思路

- 显然用动态规划来递推解题
- 递推方程

	- dp[ n ] 表示第 n 个丑数
	- dp[ n ] =

		- 2 * dp[ i ]

			- 当为三者中最小值时，当选
			- 选中之后，i++

		- 3 * dp[ j ]
		- 5 * dp[ k ]

- 思路过程

	- 1. 初始化dp[ 0 ] = 1，i,j,k = 0 
	- 2. 从 2*dp[i]，3*dp[j]，5*dp[k]中选择最小的
	- 3. 比如，选择了 2 * dp[i ]

		- 则 dp[n] = 2 * dp[i]
		- n++
		- i++

	- 4. 直到第N个丑数

- 代码实现

	- public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int i = 0, j = 0, k = 0;
        for (int m = 1; m < n; m++) {
            int next2=2*dp[i], next3=3*dp[j], next5=5*dp[k];
            dp[m] = Math.min(next2,Math.min(next3,next5));
            if(dp[m] == next2) i++;
            if(dp[m] == next3) j++;
            if(dp[m] == next5) k++;
        }
        return dp[n-1];
    }

- 评估

	- 时间复杂度：O(N)
	- 空间复杂度：O(N)

## 50.第一个只出现一次的字符

### 题目

- 在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
- s = "abaccdeff"
- 返回 "b"

### 解题思路

- HashMap的使用

	- 第一轮遍历：统计各个字符出现的个数
	- 第二轮遍历：遍历字符串每个字符，如果该字符频率为1，则作为第一个返回
	- 改进：HashMap<Charactor, Boolean>

		- value使用布尔，减小存储
		- 第一次出现将value设置为false
		- 第二次出现将value设置为true
		- key不存在表示未出现过
		- 最终返回第一个value为false的

- 评估

	- 时间复杂度 O(N)
	- 空间复杂度 O(1)

## 52. 找出两个链表的公共节点

### 题目

- 输入两个链表，找出它们的第一个公共节点。

### 解题思路

- 方法1：HashSet

	- 思路

		- 先遍历第一个链表，将每个链表加入到HashSet中
		- 然后遍历第二个链表，检查每个节点是否存在于HashSet中，存在则返回

	- 代码实现

		- public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> set = new HashSet<>();
        ListNode result = null;
        while (headA != null){
            set.add(headA);
            headA = headA.next;
        }
        while (headB != null){
            if(set.contains(headB)){
                result = headB;
                break;
            }
            headB=headB.next;
        }
        return result;
    }

	- 评估

		- 时间复杂度：O（N）
		- 空间复杂度：O(N)

- 方法2：双指针二次相遇

	- 思路

		- 双指针同时遍历两个链表，遍历完成后，分别切换到对方链表上再此遍历
		- 两个指针一定相遇在第一个公共节点

			- 因为两个指针路过的系欸但书相同

	- 这个算法要牢记，是查找链表公共节点的方法

		- Java中确定两个类的公共父类便是这样实现的

	- 代码实现

		- public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) return null;
        ListNode nodeA=headA, nodeB = headB;
        ListNode result = null;
        int count = 0;
        while (count < 2){
            if(nodeA == null){
                nodeA = headB;
                count++;
            }
            if(nodeB == null) nodeB = headA;
            if(nodeA == nodeB) {
                result = nodeA;
                break;
            }
            nodeA = nodeA.next;
            nodeB = nodeB.next;
        }
        return result;
    }

	- 评估

		- 时间复杂度 O(N)
		- 空间复杂度O(1)

## 53.排序数组中的查找

### 数字在排序数组中的次数

- 题目

	- 统计一个数字在排序数组中出现的次数。
	- 输入: nums = [5,7,7,8,8,10], target = 8
	- 输出: 2

- 解题思路

	- 很明显，基于二分查找改进
	- int binSearch(int [] nums, int start, int end, int target)
	- 二分判断条件

		- 3. num[middle] < target

			- return 右边查找

		- 4. num[middle] > target

			- return 左边查找

		- 5. num[middle] == target

			- return 1+ 左边查找 + 右边查找

	- 二分终止条件

		- start > end，查找区间结束

	- 代码实现

		- int binSearch(int[] nums, int start, int end, int target) {
        if (start > end) return 0;
        int middle = start + (end - start) / 2;
        if (nums[middle] > target) {
            return binSearch(nums, start, middle - 1, target);
        } else if (nums[middle] < target) {
            return binSearch(nums, middle + 1, end, target);
        } else {
            return 1 + binSearch(nums, start, middle - 1, target)
                    + binSearch(nums, middle + 1, end, target);
        }
    }

	- 评估

		- O(logN)

### 0～n-1中缺失的数字

- 题目

	- 一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

	- 输入: [0,1,3]
	- 输出: 2

- 解题思路

	- 基于二分查找解决

		- int searchMissing(int [] nums, int start, int end)

	- 二分判断条件

		- 如果number[middle] == middle

			- 说明左边不缺少元素，查找右边
			- start = middle +1

		- 如果number[middle] > middle

			- 说明左边缺少元素
			- end = middle -1

		- 如果number[middle] < middle

			- 不存在的

	- 二分终止条件

		- start > end
		- start之前的都是不缺失的，即满足number[middle] == middle
		- end < start，说明查找结束
		- start指向的即为确实元素

	- 代码实现

		- int binSearchMissingNum(int[] nums, int start, int end) {
        if (start > end) return start;
        int middle = start + (end - start) / 2;
        if (nums[middle] == middle ) {
            return binSearchMissingNum(nums, middle + 1, end);
        } else {
            return binSearchMissingNum(nums, start, middle - 1);
        }
    }

	- 评估

		- O(logN)

## 54. 二叉搜索树的第k大节点

### 题目

- 给定一棵二叉搜索树，请找出其中第k大的节点。

### 解题思路

- 二叉搜索树的中序遍历是顺序的
- 第K大元素，就是中序遍历倒序第K个
- int inOrder( Node root, int k)
- 中序遍历

	- if(root == null) return 0;
	- inOrder(root.right, k)
	- 计数+判断是否终结

		- count++
		- if(count == k) return root.val;

	- inOrder(root.left,k)

- 难点在遍历提前终止

	- 要保护返回过程中，结果不会被再此修改
	- 如何在递归过程中通过返回值传递结果

- 代码实现

	-     private int count =0;
    private int preOrder(TreeNode root, int k){
        if(root == null) return 0;
        int result = preOrder(root.right,k);
        count++;
        if(count == k) result = root.val;
        if(count >= k) return result;
        return preOrder(root.left,k);
    }

## 55. 平衡二叉树

### 求二叉树的深度

- 题目

	- 输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

- 解题思路

	- 递归，从底向上统计
	- 深度 = 左右子树深度最大值 + 1
	- 代码实现

		- public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left,right) + 1;
    }

	- 评估

		- O（logN）

### 判断是否是平衡二叉树

- 题目

	- 输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
	- boolean isBalanced（Node root）

- 解题思路

	- AVL定义：左右子树高度差 <= 1

		- 递归定义
		- 所有子树都要满足

	- 思路一：遍历 + 左右深度比较

		- 遍历每个节点，在每个节点求左右子树的深度，并比较，不满足AVL则直接返回
		- 代码实现
		- 评估 O(logN logN)

	- 思路二：从底向上判断

		- 融合求深度和遍历过程
		- 类似求深度的程序，返回值为

			- 是AVL，则返回深度
			- 不是AVL，则返回-1

		- 不是-1，则表明是AVL
		- 代码实现

			- int isBalancedNew(TreeNode root) {
        if (root == null) return 0;
        int left = isBalancedNew(root.left);
        int right = isBalancedNew(root.right);
        if(left == -1 || right == -1) return -1;
        if (Math.abs(left - right) <= 1) {
            return Math.max(left, right) + 1;
        } else {
            return -1;
        }
    }

		- 评估

			- O(logN)

## 56.数组中只出现一次的数

### 两个数出现一次，其余均出现两次

- 题目

	- 一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
	- 输入：nums = [4,1,4,6]
	- 输出：[1,6] 或 [6,1]

- 解题思路

	- 要求是O(N)和O(1)，所以无法通过hashMap统计频数
	- 通过异或，可以求出数组中只出现一次的数
	- 分组异或

		- 出现两次的数字异或之后为0，对最终的异或结果没有影响
		- 最终异或结果为两个出现一次数字的异或
		- 通过异或结果为1的位将数组分为2组

			- 两个出现一次的数字肯定在不同组
			- 相同的数字肯定出现在同一组

		- 对两个分组分别异或，最后异或结果就是两个出现一次的数

	- 思路过程

		- 1. 对数组进行异或操作，得到结果M
		- 2. M即为两个数字的异或结果，获取M最右边的1的位

			- mask = x & -x

		- 3. 基于mask位将数组分成两组，对两组分别进行异或

	- 相关位运算

		- 取最右边为1的位

			- x & -x
			- 结果一般作为mask

		- 判断对应位为0还是为1

			- num & mask == 0，则为0
			- num & mask == mask，则为1

	- 代码实现

		- public int[] singleNumbers(int[] nums) {
        int result = 0; // x ^ 0 = x
        for (int i = 0; i < nums.length; i++) {
            result ^= nums[i];
        }
        // x ^ -x：结果为右边第一个非0位，比如00001000
        int mask = result & -result;
        int first = 0,second=0;
        for (int i = 0; i < nums.length; i++) {
            if ((nums[i] & mask) == 0) {
                first ^= nums[i];
            } else {
                second ^= nums[i];
            }
        }
        return new int[]{first,second};
    }

	- 评估

		- O(N)，O(1)

### 一个数出现一次，其余均出现3次

- 题目

	- 在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。
	- 输入：nums = [3,4,3,3]
	- 输出：4

- 解题思路

	- 由于出现三次，无法通过上题异或方式消除抵消
	- 基于位统计的思路

		- 通过所有数各个位上1的个数
		- 对各个位的频数进行模三

			- 整除，则说明多余的数字该位为0
			- 不能整除，则说明多余的数，该位为1

	- 代码实现

		- 用一个 int [32] 数组统计int 各位上 1 的个数
		- public int singleNumber(int[] nums) {
        int [] bitSum = new int[32];
        for (int i = 0; i < nums.length; i++) {
            int mask = 1;
            for (int j = 31; j >=0; j--) {
                if((nums[i] & mask) != 0) bitSum[j]++;
                mask = mask << 1;
            }
        }
        int mask = 1,result = 0;
        for (int i = 31; i >=0; i--) {
            if(bitSum[i] % 3 != 0){
                result += mask;
            }
            mask = mask << 1;
        }
        return result;
    }

	- 评估

		- O(N)

## 57.数组中和为S的序列

### 题目一：和为S的两个数

- 题目

	- 输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
	- 输入：nums = [2,7,11,15], target = 9
	- 输出：[2,7] 或者 [7,2]

- 解题思路

	- 二分查找+双指针

		- 数组是有序数组，很容易想到二分
		- 首尾双指针迭代检查和是否为S

	- 思路与过程

		- 1. 二分查找确定最后一个小于S的数

			- 因为这两个数肯定比S要小
			- 使用二分查找的模板二，middle向上取整

		- 2.首位双指针迭代检查

			- i = 0; j = left；双指针从两端出发，终点为相遇
			- 判断 num[ i ] + num[ j ]

				- 等于S

					- 查找成功，结果返回

				- 大于S

					- 右边的大了，j--

				- 小于S

					- 左边小了，i++

	- 代码实现

		- public int[] twoSum(int[] nums, int target) {
        // 基于二分查找第一个小于等于
        int left = 0, right = nums.length-1;
        while (left < right){
            int middle = left + (right - left + 1)/2;
            if(nums[middle] >= target ){
                right = middle-1;
            }else{
                left = middle;
            }
        }
        int i=0, j = left;
        int [] result = new int[0];
        while (j > i){
            int sum = nums[i] + nums[j];
            if(sum == target){
                result = new int[]{nums[i],nums[j]};
                break;
            }else if (sum < target){
                i++;
            }else {
                j--;
            }
        }
        return result;
    }

	- 评估

		- O(N)

### 题目二：和为S的连续数

- 题目

	- 输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

	- 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
	- 输入：target = 9
	- 输出：[[2,3,4],[4,5]]

- 解题思路

	- 滑动窗口思路
	- 过程与思路

		- 1. i=0，j=0；初始滑动窗口范围。sum，记录滑动窗口中的和
		- 2.循环终点： j == target，则查找结束
		- 3. j 先右移扩展，将更大的数加入到滑动窗口

			- j++
			- sum += j

		- 4. 检查sum与target

			- 等于

				- 记录一个结果
				- i + 2，左边收缩两个，之后右边继续扩展

			- 小于

				- 需要扩展更大的，j++

			- 大于

				- 需要移除较小的，i++

	- 代码实现

		- public int[][] findContinuousSequence(int target) {
        ArrayList<int[]> result = new ArrayList<>();
        int i = 1, j = 1, sum = 1;
        while (j < target) {
            if (sum < target) {
                j++; // 右扩展
                sum += j;
            } else if (sum > target) {
                sum -= i;
                i++; // 左收缩
            } else {
                int[] ans = new int[j - i + 1];
                for (int k = i; k <= j; k++) {
                    ans[k - i] = k;
                }
                result.add(ans);
                sum -= i + i + 1;
                i += 2;
            }
        }
        return result.toArray(new int[result.size()][]);
    }

	- 评估

		- O(target)

## 58.旋转字符串

### 题目一：翻转单词顺序

- 题目

	- 输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

	- 输入: "the sky is blue"
	- 输出: "blue is sky the"

- 解题思路

	- 思路一：基于栈的逆序

		- 1. 从后向前扫描字符
		- 2. 空格跳过，非空格加入栈，遇到空格终止加入
		- 3. 将栈中元素全部弹出，加入到StringBuilder
		- 4. builder 插入一个空格
		- 5. 继续循环2操作

	- 思路二：基于split()方法

		- 1. 使用split将字符串切成单词数组
		- 2. 倒叙遍历数组，将单词加入到builder

	- 思路三：subString(start, end)

		- 1. 从后向前检索每个单词，i为单词首字符，j为单词尾字符索引
		- 2. j 前移， 遇到空格跳过，直到遇到非空格字符停下
		- 3. i 从 j 出发，前移，遇到非空格跳过，直到遇到空格字符
		- 4. subString(i+1, j+1)即为单词，加入buidler

### 题目二：左旋转字符串

- 题目

	- 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

- 解题思路

	- return s.substring(n,s.length()) + s.substring(0,n);


## 59.单调队列问题

### 滑动窗口的最大值

- 题目

	- 给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。
	- 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
	- 输出: [3,3,5,5,6,7] 
	-  k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。
	- public int[] maxSlidingWindow(int[] nums, int k) 

- 解题思路

	- 滑动窗口范围通过双指针来实现
	- 主要考察的是滑动窗口中如何求最大值
	- 思路一：用单调队列实现

		- 思路与过程

			- 1. 单调队列用一个双端队列实现
			- 2. 单调队列中维护最大值

				- 队列首为当前滑动窗口最大值
				- 移入窗口的元素在单调栈中参与最大值精选，从后向前，将比新元素小的全部从队尾移除
				- 最后，将新元素从队尾加入
				- 单调队列始终保持单调递减

			- 3. 移出窗口的元素如果和队列首元素相等，则移出队列首，换之后的元素最为最大

		- 代码实现

			- int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length == 0 ) return new int[0];
        int i = 0 - k + 1, j = 0;
        int[] result = new int[nums.length - k + 1];
        Deque<Integer> deque = new LinkedList<>();
        while (j < nums.length) {
            while (!deque.isEmpty() && deque.peekLast() < nums[j]){
                deque.removeLast();
            }
            deque.addLast(nums[j++]);
            if(i >= 0){
                result[i] = deque.peekFirst();
                if (nums[i] == deque.peekFirst()) {
                    deque.removeFirst();
                }
            }
            i++;
        }
        return result;
    }

		- 评估

			- 时间复杂度 O(N)
			- 空间复杂度O(K)
			- 子主题 3

	- 思路二：用优先级队列实现

		- 即用大根堆来维护滑动窗口中最大值
		- 与单调队列类似的实现
		- 时间复杂度为O(N + logK)

### 队列的最大值 

- 题目

	- 请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

	- 即，实现一个单调队列，能快速获取队列中的最大值
	- 若队列为空，pop_front 和 max_value 需要返回 -1

- 解题思路

	- 和滑动窗口中的最大值类似
	- 通过双队列来实现

		- 一个队列用来维护队列元素顺序
		- 另一个队列用来维护最大值

			- 和上一题一样的实现

	- 类似之前最小栈的实现，通过双栈实现的思路
	- 思路与过程

		- max_value方法

			- 直接返回最大值队列首元素，需要判空

		- pop_front方法

			- 移除队列首元素，同时检查是不是最大元素

		- push_back方法

			- 加入队列队尾，然后加入最大值队列队尾（移除其前边的小值）

	- 代码实现

		- void push_back(int value) {
        queue.addLast(value);
        while (!maxQueue.isEmpty() && maxQueue.peekLast() < value){
            maxQueue.removeLast();
        }
        maxQueue.addLast(value);
    }
		- int pop_front() {
        if(queue.isEmpty()) return -1;
        if(queue.peek().equals(maxQueue.peekFirst())){
            maxQueue.removeFirst();
        }
        return queue.poll();
    }

## 60.n个骰子的点数

### 题目

- 把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。
- 你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。
- 输入: 1
- 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]

### 解题思路

- 1. 看到按步骤进行，则可以画出递归树

	- 递归参数：

		- depth，树深度，即步骤
		- sum，即点数之和

	- 将参数作为状态变量，则存在重复子问题
	- 一共6^n个节点
	- 有重复子问题，且复杂度较高，必然考虑动态规划算法

- 2. 动态规划

	- dp[ i ][ j ]，第 i 次抛硬币，和为 j 的个数
	- 递推方程

		- dp[ i ][ j ] = 

			- dp[ i-1 ][ j-6] + ... + dp[ i-1][ j-1 ]
			- 即这次的次数等于上次6种和的数量之和

	- 发现 dp[ i ][ j ]之和 i-1 有关联，所以可以简化为一维数组
	- dp[ i ] = 

		- dp[ i-6 ] + ... + dp[ i-1]
		- 但需要逆序求解，否则提前覆盖前边的数值，会影响之后的计算

- 代码实现

	- double[] twoSum(int n) {
        int len = 6*n;
        int [] dp = new int[len];
        for (int i = 0; i < 6; i++) {
            dp[i] = 1;
        }
        for (int i = 2; i <= n; i++) {
            for (int j = len -1; j >= 0 ; j--) {
                dp[j] = 0;
                for (int k = 1; k <= 6; k++) {
                    if(j-k < 0) break;
                    dp[j] += dp[j-k];
                }
            }
        }
        double total = Math.pow(6,n);
        double [] res = new double[len-n+1];
        for (int i = 0; i < len-n+1; i++) {
            res[i] = dp[i+n-1] / total;
        }
        return res;
    }

- 评估

	- O(N)

## 61. 扑克牌中的顺子

### 题目

- 从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

- 输入: [1,2,3,4,5]
- 输出: True

### 解题思路

- 思路

	- 1. 肯定是先排序，排序之后检查更简单
	- 2. 解决两大问题

		- 不能有非0的重复数字

			- 方法一：可以用Set来判断是否有重复
			- 方法二：重复元素也会导致不满足前后差为1

		- 5个数，通过0填补之后，必须是连续的

			- 方法一：遍历，如果最终无法到达最后一个数，则不是
			- 方法二：如果是连续的，则最大值-最小值应该< 
 5 (通过set排除重复元素)

- 实现一：遍历检查

	- 过程

		- 1. 先对5个数进行排序
		- 2. 从头检索0的个数，作为替代工具
		- 3. curr取第一个非0的数，索引index，判断其和后 index+1 位置的元素是否比它大1

			- 是，则index++，curr++，继续判断

				- 终止条件，index + 1 < length

			- 不是，则判断小王工具是否为0

				- 为0，则失败，退出
				- 不为0，则工具数量-1，curr++，index不变

	- 代码实现

		- boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int joker = 0,index=0;
        while (index < nums.length && nums[index] == 0){
            joker++;
            index++;
        }
        if(index >= nums.length) return true;
        int curr = nums[index];
        while (index+1 < nums.length){
            if(nums[index+1] - curr == 1){
                curr = nums[++index];
            }else{
                if(joker==0) break;
                else {
                    joker--;
                    curr++;
                }
            }
        }
        return index == nums.length-1;
    }

- 实现二：通过Max-Min判断是否连续

	- z
	- 思路

		- 1. 遍历5个元素

			- 加入set，检查是否重复，重复则直接false退出
			- 统计最大值和最小值

		- 2. 如果不存在重复元素，那么如果是连续的，一定存在 max -min < 5

	- 代码实现

		- boolean isStraight(int[] nums) {
        int max = 0, min = 13;
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < nums.length; i++) {
            if(nums[i] == 0){
                continue;
            }else{
                if(!set.contains(nums[i])){
                    set.add(nums[i]);
                    max = Math.max(max,nums[i]);
                    min = Math.min(min,nums[i]);
                }else{
                    // 存在重复
                    return false;
                }
            }
        }
        return max -min < 5;
    }

## 62. 圆圈中最后剩下的数字

### 题目

- 0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
- 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
- 输入: n = 5, m = 3
- 输出: 3

### 著名的 约瑟夫环 问题

### 解题思路

- 思路一：队列模拟(超时)

	- 1. 初始将所有的数字加入队列中
	- 2. 从队首取出元素，然后再从队尾加入
	- 3. 每取m-1个，到第m个时，直接抛弃，不再入队尾
	- 4. 如果 m-1 > queue.size()，取模 
	- 代码实现

		- int lastRemaining(int n, int m) {
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            queue.add(i);
        }

        while (queue.size() > 1){
            for (int i = 0; i < (m-1)%queue.size(); i++) {
                queue.add(queue.poll());
            }
            queue.poll();
        }
        return queue.poll();
    }

	- 评估

		- 时间复杂度O(n^2)
		- 空间复杂度O(N)

- 思路二：约瑟夫环，动态规划

	- 以队列模拟为基础，每次被选中的元素必然都是位于队首的
	- 即，最终选中元素的下标为0，可以向上一次移除倒推
	- 定义dp[ i ]，剩余 i 个数字，最终元素的坐标
	- dp [ i ] = (dp[ i-1 ] + m) % n

		- 将移除的那个元素复原
		- 如何复原，将队首m-1个以及移除的取回，所以，所有元素要向后位移 m个
		- 可能溢出，所以取模运算

	- 代码实现

		- int lastRemaining(int n, int m) {
        int ans = 0;
        for (int i = 2; i < n; i++) {
            ans = (ans + m) % i;
        }
        return ans;
    }

	- 评估

		- 时间复杂度O(N)

## 63.股票的最大利润

### 题目

- 假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？
- 输入: [7,1,5,3,6,4]
- 输出: 5
- 0 <= 数组长度 <= 10^5

### 解题思路

- 思路一：暴力回溯(超时)

	- 计算以 i 买入，j 卖出的所有组合的利润，求最大值
	- 时间复杂度，O(N^2)

- 思路二：动态规划

	- 区间中的最大利润：等于区间中的最高价格 - 最低价格
	- dp[ i ] 定义为，截至到第[ i ]天的最大利润
	- dp[ i ] = max{ dp[ i-1 ] , price[ i ] - minValue }

		- 前i-1天的最大利润
		- 最大利润在今天出现，即price[ i ] - minPrice
		- minPrice 表示前 i 的最低价格

	- 代码实现

		- int maxProfit(int[] prices) {
        if(prices.length == 0) return 0;
        int maxProfit = 0;
        int minPrice = prices[0];
        int [] dp = new int[prices.length];
        for (int i = 1; i < prices.length; i++) {
            dp[i] = Math.max(dp[i-1], prices[i]-minPrice);
            maxProfit = Math.max(dp[i],maxProfit);
            minPrice = Math.min(prices[i],minPrice);
        }
        return maxProfit;
    }

	- 评估

		- 时间复杂度O(N)
		- 空间复杂度O(N)，可以做大O(1)

## 64.不用乘除法、条件判断语句和循环计算1+2+..+n

### 题目

- 求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

### 这种题，一定要从位运算、递归、二进制运算等角度去考虑

### 解题思路

- 不用乘除，那只能叠加了
- 不用循环，那必然要使用递归来计算了
- 不能用条件判断语句

	- 使用逻辑语句的短路作用

- 代码实现

	-  public int sumNums(int n) {
        boolean flag = n >0 && (n+=sumNums(n-1)) == 0 ;
        return n;
    }

## 65.不用加减乘除做加法

### 题目

- 写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
- a, b 均可能是负数或 0
- 输入: a = 1, b = 1
- 输出: 2

### 解题思路

- 条件反射的思路

	- 不能用加减乘除，那只能用位运算，从二进制角度实现加法

- 计算机使用补码表示，符号位直接参与计算，所以这里不需要区分正数和负数
- 对于 a+b

	- xsum = a^b

		- 异或运算，得到无进位加法的和

	- full = a&b

		- 两数想并，得到各位的进位情况

	- full = full>>1

		- 进位右移，给下一位

	- sum = xsum + full

		- 最终进位和无进位和相加，即是最后的和
		- 又是一个加法，继续以上操作，知道进位为0，加法结束

- 代码实现

	- int add(int a, int b) {
        int xadd = a^b;
        int full = (a&b)<<1;
        while (full != 0){
            int sum = xadd^full;
            full = (xadd&full) << 1;
            xadd = sum;
        }
        return xadd;
    }

## 66.构建乘积数组

### 题目

- 给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B 中的元素 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

- 输入: [1,2,3,4,5]
- 输出: [120,60,40,30,24]
- a.length <= 100000

### 解题思路

- 思路一：暴力法

	- 两层循环，O(n(n-1))
	- 10^5数量级，肯定超时

- 思路二：动态规划

	- 思路一中有很多的重复计算
	- dpLeft[ i ] ，从左起，累乘到 i  的结果
	- dpRight[ i ]，从右起，累乘到 i 的结果
	- result [ i ] = dpLeft[ i-1 ] * dpRight[ len -1 -i ]
	- 代码实现

		- public int[] constructArr(int[] a) {
        if(a.length == 0) return new int[0];
        int [] left = new int[a.length];
        int [] right = new int[a.length];
        left[0] = a[0];
        right[a.length-1] = a[a.length-1];
        for (int i = 1; i < a.length; i++) {
            left[i] = left[i-1]* a[i];
            right[a.length-1-i] = right[a.length-i] * a[a.length-1-i];
        }
        int [] result = new int[a.length];
        result[0] = right[1];
        result[result.length-1] = left[a.length-2];
        for (int i = 1; i < result.length-1; i++) {
            result[i] = left[i-1]*right[i+1];
        }
        return result;
    }

	- 评估

		- 时间复杂度O(N)
		- 空间复杂度O(2N)

## 67.把字符串转换成整数

### 题目

- 写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。
- 首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
- 当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
- 该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
- 注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
- 在任何情况下，若函数不能进行有效的转换时，请返回 0
- 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

### 解题思路

- 就是一个很常规的字符串处理题
- 过程

	- 1. 处理空格

		- 循环读取空格

	- 2. 处理首个非空格字符，识别正负

		- 数字，开始算数值
		- 正负，记录到flag
		- 其他字符，返回0

	- 3. 处理连续的数字，并计算值

		- res = res * 10 +  char(i) - ‘0’

	- 4. 处理大数溢出问题

		- 用long来处理

- 代码实现

	- 太长，见leetcode

## 68. 二叉树中两个节点的公共祖先

### 对于二叉搜索树

#### 题目
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

#### 解题思路
- BST中公共祖先的特征
  
  - 大于等于一个节点
  
  - 小于等于另一个节点
	
  - 且，再向下搜索就不符合了

- 根据公共祖先节点的特征，在BST中查找即可
- 检索过程
	
  1. 从根节点开始搜索BST
	
  2. 如果两个节点都比 当前节点小，则搜索左子树，否则右子树
	
  3. 当出现满足公共节点特征的时候，检索结束，返回该节点即可

#### 代码实现
- 迭代实现,因为只有深入过程，没有回溯过程，可以用迭代实现

```java
TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
  // 检索到一个分界点，p和q不在同一个分支上即可
  TreeNode curr = root;
  while (curr != null){
    if(p.val < curr.val && q.val < curr.val){
    curr = curr.left;
    }else if(p.val > curr.val && q.val > curr.val){
    curr = curr.right;
    }else{
      break;
    }
  }
  return curr;
}	
```

### 对于普通二叉树

- 题目

	- 和上题一样，只是普通的二叉树

- 解题思路

	- 思路0：一下就想到了查找两个链表公共节点的问题，但发现树没有子节点向上的指针，遂放弃
	- 思路1：队列记录两个节点的路径

		- DFS两次分别检索p和q节点，将最终查找成功的路径保存在两个队列中。

			- 然后从后向前遍历队列，最后一个一样的节点，就是公共祖先

		- 难点在于遍历过程中路径队列中元素的进出

			- 回溯
			- bfs返回值为是否检索成功
			- 为false时，需要将元素从中移除

		- 代码实现

			- public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        Deque<TreeNode> pDeque = new LinkedList<>();
        Deque<TreeNode> qDeque = new LinkedList<>();
        bfs(root,p,pDeque);
        bfs(root,q,qDeque);
        TreeNode node = null;
        while (!pDeque.isEmpty() && !qDeque.isEmpty()){
            TreeNode curr = pDeque.pollLast();
            if(curr == qDeque.pollLast()){
                node = curr;
            }else{
                break;
            }
        }
        return node;
    }

    private boolean bfs(TreeNode root, TreeNode p, Deque<TreeNode> deque){
        if(root == null) return false;
        deque.addFirst(root);
        if(root == p) return true;
        boolean success = bfs(root.left,p,deque);
        if(success) return true;
        if(root.left != null) deque.removeFirst();
        success = bfs(root.right,p,deque);
        if(!success&& root.right != null){
            deque.removeFirst();
        }
        return success;
    }

	- 思路二：DFS检索特征节点

		- 无法再通过公共祖先的特征搜索BST了
		- 那就只能采用DFS等遍历算法来查找了
		- 公共祖先的特征

			- 1. 一个节点在左子树，一个节点在右子树
			- 2. 一个节点在左子树 / 右子树，另一个就是祖先节点
			- 对应的不是公共祖先的特征

				- 1. 两个节点都在一个子树
				- 2. 左右子树每一个节点

		- 思路过程

			- 1. 执行DFS遍历每个节点

				- DFS返回值为检索到的节点

			- 2. 后序遍历，根据左右子树的检索结果，判断是否是祖先节点

				- 0. 检索到了 p和q节点，返回p和q，用非空标识检索到了
				- 1. left 和 right 都是null，返回null
				- 2. left 和 right 都不是null，则符合分布在左右子树，返回当前节点
				- 3. left 和 right 有一个非空，返回非空节点

		- 代码实现

			- TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return null;
        if(root == p || root == q) return root; // 自己是自己的祖先
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
         if(left == null && right == null){ // 叶子节点了
            return null;
        }else if(left != null && right != null){ // 分布在左右，ok，它是第一个祖先
            return root;
        }else if(left!= null){
            return left;
        }else{
            return right;
        }
    }

