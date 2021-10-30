## 1  栈 队列 堆

#### 单调栈

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


反向迭代单调栈

#### LRU 与LFU 数据结构

##### LRU 

功能类似 LinkedHashMap，hash链表结构(双向链表（手动实现） + 哈希（dict）)

DlinkNode:伪头部和伪尾部，标记界限，在添加节点和删除节点的时候就不需要检查相邻的节点是否存在

dict: key --> value ,存储value的引用

class 数组：list.append(data(dic)) 

```python

class DLinkedNode:
    def __init__(self,key = 0,value = 0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = dict()
        self.head = DLinkedNode(0)
        self.tail = DLinkedNode(0)
        self.head.next = self.tail
        self.tail.prev = self.head
        self.capacity = capacity
        self.size = 0 

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.moveToHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            node = DLinkedNode(key,value)
            self.cache[key] = node
            self.addToHead(node)
            self.size += 1
            if self.size > self.capacity:
                # del node  before tail
                del_node = self.tail.prev
                del_node.prev.next = self.tail
                self.tail.prev = del_node.prev
                self.cache.pop(del_node.key)
                self.size -= 1

        else:
            node = self.cache[key]
            node.value = value
            self.moveToHead(node)
            
    
    def removeNode(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def addToHead(self, node):
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def moveToHead(self, node):
        self.removeNode(node)
        self.addToHead(node)

```

##### 206  反转链表1

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head:
            return None
        if not head.next:
            return head
        last = self.reverseList(head.next)
        #理解引用，这里恢复head.next指none的设置
        head.next.next = head
        head.next = None
        return last
```		

##### 92  反转链表2 

```python
class Solution:
    def __init__(self):
        self.succesor = None
    def reverseN(self,head,N):
        if N == 1:
            self.succesor = head.next
            return head
        last = self.reverseN(head.next,N - 1)
        head.next.next = head
        head.next = self.succesor
        return last

    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode:
        if left == 1:
            return self.reverseN(head,right)
        
        head.next = self.reverseBetween(head.next,left - 1,right - 1)
        return head
```

## 2 树

####  二叉搜索树（Binary Search Tree，简称 BST) 

root.left < root < root.right


#### 相同的树：100.相同的树

```python

	class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        if not p and q:
            return False
        if p and not q:
            return False
        if p.val != q.val:
            return False
        
        return self.isSameTree(p.left,q.left) and self.isSameTree(p.right,q.right)
```

##### 合法性: 98.验证二叉搜索树

辅助函数，增加函数参数列表，在参数中携带额外信息 min、max 节点传递并更新

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        return self.isValidBSTTree(root,None,None)

    def isValidBSTTree(self,root:TreeNode,min,max)->bool:
        if not root:
            return True
        if min and root.val <= min.val: return False
        if max and root.val >= max.val: return False
        return self.isValidBSTTree(root.left,min,root) and self.isValidBSTTree(root.right,root,max)
```

##### 查找：700.二叉搜索树中的搜索


```python

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        return self.isValidBSTTree(root,None,None)

    def isValidBSTTree(self,root:TreeNode,min,max)->bool:
        if not root:
            return True
        if min and root.val <= min.val: return False
        if max and root.val >= max.val: return False
        return self.isValidBSTTree(root.left,min,root) and self.isValidBSTTree(root.right,root,max)
```

##### 插入：701.二叉搜索树中的插入操作

```python
class Solution:
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return TreeNode(val)
        if root.val < val:
            root.right = self.insertIntoBST(root.right,val)
        if root.val > val:
            root.left = self.insertIntoBST(root.left,val)

        return root
```

##### 删除：450.删除二叉搜索树中的节点

删除节点的时候使用右子树的最左值进行替换

```python
class Solution:
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        if not root:
            return None
        if root.val == key:
            if not root.left:
                return root.right
            if not root.right:
                return root.left
            min_val = self.getmin(root.right)
            root.val = min_val
            root.right = self.deleteNode(root.right,min_val)
        elif root.val < key:
            root.right = self.deleteNode(root.right,key)
        elif root.val > key:
            root.left = self.deleteNode(root.left,key)
        return root
    def getmin(self,root:TreeNode)->int:
        while root.left:
            root = root.left
        return root.val
```


## 3 动态规划


#### 300 最长递增子序列

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1] * (len(nums))
        max_len = 0 
        for i in range(0,len(nums)):
            for j in range(0,i):
                if nums[i]> nums[j]:
                    dp[i] = max(dp[i],dp[j] + 1)

        for i in range(0,len(nums)):
            max_len = max(max_len,dp[i])
        return max_len
