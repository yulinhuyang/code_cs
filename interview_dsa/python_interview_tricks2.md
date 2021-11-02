## 1 链表（堆 栈）

### 1.1  base code

链表基本结构：

```python
class list_node:
    def __init__(self,x):
        self.data = x
        self.next = None
    
    #迭代
    def traverse(self,head):
        p = head
        while p:
            p = p.next
    #前序、后序
    def traverse(self,head):
        #前序print head.val
        self.traverse(head.next)
        #后序print head.val	

```

链表基本操作：

```
class node:
    def __init__(self,x):
        self.data = x
        self.next = None

class MyLinkedList:
    def __init__(self):
        self.head = node(0)
        self.size = 0
    
    def get(self,index:int) -> int:
        
        if index < 0 or index >= self.size:
            return -1
        head_node = self.head
        for _ in range(index+1):
            head_node = head_node.next
        return head_node.data
    
    def addAtHead(self,val:int) -> None:
        self.addAtIndex(0,val)
        
    def addAtTail(self,val:int) -> None:
        self.addAtIndex(self.size,val)

    def addAtIndex(self, index: int, val: int) -> None:
        
        if index > self.size:
            return
        
        if index < 0:
            index = 0
        
        head_node = self.head
        for _ in range(index):
            head_node = head_node.next
        new_node = node(val)
        new_node.next = head_node.next
        head_node.next = new_node
        self.size = self.size + 1

    def deleteAtIndex(self, index: int) -> None:
        
        if 0 <= index < self.size:
            head_node = self.head
            for i in range(index):
                head_node = head_node.next
            head_node.next = head_node.next.next
            self.size = self.size -1
	    
	    
// 1 有sentinel==0的哨兵节点,然后index 从0 开始

//2 所有的插入和删除都是针对index=0 开始的，哨兵节点不动

//3 get的时候，需要range index +1 ,add的时候，range index

//4 主要边界条件，delete < size。
	
//5  头插，尾插，初始化，增加删除查询index
	   	   
```


单调栈：下一个最大元素、包含min函数的栈

```python

def nextGreatElement(nums):

ans = [0 for i in range(len(nums))]
stack = []

for i in range(len(nums) - 1,0,-1):
    while stack and stack[-1] <= nums[i]:
        stack.pop(-1)

    if not stack:
        ans[i] = -1
    else:
        ans[i] = stack[-1]

    stack.append(nums[i])
	
return ans
```


单调队列：滑动窗口的最大值

 
```python

import collections

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums and k==0:
            return []
        deque = collections.deque()
        res = []
        
        for i in range(k):
            while deque and deque[-1] < nums[i]:
                deque.pop()
            deque.append(nums[i])

        res = [deque[0]]
        for i in range(k,len(nums)):
            if deque[0] == nums[i-k]:
                deque.popleft()
            while deque and deque[-1] < nums[i]:
                deque.pop()

            deque.append(nums[i])
            res.append(deque[0])
        
        return res

```

## 2 树

树天生为了递归左右子树而存在

树的三种遍历

平衡二叉树:

	空树是平衡二叉树；非空的树，所有子树都满足左子树和右子树高度差不超过1。
	
二叉查找树（Binary Search Tree）：（二叉搜索树，二叉排序树）
		
	空树是二叉查找树;左子树上所有结点的值均小于它的根结点的值,右子树上所有结点的值均大于它的根结点的值。用min,max向下比较。
	
	搜索二叉树按照中序遍历得到的序列一定是从小到大排列的.
	
	红黑树.平衡搜索二叉树(AVL)树,都是搜索二叉树的不同实现
	
完全二叉树： 	
	
	除了最后一层之外,其他每一层的节点数都是满的,如果最后一层满了就是满二叉树。
	
	如果最后一层不满,缺少的节点也全部集中在右边。

### 2.1  base code