```

#### 354. 俄罗斯套娃信封问题

	
```python

class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:

        envelopes.sort(key=(lambda x:(x[0],-x[1])))

        height = [0]*len(envelopes)
        for i in range(len(envelopes)):
            height[i] = envelopes[i][1]
        
        #lis
        dp = [1]*len(height)
        for i in range(len(height)):
            for j in range(i):
                if height[i] > height[j]:
                    dp[i] = max(dp[i],dp[j] + 1)
        
        max_len = 0
        for i in range(len(height)):
            max_len = max(max_len,dp[i])

        return max_len 
```
#### 1143. 最长公共子序列

```python

class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m = len(text1)
        n = len(text2)
        dp = [[0 for i in range(n + 1)] for j in range(m + 1)]
        dp[0][0] = 0 
        for i in range(1,m+1):
            for j in range(1,n+1):
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])

        return dp[m][n] 

```

#### 516.最长回文子序列

子序列问题本身就相对子串、子数组更困难一些，因为前者是不连续的序列，而后两者是连续的

子序列问题的两个模板

模板1

```python

int n = array.length;
int[] dp = new int[n];

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        dp[i] = 最值(dp[i], dp[j] + ...)
    }
}
```
模板2

```python

int n = arr.length;
int[][] dp = new dp[n][n];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (arr[i] == arr[j]) 
            dp[i][j] = dp[i][j] + ...
        else
            dp[i][j] = 最值(...)
    }
}

```

2.1 涉及两个字符串/数组时（比如最长公共子序列），dp 数组的含义如下：

在子数组 arr1[0..i] 和子数组 arr2[0..j] 中，我们要求的子序列（最长公共子序列）长度为 dp[i][j]。

2.2 只涉及一个字符串/数组时（比如本文要讲的最长回文子序列），dp 数组的含义如下：

在子数组 array[i..j] 中，我们要求的子序列（最长回文子序列）的长度为 dp[i][j]。

在子串 s[i..j] 中，最长回文子序列的长度为 dp[i][j]

```python

class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:

        n = len(s)
        dp = [[0]*n for i in range(n)]
        for i in range(n):
            dp[i][i] = 1
        
		#纵向遍历
        for i in range(n-1,-1,-1):
            for j in range(i+1,n,1):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j],dp[i][j-1])

        return dp[0][n-1] 
    
```

#### 72. 编辑距离

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)
		
		#[[0]*3]*3  二维数组的声明方法
        dp = [[0 for i in range(n+1)] for i in range(m+1)]
        for i in range(1,m + 1):
            dp[i][0] = i
        for j in range(1,n + 1):
            dp[0][j] = j
        
        for i in range(1,m+1):
            for j in range(1,n+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1],min(dp[i-1][j],dp[i][j-1])) + 1
        
        return dp[m][n]
```

注意字符串动态规划问题的一般解法



#### 887 高楼扔鸡蛋

dp[k][m] = n

当前有 k 个鸡蛋，可以尝试扔 m 次鸡蛋
这个状态下，最坏情况下最多能确切测试一栋 n 层的楼

dp[k][m] = dp[k][m - 1] + dp[k - 1][m - 1] + 1


```python

def superEggDrop(self, k: int, n: int) -> int:
	
	dp = [ [0] * (n + 1)  for  i in range(k+1)]
	
	i = 0
	m = 0 
	while dp[i][m] < n:
		m += 1
		for i in range(1,k+1):
			dp[i][m] = dp[i-1][m-1] + dp[i][m-1] + 1

	return  m 
```



####  股票买卖问题

dp定义的含义：

选择 buy 的时候，把 k 减小了 1

```python

dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max(   选择 rest  ,             选择 sell      )

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max(   选择 rest  ,           选择 buy         )
```

i:天数，k是交易次数(买卖算一次)，0(没有)/1(持有)

状态转移方程：

```python

dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max(   选择 rest  ,             选择 sell      )

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max(   选择 rest  ,           选择 buy         )
```			  

框架：

```python
base case：
dp[-1][k][0] = dp[i][0][0] = 0
dp[-1][k][1] = dp[i][0][1] = -infinity

状态转移方程：
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])

```


##### 买卖股票的最佳时机

只能交易一次，k =1 

lambda [arg1 [,arg2,.....argn]]:expression

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        dp_i_0 = 0
        dp_i_1 = -float('inf')

        for i in range(len(prices)):
            dp_i_0 = max(dp_i_0,dp_i_1 + prices[i])
            dp_i_1 = max(dp_i_1,-prices[i])
        
        return dp_i_0
```


##### 买卖股票的最佳时机 II

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票），隔日

```python

class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        dp_i_0 = 0
        dp_i_1 = -float('inf')

        for i in range(len(prices)):
            temp = dp_i_0
            dp_i_0 = max(dp_i_0,dp_i_1 + prices[i])
            dp_i_1 = max(dp_i_1,temp - prices[i])
        
        return dp_i_0
```

##### 最佳买卖股票时机含冷冻期

可以多次交易，间隔一天冷静期 pre

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:

        dp_i_0 = 0
        dp_i_1 = -float('inf')
        pre = 0 

        for i in range(len(prices)):
            temp = dp_i_0
            dp_i_0 = max(dp_i_0,dp_i_1 + prices[i])
            dp_i_1 = max(dp_i_1,pre - prices[i])
            pre = temp
        
        return dp_i_0
```


#####  买卖股票的最佳时机含手续费

隔日，购买收手续费

```
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:

        dp_i_0 = 0
        dp_i_1 = -float('inf')
        pre = 0 

        for i in range(len(prices)):
            temp = dp_i_0
            dp_i_0 = max(dp_i_0,dp_i_1 + prices[i])
            dp_i_1 = max(dp_i_1,temp - prices[i] - fee)
        
        return dp_i_0	
```



##### 买卖股票的最佳时机 III

k = 2 笔交易

```python

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        max_k = 2
        dp = [[[0 for i in range(2)] for i in range(max_k+1)] for i in range(n)] 

        for i in range(n):
            for k in range(max_k,0,-1):
                if i == 0:
                    dp[i][k][0] = 0
                    dp[i][k][1] = -prices[i]
                    continue
                    
                dp[i][k][0] = max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
                dp[i][k][1] = max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])

        return dp[n-1][max_k][0]
```

##### 买卖股票的最佳时机 IV

k = any integer

```python

class Solution:
    def maxProfitNoTimes(self,prices):
        dp_i_0 = 0 
        dp_i_1 = -float('inf') 
        for i in range(len(prices)):
            temp = dp_i_0
            dp_i_0 = max(dp_i_0,dp_i_1 + prices[i])
            dp_i_1 = max(dp_i_1,temp - prices[i])
        
        return dp_i_0

    def maxProfit(self, k, prices: List[int]) -> int:
        n = len(prices)
        
        if k >= n/2:
            return self.maxProfitNoTimes(prices)
            
        max_k = k
        dp = [[[0 for i in range(2)] for i in range(max_k+1)] for i in range(n)]     
        for i in range(n):
            for k in range(max_k,0,-1):
                if i == 0:
                    dp[i][k][0] = 0
                    dp[i][k][1] = -prices[i]
                    continue
                    
                dp[i][k][0] = max(dp[i-1][k][0],dp[i-1][k][1] + prices[i])
                dp[i][k][1] = max(dp[i-1][k][1],dp[i-1][k-1][0] - prices[i])

        return dp[n-1][max_k][0]

```

#### 抢房子问题

##### 198. 打家劫舍

隔一间，打劫一间

```python

class Solution:
    def rob(self, nums: List[int]) -> int:

        dp_i = 0 
        dp_i_1 = 0
        dp_i_2 = 0
        n = len(nums)
        for i in range(n-1,-1,-1):
            dp_i = max(dp_i_1,nums[i] + dp_i_2)
            dp_i_2 = dp_i_1
            dp_i_1 = dp_i
        
        return dp_i
```

##### 213. 打家劫舍 II

屋子成环

```python

class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 1:
            return nums[0]
        elif n == 2:
            return max(nums[0],nums[1])
        return max(self.dp(nums,0,n-2),self.dp(nums,1,n-1))

    
    def dp(self,nums,start,end):
        dp_i = 0 
        dp_i_1 = 0
        dp_i_2 = 0
        n = len(nums)
        for i in range(end,start-1,-1):
            dp_i = max(dp_i_1,nums[i] + dp_i_2)
            dp_i_2 = dp_i_1
            dp_i_1 = dp_i
        
        return dp_i

```
		
		
##### 337. 打家劫舍 III

二叉树间隔抢劫