```python

class tree_node:
    def __init__(self,x):
        self.data = x
        self.left = None
        self.right = None
      
class tree_node:
    def __init__(self,x):
        self.data = x
        self.left = None
        self.right = None
    #二叉树    
    def traverse(self,root):
        self.traverse(root.left)
        self.traverse(root.right)
    #遍历多叉树
    def traverse(self,root):
        for chid in root.children:
            self.traverse(chid)

#验证二叉搜索树

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        return self.isValidBSTTree(root,None,None)

    def isValidBSTTree(self,root:TreeNode,min,max)->bool:
        if not root:
            return True
        if min and root.val <= min.val: return False
        if max and root.val >= max.val: return False
        return self.isValidBSTTree(root.left,min,root) and self.isValidBSTTree(root.right,root,max)

#验证平衡二叉树

自顶而下： o(n*n）

class Solution:

    def depth(self,root:TreeNode) -> int:
        if not root:
            return 0 
        return max(self.depth(root.left),self.depth(root.right)) + 1
            
    def isBalanced(self, root: TreeNode) -> bool:
        if not root:
            return True
        
        if abs(self.depth(root.left) - self.depth(root.right)) > 1:
            return False
        
        return self.isBalanced(root.left) and self.isBalanced(root.right)

```

## 3 动态规划

### 3.1  base code 

重叠子问题、状态转移方程、最优子结构

套路: 明确 状态 --->明确 选择 ---> 明确dp函数/数组的定义---->明确base case 

递归(自顶向下)：暴力递归---> memo 解法

动归(自底而上)：dp 数组 ---> 状态压缩二维变一维---> pre cur for循环解法 


#### 递归 + memo：

	自顶向下的递归：f(20)————>f(19)————>.....f(0)
	
	递归+状态变化

        递归问题一般结构：判截止---> 查memo --->做选择 --->置memo

memo模板:
 
```python
    memo = dict()
    def dp(i,j):

        #判断截止条件
        if i == m or j == n:
            return 

        #查memo
        if (i,j) in memo:
            return memo[i,j]
        
        #状态选择递归
        dp: dp(i,j) --->dp(i+1,j+1)或者其他,计算ret

        #置位memo
        memo[i,j] = ret
        
        return ret

```

```python

def helper(memo,n):
    if n == 1 or n == 2:
        return 1
    
    if memo[n] != 0:
        return memo[n]
        
    memo[n] = helper(memo,n-1) + helper(memo,n-2)
    return memo[n]


def fib(n):
    memo = [0 for i in range(n+1)]
    return helper(memo,n)
	
```
	
#### 动态规划:
	
	自底向上的迭代：f(0)--->f(1)————>....f（20）

	dp数组里面存的内容和memo是完全一样的，只不过把递归改成了for循环迭代了而已。
        

##### dp数组模板：