```python
class Solution:
    def rob(self, root: TreeNode) -> int:
        def dfs(root):
            if not root:return 0,0

            left_select,left_noselect = dfs(root.left)
            right_select,right_noselect = dfs(root.right)

            select = left_noselect + root.val + right_noselect
            noselect = max(left_select,left_noselect) + max(right_select,right_noselect)

            return select,noselect
        
        return max(dfs(root))
```



#### 背包问题： 

https://app.gitbook.com/@maoshifan1989/s/python/hui-su-yu-dong-tai-gui-hua/bei-bao-dp

0-1 背包：每件物品只有一件，子集背包: 416. 分割等和子集

完全背包: n 种物品，每种物品可以取无限件， 322 零钱兑换  518. 零钱兑换 II


```python

0-1 背包 模板

dp[i][w] 的定义如下：对于前 i 个物品，当前背包的容量为 w，这种情况下可以装的最大价值是 dp[i][w]。

int[][] dp[N+1][W+1]
dp[0][..] = 0
dp[..][0] = 0

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
		
return dp[N][W]


完全背包 模板

dp[i][j] 定义: 若只使用 coins 中的前 i 个(种)硬币的面值，若想凑出金额 j，有 dp[i][j] 种凑法

N = len(coins)
int dp[N+1][amount+1]
dp[0][..] = 0
dp[..][0] = 1


for (int i = 1; i <= N; i++) {
    for (int j = 1; j <= amount; j++) {
        if (j - coins[i-1] >= 0)
            dp[i][j] = dp[i - 1][j] 
                     + dp[i][j-coins[i-1]];
return dp[N][W]


```

##### 416. 分割等和子集

```python
	
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        sum = 0 
        for num in nums:
            sum += num
        if sum % 2 == 1:
            return False
        sum = sum//2 
        n = len(nums)

        dp = [[False for i in range(sum + 1)] for j in range(n+1)]
        for i in range(n+1):
            dp[i][0] = True
        
        for i in range(1,n + 1):
            for j in range(1,sum + 1):
                if j <  nums[i-1]:
                    dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j] | dp[i-1][j - nums[i-1]]
                
        return  dp[n][sum]	

```
##### 494. 目标和

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:

        sum = 0
        for num in nums:
            sum += num
        diff = sum - target
        if diff < 0 or  diff % 2 == 1:
            return 0
        neg = diff //2
        n = len(nums)
        dp = [[0 for i in range(neg + 1)] for j in range(n + 1)]

        dp[0][0] = 1
        for i in range(1,n + 1):
            num = nums[i - 1]
            for j in range(neg + 1):
                dp[i][j] = dp[i - 1][j]
                if j >= num:
                    dp[i][j] += dp[i - 1][j - num]

        return dp[n][neg]
```

##### 322. 零钱兑换

零钱兑换问题

```python

class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf') for i in range(amount + 1)]
        dp[0] = 0
		#类似完全平方数
		#转移方程
        for coin in coins:
            for i in range(coin,amount + 1):
                dp[i] = min(dp[i],dp[i-coin] + 1)

        return dp[amount] if dp[amount] <= amount else -1
```


#### 518. 零钱兑换 II

```python

class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        n = len(coins)
        dp = [[0 for i in range(amount + 1)] for j in range(n + 1)]

        for i in range(n+1):
            dp[i][0] = 1
        for i in range(1,n + 1):
            for j in range(1,amount + 1):
                if j < coins[i-1]:
                    dp[i][j] = dp[i - 1][j]
                else:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]]
                
        return dp[n][amount]	

```


## 4  回溯

回溯算法就是个多叉树的遍历问题

##### 51. N 皇后

```python

class Solution:
    def __init__(self):
        self.res = []
    def solveNQueens(self, n: int) -> List[List[str]]:
        board = [['.' for i in range(n)] for j in range(n)]
        self.backtrack(board,0)
        return self.res

    def backtrack(self,board,row):
        if row == len(board):
            #注意把二维list 转一维
            self.res.append(list(''.join(board[i]) for i in range(len(board))))
            return 

        n = len(board)
        for j in range(n):
            if not self.isvalid(board,row,j):
                continue
            board[row][j] = 'Q'
            self.backtrack(board,row + 1)
            board[row][j] = '.'

    def isvalid(self,board,row,col):
        n = len(board)
        for i in range(n):
            if board[i][col] == 'Q':
                return False

        i = row - 1
        j = col - 1
        #区分两个
        while i >= 0 and j >= 0:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j -= 1

        i = row - 1
        j = col + 1
        while i >= 0 and j < n:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j += 1

        return True

```

##### 78. 子集

```python
class Solution:
    def __init__(self):
        self.res = []

    def subsets(self, nums: List[int]) -> List[List[int]]:
        track = []
        self.backtrack(nums,0,track)
        return self.res

    def backtrack(self,nums,start,track):
        self.res.append(list(track))

        for i in range(start,len(nums),1):
            track.append(nums[i])
            #注意i + 1
            self.backtrack(nums,i+1,track)
            track.pop(-1)
```


##### 77 组合

```python
class Solution:
    def __init__(self):
        self.res = []

    def combine(self, n: int, k: int) -> List[List[int]]:
        track = []
        self.backtrack(track,1,k,n+1)
        return self.res

    def backtrack(self,track,start,k,n):
        if len(track) == k:
            self.res.append(list(track))
            return 

        for i in range(start,n):
            track.append(i)
            #注意这里i+1
            self.backtrack(track,i+1,k,n)
            track.pop(-1)
```
##### 46 全排列

```python
class Solution:
    def __init__(self):
        self.res = []
        self.visit = []

    def backtrack(self,nums,track):
        if len(track) == len(nums):
            self.res.append(list(track))
            return 
        
        for i in range(len(nums)):
            if self.visit[i]:
                continue
            
            self.visit[i] = True
            track.append(nums[i])
            self.backtrack(nums,track)
            track.pop(-1)
            self.visit[i] = False


    def permute(self, nums: List[int]) -> List[List[int]]:
        track = []
        self.visit = [False for i in range(len(nums))]
        self.backtrack(nums,track)

        return self.res
```

#####  FloodFill 问题, 岛屿问题


```python
class Solution:
    def dfs(self,grid,row,col)-> int:
        if row > len(grid) -1  or col > len(grid[0])-1 or row < 0 or col < 0:
            return 
        if grid[row][col] != '1':
            return 
        grid[row][col] = '2'
        self.dfs(grid,row - 1,col)
        self.dfs(grid,row + 1,col)
        self.dfs(grid,row,col - 1)
        self.dfs(grid,row,col + 1)
        
        
    def numIslands(self, grid: List[List[str]]) -> int:
        row = len(grid)
        col = len(grid[0])
        count = 0 

        for i in range(row):
            for j in range(col):
                if grid[i][j] == '1':
                    self.dfs(grid,i,j)
                    count += 1
        
        return count 
```

##### 42.接雨水

左边最大、右边最大，相互减去

```python

class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        left_max = [0 for i in range(n)]
        right_max = [0 for i in range(n)]

        left_max[0] = height[0]
        right_max[n-1] = height[n-1]

        for i in range(1,n):
            left_max[i] = max(left_max[i-1],height[i])
        
        for j in range(n-2,-1,-1):
            right_max[j] = max(right_max[j+1],height[j])

        sum = 0 
        for k in range(n):
            water = min(left_max[k],right_max[k])
            sum += water - height[k]

        return sum

```


## 6 双指针

#### 快慢指针

##### 142. 环形链表 II

````python

快慢指针

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:

        #先相遇
        slow = head
        fast = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break

        if not fast or not fast.next:
            return None
        
	#距开始k-m
        slow = head
        while fast != slow:
            fast = fast.next
            slow = slow.next
        return slow
````

```python

左右指针

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0 
        right = len(numbers) - 1
        while left < right:
            sum = numbers[left] + numbers[right]
            if sum == target:
                return [left+1,right+1]
            elif sum < target:
                left += 1
            else:
                right -= 1
```   

#### two sum类问题

有序数组使用双指针，无序数组使用hash表

```python

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        map = dict()
        for i in range(len(nums)):
            map[nums[i]] = i
        for i in range(len(nums)):
            sub = target - nums[i]
            if sub in map and map[sub] != i:
                return [i,map[sub]]
            
```
###  双指针去重  

#####  26. 删除有序数组中的重复项

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:

        n = len(nums)
        slow = 0
        fast = 0
        while fast < n:
            if nums[slow] != nums[fast]:
                slow += 1
                nums[slow] = nums[fast]
            fast += 1

        return slow + 1
```

##### 83. 删除排序链表中的重复元素

```python

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return
        slow = head
        fast = head.next
        while fast:
            if slow.val != fast.val:
                slow.next = fast
                slow = slow.next
            fast = fast.next
        slow.next = None
        return head
		
```
回文数：中心拓展法