[告别动态规划，连刷 40 道题](https://zhuanlan.zhihu.com/p/91582909)

最优子结构 + 重叠子问题

明确 状态和选择--->base case ---> 明确dp数组含义(m,n)或(m+1,n+1)，状态转移--->返回dp值

```python

	#初始化base case，填充dp[0][0]、0行、0列
	dp[0][0][...] = base

	#进行状态转移，遍历[1:m][1:n](或[1:m+1][1:n+1])填充dp[i][j] 
	
	for 状态1  in 状态1的所有取值：
		for 状态2 in 状态2的所有取值：
			for ..
				dp[状态1][状态2][..] = 求最值（选择1，选择2...）
	
	#返回dp[m-1][n-1](或dp[m][n])	
```

##### dp的数组的遍历方向：

	1、遍历的过程中，所需的状态必须是已经计算出来的。

	2、遍历的终点必须是存储结果的那个位置。
	
	3. 常见的遍历方向：正向、反向、斜着

正向：	

```python
int[][] dp = new int[m][n];
for (int i = 0; i < m; i++)
	for (int j = 0; j < n; j++)
		// 计算 dp[i][j]
```

反向:

```python 
for (int i = m - 1; i >= 0; i--)
	for (int j = n - 1; j >= 0; j--)
		// 计算 dp[i][j]
```

斜向：

```python
// 斜着遍历数组
for (int l = 2; l <= n; l++) {
	for (int i = 0; i <= n - l; i++) {
		int j = l + i - 1;
		// 计算 dp[i][j]
	}
}
```

大部分的dp都是二维dp，用dp[m][n]还是dp[m+1][n+1]取决于含义，dp[0][0]是否有意义,是否需要定义

##### 常见情况：
	
	序列问题：dp[i] 表示以 nums[i] 这个数结尾的最长递增子序列的长度

	路径轨迹问题：dp[m][n] ---> i,j含义是位置，定义dp[m][n],结果是dp[m-1][n-1]

	字符串(两个)问题(编辑距离)： dp[m+1][n+1] --> i,j含义是长度,定义dp[m+1][n+1],结果是dp[m][n]，可递归解

```python
	int[][] dp[m+1][n+1]
	dp[0][..] = 0
	dp[..][0] = 0
	for i in [1..m]:
		for j in [1..n]:
	
```		


	背包问题： dp[i][w] 的定义如下：对于前 i 个物品，当前背包的容量为 w，这种情况下可以装的最大价值是 dp[i][w]

```python 
	int[][] dp[N+1][W+1]
	dp[0][..] = 0
	dp[..][0] = 0

	for i in [1..N]:
	for w in [1..W]:
		dp[i][w] = max(
			把物品 i 装进背包,dp[i-1][w - wt[i-1]] + val[i-1]
			不把物品 i 装进背包，dp[i-1][w],
		)
	return dp[N][W]

```

        2.1 涉及两个字符串/数组时（比如最长公共子序列），dp 数组的含义如下：

        在子数组 arr1[0..i] 和子数组 arr2[0..j] 中，我们要求的子序列（最长公共子序列）长度为 dp[i][j]。
        
        2.2 只涉及一个字符串/数组时（比如本文要讲的最长回文子序列），dp 数组的含义如下：
        
        在子数组 array[i..j] 中，我们要求的子序列（最长回文子序列）的长度为 dp[i][j]。

```python

def fib(n):
    if n == 0: return 0
    if n == 1 or n == 2: return 1
    dp = [0 for i in range(n+1)]

    dp[1] = 1
    dp[2] = 1
    for i in range(3,n+1):
        dp[i] = dp[i-1] + dp[i-2] 
    
    return dp[n]
	
```


## 4 回溯（DFS）

### 4.1 base code 

	递归+选择

	结束条件+ 做选择和撤销选择 + 决策树

二叉选择、多叉选择

```python

	result = []
	
	def backtrack(路径、选择列表)：
		if 满足结束条件：
			result.add(路径)
			return 
		
		for 选择 in 选择列表:
			排除不合法的选择
		
			做选择
			backtrack(路径，选择列表)
			撤销选择
	
	

```

## 5  BFS

### 5.1 base code

```python

def BFS(Node start,Node target):
    queue = []
    visited = []

    queue.append(start)
    visited.append(start)

    while not queue:
        sz = len(queue)
        for i in range(sz):
            cur = queue.pop(0)
            
            for x cur.adj:
                if x not in visited:
                    queue.append(x)
                    visited.append(x)
                    
        step += 1
```

两个循环（while + for） + 一个遍历（adj点）

## 6 双指针

### 6.1 base code 

	快慢指针(fast slow)： 也包括前后指针(pre cur、i j)  ->pre cur next。归并找中点、链表成环。

	左右指针(left right)：左右相向，反转数组，二分搜索。

	滑动窗口：子串问题
	
链表有环判断

```python 

class list_node:
    def __init__(self,x):
        self.data = x
        self.next = None

def has_cycle(head:list_node)->bool:
    fast = slow = head

    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next

        if fast == slow:
            return True

    return False

```	
注意是fast!=null 还是fast.next!=null



前后指针: 快速排序 随机法：

```python

class Solution:

	def random_partition(self,nums,l,r)-> None:
            pivot_index = random.randint(l,r)
            nums[l],nums[pivot_index] = nums[pivot_index],nums[l]
            i = l
            j = r
            pivot = nums[l]
            while i < j:
                while nums[j] >= pivot and i < j:
                    j -= 1
                while nums[i] <= pivot and i < j:
                    i += 1
                nums[i],nums[j] = nums[j],nums[i]
            nums[i],nums[l] = nums[l],nums[i]
    
	def random_sort(self,nums,l,r)-> None:
		if l >= r:
		    return
		index = self.random_partition(nums,l,r)
		self.random_sort(nums,l,index - 1)
		self.random_sort(nums,index + 1,r)

	def sortArray(self, nums: List[int]) -> List[int]:
		self.random_sort(nums,0,len(nums)-1)
		return nums
```
	
random quicksort ---> random partition ---> random quicksort 左右

random partition:  pivot选择l的时候，快排为什么j先走：https://blog.csdn.net/lkp1603645756/article/details/85008715

交换两个数： a,b = b,a



## 7 二分搜索

### 7.1 base code 

```python
		
def search(self, nums: List[int], target: int) -> int:

    left = 0
    right = len(nums)-1
    
    while left <= right:
        mid = left +(right - left)//2
        if nums[mid] < target:
    	    left = mid + 1
        elif nums[mid] > target:
    	    right = mid - 1
        else:
    	    return mid
    return -1

```	
	

不要出现 else，把所有情况用 else if 写清楚

防止溢出:left + (right - left) / 2

对比终止条件：

	while取 <=，因为都两侧都闭的区域

	while(left < right) 的终止条件是 left == right，会漏掉=节点

理解区间：
		
		[left,right]      left(0) <= right(num-1)
		
		[left,right)      left(0) < right(num)

```python

左侧边界搜索：
	
		} else if (nums[mid] == target) {
			// 收缩右侧边界
		    right = mid - 1;
					
		// 检查出界情况
		if (left >= nums.length || nums[left] != target)
		    return -1;
		return left;

右侧边界搜索：

		} else if (nums[mid] == target) {
		// 这里改成收缩左侧边界即可
		    left = mid + 1;

		//检查出界情况
		// 这里改为检查 right 越界的情况，见下图
		if (right < 0 || nums[right] != target)
		    return -1;
		return right;
```			


## 8 滑动窗口

### 8.1 base code 

```python


import sys

def sliding_window(s:str,t:str):

    need = dict()
    window = dict()
    #迭代字符串的第二种方式
    for a in t:
        if a in need:
            need[a] += 1
        else:
            need[a] = 1
            window[a] = 0 

    s_list = list(s)
    left = 0 
    right = 0 
    valid = 0
    start = 0 
    start_len = sys.maxsize
    while right < len(s):
        c = s_list[right]
        right +=1
        if c in need:
            window[c]+= 1
            if window[c] == need[c]:
                valid += 1
    
        while valid == len(need):
            if right - left < start_len:
                start = left
                start_len = right - left

            d = s_list[left]
            left += 1
            if d in need:
                if window[d] == need[d]:
                    valid -= 1
                window[d] -= 1
    
    return " " if start_len == sys.maxsize else s[start:start + start_len]
    
```

right进，再左缩，两个while

python 三目运算符 max = a if a>b else b

map必记录的api： keys、values、get、setdefault、pop、update、in

hash表（用list 或者 dict()）: 用true 或者false表示是否出现过(a-z)；count计算出现的数量（32位宽）；记录上次出现的索引位置（滑窗）。

## 9 其他高频

### 9.1 base code

**位运算** 

位运算相关操作


& 按位与

| 按位或

^ 位异或

~ 按位取发

<< 左移

>> 右移


//整除   math.pow() 求幂


**贪心算法**

贪心差分算法

贪心算法是动态规划的特例

##### 122. 买卖股票的最佳时机 II

贪心算法只能用于计算最大利润，计算的过程并不是实际的交易过程

```python

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = [0]*len(prices)
        sum_profit = 0
        for i in range(1,len(prices)):
            sum_profit += max(0,prices[i] - prices[i-1])
        
        return sum_profit
		
```

**前缀和数组**

##### 560. 和为K的子数组

```python

class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:

        map = dict()
        sum_i = 0
        ans  = 0
        map[0] = 1
        for i in range(len(nums)):
            sum_i += nums[i]

            sum_j = sum_i - k
            if sum_j in map:
                ans += map[sum_j]
            
            map[sum_i] = map.get(sum_i,0) +1
        return ans
```

前缀和 + hash法

**HASH算法** 



**并查集**

合并集合、查找集合中的元素

合并：把两个不相交的集合按照某种条件合并为一个集合。 

查询：查询两个元素是否在同一个集合中