##### 5. 最长回文子串

```python
class Solution:
    def PalindromeStr(self,s,l,r):
        while l >= 0 and r < len(s):
            if s[l] == s[r]:
                l -= 1
                r += 1
            else:
                break
        return s[l+1:r]


    def longestPalindrome(self, s: str) -> str:
        index = 0
        res = s[0]
        while index < len(s):
            str1 = self.PalindromeStr(s,index,index)
            str2 = self.PalindromeStr(s,index,index + 1)
            
            #if else 判断
            max_str = str1 if len(str1) > len(str2) else str2
            res = res if len(res) > len(max_str) else max_str

            index += 1
        return res
```

#####  回文链表

``` python 
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        
        slow = head
        fast = head
        pre = head
        prepre = None
        while fast and fast.next:
            pre = slow
            slow = slow.next
            fast = fast.next.next
            pre.next = prepre
            prepre = pre    
        #模拟奇数偶数情况
        if fast:
            slow = slow.next
        while pre and slow:
            if pre.val != slow.val:
                return False
            
            pre = pre.next
            slow = slow.next
 
        return True
```
##### 回文子串

````python

class Solution:
    def countSubstrings(self, s: str) -> int:
        slen = len(s) 
        num = 2 * slen - 1
        left = 0
        right = 0
        ans = 0 
        for center in range(num):
            #偶数 奇数问题
            left = center // 2 
            right = left + center % 2
            
            while left >= 0 and right < len(s) and  s[left] == s[right]:
                ans += 1
                left -= 1
                right += 1
        return ans 
````


## 8 滑动窗口


5个基本元素： need(dict)、windows(dict)、left、right、valid

先while找扩大right,后while缩小left 

一共5个if 


##### 76. 最小覆盖子串

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if len(s) < len(t):
            return ''
        need = dict()
        window = dict()
        left = 0 
        right = 0 
        valid = 0 
        start = 0 
        #相当于一个极大值
        startlen = len(s) * 2
        for char in t:
            if char in need:
                need[char] += 1
            else:
                need[char] = 1
                window[char] = 0
        
        while right < len(s):
            char = s[right]
            right += 1
            if char in window:
                window[char] += 1
                if window[char] == need[char]:
                    valid += 1

            while valid == len(need):
                #收缩left
                if right - left < startlen:
                    startlen = right - left
                    start = left
                char = s[left]
                left += 1
                if char in window:
                    if window[char] == need[char]:
                        valid -= 1
                    window[char] -= 1
					
        if startlen == len(s)*2:
            return ''
        else:
            return s[start:start + startlen]	
```


##### 438. 找到字符串中所有字母异位词

```python

第二个while
			
			while valid == len(need):
			#收缩left
			if right - left == taget_len:
				res.append(left)
```

#####  3. 无重复字符的最长子串

```python				
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if s == '':return 0
        left = 0 
        right = 0
        map = dict()
        maxlen = -1
        while right < len(s):
            char = s[right]
            right += 1
            map[char] = map.get(char,0) + 1

            while map[char] > 1:
                charleft = s[left]
                left += 1
                map[charleft] -= 1

            maxlen = max(maxlen,right-left)
        
        return maxlen
```

##### 567. 字符串的排列



```python
                while valid == len(need):
                    if right - left == len(s1):
                        return True
```




## 9 其他算法

### 贪心算法

##### 55. 跳跃游戏I

贪心:不需要递归所有的结果，只需要选择最有潜力的结果。

```python

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        farest = 0
		
		#len - 1 解决少一个数的问题
        for i in range(len(nums)- 1):
            farest = max(farest,i + nums[i])
            if farest <=i:
                return False
            
        return farest >= len(nums) - 1
```


##### 45. 跳跃游戏II

贪心法，主要循环n-1

```python

class Solution:
    def jump(self, nums: List[int]) -> int:
        
        farest = 0 
        end = 0
        jump = 0
        for i in range(len(nums)-1):
            farest = max(farest,i + nums[i])
            if i == end:
                jump += 1
                end = farest
        
        return jump

```

##### 区间调度问题  435  无重叠区间

所有与 x 相交的区间必然会与 x 的 end 相交；如果一个区间不想与 x 的 end 相交，它的 start 必须要大于（或等于）x 的 end

sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。

sorted(intvs,key=(lambda x:x[1]))

或者 intvs.sort(key=lambda x: x[1]) 


```python
class Solution:
    def intervalSchedule(self,intvs):

        intvs.sort(key=(lambda x:x[1]))  
        count = 1
        intv_end = intvs[0][1]
        index = 1
        while index < len(intvs):
            if intvs[index][0] >= intv_end:
                count += 1
                intv_end = intvs[index][0]
            index += 1
        
        return count

    def eraseOverlapIntervals(self,intervals):

        num = self.intervalSchedule(intervals)
        return len(intervals) - num
```

##### 56. 合并区间

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:

        intervals.sort(key = lambda x:x[0])
        res = []
        res.append(intervals[0])

        for i in range(1,len(intervals),1):
            #这里是引用
            last = res[-1]
            if intervals[i][0] <= last[1]:
                last[-1] = max(last[-1],intervals[i][1])
            else:
                res.append(intervals[i])
            
        return res
```

##### 43. 字符串相乘

num1[i] 和 num2[j] 的乘积对应的就是 res[i+j] 和 res[i+j+1]

P2；个位累积，再取余

P1: 进位

转字符串的方法： ''.join(str(i) for i in res[k:])

```python
	
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        m = len(num1)
        n = len(num2)
        res = [0 for i in range(m+n)]

        for i in range(m-1,-1,-1):
            for j in range(n-1,-1,-1):
                p1 = i + j 
                p2 = i + j + 1
                mul = int(num1[i])*int(num2[j]) 
                sum = mul + res[p2]

                res[p2] = sum % 10
                res[p1] += sum // 10

        k = 0 
        while k < (m + n) and res[k] == 0:
            k += 1
            
        if k == (m + n):
            return '0'

        return ''.join(str(i) for i in res[k:])

```
#### KMP 算法

dp[j][c] = next 表示，当前是状态 j，遇到了字符 c，应该转移到状态 next

影子状态：X，它永远比当前状态 j 落后一个状态, 拥有和 j 最长的相同前缀

构建当前状态 j 的转移方向时，只有字符 pat[j] 才能使状态推进（dp[j][pat[j]] = j+1）；

而对于其他字符只能进行状态回退，应该去请教影子状态 X 应该回退到哪里（dp[j][other] = dp[X][other]，其中 other 是除了 pat[j] 之外所有字符）。


```python

class KMP:
    def __init__(self) -> None:
        self.dp = []
        self.pat = ''

    def KMP(self,pat):
        self.pat = pat
        m = len(pat)
        self.dp = [[0 for i in range(256)] for j in range(m)]
        X = 0
        self.dp[0][ord(pat[0])] = 1
        for j in range(1,m):
            for c in range(256):
                self.dp[j][c] = self.dp[X][c]
            self.dp[j][ord(pat[j])] = j + 1
            X = self.dp[X][ord(pat[j])]

    def search(self,txt):
        M = len(self.pat)
        N = len(txt)
        j = 0 
        for i in range(N):
            j = self.dp[j][ord(txt[i])]

            if j == M:
                return i - M + 1
        
        return -1 

s1 = KMP()
s1.KMP("aaab")
pos = s1.search("aaacaaab")
print(pos)


```

#### 位运算

n & (n - 1): 消除数字 n 的二进制表示中的最后一个 1

##### 判断两个数是否异号

```python

bool f = ((x ^ y) < 0); // true

bool f = ((x ^ y) < 0); // false

```

##### 231. 2 的幂

```python

class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n <= 0:
            return False
        return n & (n-1) == 0
		
```


#### 前缀和

##### 560. 和为K的子数组

前缀和+ 哈希表优化

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

##### 437. 路径总和 III

```python

class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        
        def recurPathSum(root,curSum,targetSum):
            if not root:
                return 0
            
            curSum += root.val

            res = 0
            res += mapSum.get(curSum - targetSum,0)
            mapSum[curSum] = mapSum.get(curSum,0) + 1

            res += recurPathSum(root.left,curSum,targetSum)
            res += recurPathSum(root.right,curSum,targetSum)

            mapSum[curSum] = mapSum.get(curSum,0) - 1

            return res

        mapSum = dict()
        mapSum[0] = 1
        return recurPathSum(root,0,targetSum)
```

#### 拓扑排序

##### 207. 课程表

拓扑排序：BFS遍历，indegree表、adjacency邻接表、queue(deque)

```python

from collections import deque
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        indegree = [0 for i in range(numCourses)]
        adjacency = [[] for i in range(numCourses)]

        for cur,pre in prerequisites:
            indegree[cur] += 1
            adjacency[pre].append(cur)
        queue  = deque()
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)
        while queue:
            top = queue.popleft()
            #无环：出队次数==节点个数
            numCourses -= 1
            for cur in adjacency[top]:
                indegree[cur] -= 1
                #入度为0，则加入
                if indegree[cur] == 0:
                    queue.append(cur) 

        return not numCourses
```

#####   210. 课程表 II

```python

from collections import deque
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        indegree = [0 for i in range(numCourses)]
        adjacency = [[] for i in range(numCourses)]

        for cur,pre in prerequisites:
            indegree[cur] += 1
            adjacency[pre].append(cur)
        queue  = deque()
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)

        result = []
        while queue:
            top = queue.popleft()
            result.append(top)
            for cur in adjacency[top]:
                indegree[cur] -= 1
                #入度为0，则加入
                if indegree[cur] == 0:
                    queue.append(cur) 

        if len(result) != numCourses:
            result = []

        return result
```

##### 前缀树

self使用：类中函数的第一个参数是实例对象本身，并且约定俗成，把其名字写为self。其作用相当于java中的this

字符串处理ord

本质是一个26节点的树

208. 实现 Trie (前缀树)

```python

class Trie:
    
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.children = [None] *26
        self.isEnd = False
        
    def searchPrefix(self, prefix: str):
        node = self
        for i in range(len(prefix)):
            ch = ord(prefix[i]) - ord('a')
            if not node.children[ch]:
                return None
            node = node.children[ch]
        return node
    
    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self
        for i in range(len(word)):
            ch = ord(word[i]) - ord('a')
            if not node.children[ch]:
                node.children[ch] = Trie();
            node = node.children[ch]
        node.isEnd = True
        return 

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        ret = self.searchPrefix(word)
        if ret and ret.isEnd:
            return True
        return False


    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        if self.searchPrefix(prefix) and not self.isEnd:
            return True
        return False
```

##### 并查集

并查集模板: 3个成员 count、parent、size, 3个函数 union、isconnected、find

```python

class UnionFind:
    def __init__(self,n):
        # count 联通域个数
        # size 每棵树包含的节点数,重量
        self.count = n
        self.parent = [i for i in range(n)]
        self.size = [i for i in range(n)]
		
    #连通两个节点union
    def union(p,q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return
		
		#小树接在大树下
        if self.size[rootP]  > self.size[rootQ]:
            self.parent[rootQ] = rootP
            self.size[rootP] += 1
        else:
            self.parent[rootP] = rootQ
            self.size[rootQ] += 1
        count -= 1
        return
		
    #判断节点连通
    def isconnected(p,q):
        rootP = self.find(p)
        rootQ = self.find(q)
        return rootP == rootQ
		
    #路径压缩
    def find(x):
        while self.parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x
```


399 除法求值

```python

class UnionFind:
    def __init__(self,n):
        # count 联通域个数
        # size 每棵树包含的节点数
        self.count = n
        self.parent = [i for i in range(n)]
        self.weight = [1.0 for i in range(n)]
    #连通两个节点union
    def union(self,p,q,value):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return
        self.parent[rootP] = rootQ
        self.weight[rootP] = self.weight[q] * value / self.weight[p]
        return
    #判断节点连通
    def isconnected(self,p,q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP == rootQ:
            return self.weight[p]/self.weight[q]
        else:
            return -1.0
    #路径压缩
    def find(self,x):
        if self.parent[x] != x:
            origin = self.parent[x]
            self.parent[x] = self.find(self.parent[x])
            #递归过程中，父节点权重由上向下更新了
            self.weight[x] *= self.weight[origin]
        return self.parent[x]

class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        
        numSize = len(equations)
        UF = UnionFind( 2 * numSize)
        map = dict()
        id = 0
        for i in range(numSize):
            var1 = equations[i][0]
            var2 = equations[i][1]
            
            if var1 not in map:
                map[var1] = id
                id += 1
            if var2 not in map:
                map[var2] = id
                id += 1
            UF.union(map[var1],map[var2],values[i])
        
        res = []
        for i in range(len(queries)):
            var1 = queries[i][0]
            var2 = queries[i][1]
            
            id1 = map.get(var1,-1)
            id2 = map.get(var2,-1)
            
            if id1 == -1 or id2 == -1:
                res.append(-1.0)
            else:
                res.append(UF.isconnected(id1,id2))
        return res
	
```
