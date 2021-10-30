## 1 链表（栈、队列、堆）

### 1.2  实例 

单调栈、单调队列（deque)、堆（heapq）

#### offer 09. 用两个栈实现队列

class CQueue:

    def __init__(self):

        self.stack_list1 = []
        self.stack_list2 = []

    def appendTail(self, value: int) -> None:

        self.stack_list1.append(value)

        return value

    def deleteHead(self) -> int:

        if self.stack_list2  == []:
            while len(self.stack_list1)!=0:
                self.stack_list2.append(self.stack_list1.pop(-1))

        if self.stack_list2 == []:
            return -1
        else:
            value = self.stack_list2.pop(-1)
            return value
			

#### offer 18  删除链表的节点

```python
class Solution:
    def deleteNode(self, head: ListNode, val: int) -> ListNode:
        
        if head.val == val:
            return head.next
        
        pre = head
        cur = head.next
        
        while cur  and cur.val != val:
            pre = cur
            cur = cur.next
        
        if cur:
            pre.next = cur.next
        
        return head
```

pre cur  双指针循进

python传值和引用,

值传：列表、字典、集合

引用传：数值、元组、字符串

	
#### offer  22 合并两个有序链表

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        
        cur = dum = ListNode(0)
        while l1 and l2:
            if l1.val < l2.val:
                cur.next,l1 = l1, l1.next
            else:
                cur.next,l2 = l2, l2.next
            cur = cur.next
        if l1: 
            cur.next = l1
        else:
            cur.next = l2
        
        return dum.next
```		
		
a,b  = b,c ----> a = b    b = c
		
非交换传数

#### offer 23  反转链表

```python

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:

        if head == None:
            return None

        cur = head.next
        prev = head
        prev.next = None
        while cur:
            next = cur.next        
            cur.next = prev 
            prev = cur
            cur = next

        return prev
```


双指针: pre cur next  前后指针（换序）

dummy链表法： 

	dummy = ListNode(0)

	return dummy.next	


#### offer 24  复杂链表的复制

##### 方法1: 链表法  复制-调整-拆分

```python

class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':

        if not head:
            return None
        
        cur = head
        while cur:
            tmp = Node(cur.val)
            tmp.next = cur.next
            cur.next = tmp 
            cur = tmp.next
        
        cur = head
        while cur:
            if cur.random:
                cur.next.random = cur.random.next
            cur = cur.next.next
            
        pre = head
        res = cur = head.next
        while cur.next:
            pre.next = pre.next.next
            cur.next = cur.next.next

            cur = cur.next
            pre = pre.next
        
        pre.next = None
        
        return res
```

##### 方法2: 字典法

```python

class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head: return None
        
        dict = {}
        cur = head
        while cur:
            dict[cur] = Node(cur.val)
            cur = cur.next

        cur = head
        while cur:
            dict[cur].next = dict.get(cur.next)
            dict[cur].random = dict.get(cur.random)
            cur = cur.next
        
        return dict[head]
```


字典的常规操作:
	
	dic = {}
	
	增加新元素: dict['Age'] = 8
	
	访问键值: dict['Age']、dict.get('Age')(键不存在时，返回None)
	
	遍历字典: dict.items()方法获取字典的各个元素即“键值对”的元祖列表
	
```python

dict = {1: 1, 2: 'aa', 'D': 'ee', 'Ty': 45}
for key, value in dict.items():
    print(key, value)
	    
```

#### offer 30 包含min函数的栈

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.A = []
        self.B = []

    def push(self, x: int) -> None:
        self.A.append(x)
        if not self.B or self.B[-1] >= x:
            self.B.append(x)

    def pop(self) -> None:
        tmp = self.A.pop()
        if tmp == self.B[-1]:
            self.B.pop()

    def top(self) -> int:
        
        return self.A[-1]

    def min(self) -> int:
        return self.B[-1]
```

双栈问题:  栈A正常存,  栈B存储非严格降序的元素

单调栈解法：

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


反向迭代单调栈



#### offer 50 滑动窗口的最大值

单调队列： collections.deque()

常用API： 

d = collections.deque([])

d.append('a')   #右边添加元素

d.pop()   #右边弹出元素

d.appendleft('b')   #左边添加元素

d.popleft() # 将最左边的元素取出

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

单调队列


#### offer 41  数据流的中位数---双堆问题

双堆问题: 大顶堆和小顶堆组合

Python标准库模块之heapq
 
 https://www.jianshu.com/p/801318c77ab5
 
heapq有两种方式创建堆， 一种是使用一个空列表，然后使用heapq.heappush()函数把值加入堆中(弹出则是heapq.heappop())，另外一种就是使用heap.heapify(list)转换列表成为堆结构
 
heapq默认的是小顶堆，如果需要实现大顶堆，则需要push -num


```python

import heapq

class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.A = []
        self.B = []

    def addNum(self, num: int) -> None:
        if len(self.A) != len(self.B):
            heapq.heappush(self.A,num)
            heapq.heappush(self.B,-heapq.heappop(self.A))
        else:
            heapq.heappush(self.B,-num)
            heapq.heappush(self.A,-heapq.heappop(self.B))

    def findMedian(self) -> float:

        if len(self.A)!=len(self.B):
            return self.A[0]
        else:
            return (self.A[0]- self.B[0])/2
``` 

#### offer 31  栈的压入弹出序列

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        
        stack = []
        i = 0
        for j in range(len(pushed)):
            stack.append(pushed[j])
            while stack and stack[-1] == popped[i]:
                i += 1
                stack.pop()

        return not stack
```







## 2 树

### 2.2 实例

#### offer  26 树的子结构

```python
class Solution:
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        def recur(A,B):
            if B == None:
                return True
            if not A or A.val != B.val:
                return False
            return recur(A.left,B.left) and recur(A.right,B.right)

        return bool(A and B) and (recur(A,B) or  self.isSubStructure(A.left,B) or  self.isSubStructure(A.right,B))	
```	

if not A 判空法

树天生为了递归左右子树而存在


二叉树的："recur  +   isSub法"

#### offer 27  二叉树的镜像

```python
class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if root == None:
            return None
        left = self.mirrorTree(root.right)
        right = self.mirrorTree(root.left)

        root.left,root.right = left,right

        return root
```

#### offer 28 对称的二叉树

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def recur(left,right):
            if not left  and not right :
                return True
            if not left or not right  or  left.val != right.val:
                return False
            return recur(left.left,right.right) and recur(left.right,right.left)

        if not root:
            return True

        return recur(root.left,root.right)
```
		
连续 not 判断


#### offer 33   二叉搜索树的后序遍历序列

判断一个数组是否是二叉树的后序遍历结果

```python
class Solution:
    def verifyPostorder(self, postorder: List[int]) -> bool:

        def recur(i,j):
            if i >= j: 
                return True
            p = i
            while(postorder[p] < postorder[j]):
                p += 1
            m = p
            while(postorder[p] > postorder[j]):
                p += 1
            return p == j and recur(i,m - 1) and recur(m,j-1)

        return recur(0,len(postorder) - 1)

```

#### Offer 55 - I. 二叉树的深度
```python

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        left_max = self.maxDepth(root.left)
        right_max = self.maxDepth(root.right)
        maxDepth = 1 + max(left_max,right_max)

        return maxDepth
```


#### Offer 55 - II. 平衡二叉树


```python

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
        
        return self.isBalanced(root.left) and self.isBalanced(root.right)\
```


```python
自底而上: o(n)

class Solution:
            
    def isBalanced(self, root: TreeNode) -> bool:
        def height(root:TreeNode)-> int:
            if not root:
                return 0
            left_height = height(root.left)
            right_height = height(root.right)
            if left_height == -1 or right_height == -1 or abs(left_height - right_height) > 1:
                return -1
            else:
                return max(left_height,right_height) + 1

        return height(root) >= 0
```
		
自底而上：先判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡,复杂度O(n)

自顶而下：先判断当前节点，再判断左右子树，复杂度o(n*n）

 #### Offer 68 - I. 二叉搜索树的最近公共祖先

祖先不可以是自己

```python
迭代法： time O(n)   space O(1)

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

        ancestor = root
        while ancestor:
            if ancestor.val < p.val and ancestor.val < q.val :
                ancestor = ancestor.right
            elif ancestor.val > p.val and ancestor.val > q.val:
                ancestor = ancestor.left
            else:
                break
        
        return ancestor
```


```python 
递归法
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

	ancestor = root
	while ancestor:
		if ancestor.val < p.val and ancestor.val < q.val :
			ancestor = self.lowestCommonAncestor(ancestor.right,p,q)
		elif ancestor.val > p.val and ancestor.val > q.val:
			ancestor = self.lowestCommonAncestor(ancestor.left,p,q)
		else:
			break
	
	return ancestor
	
```

#### Offer 68 - II. 二叉树的最近公共祖先

祖先可以是自己，是q、p、或者q和p中间的

树问题，天然使用递归法

```python
	
class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        
        if not root or root.val == p.val or root.val == q.val:
            return root
        left = self.lowestCommonAncestor(root.left,p,q)
        right = self.lowestCommonAncestor(root.right,p,q)

        if not left and not right: return
        if not left:
            return right
        if not right:
            return left

        return root	
	
```


## 3 动态规划

### 3.2 实例


#### offer 13  机器人的运动范围

 ```python
    def digit_num(num):
        sum = 0
        while num:
            sum = sum + num%10
            num = num//10
        return sum

    class Solution:
            def movingCount(self, m: int, n: int, k: int) -> int:

                vis = set([(0,0)])

                for i in range(m):
                    for j in range(n):
                        if ((i-1,j) in vis or (i,j-1) in vis) and digit_num(i) + digit_num(j) <= k:
                            vis.add((i,j))

                return len(vis)
```


**Notes:**

	**set**

	用set来表示一个无序不重复元素的序列。set的只要作用就是用来给数据去重。 

	可以使用大括号 { } 或者 set() 函数创建集合

	eg: 

	添加一对数：

	vis = set([(0, 0)])

	vis.add((i, j))


/ 除以，带小数

//整除

	**dict**

	创建字典：

	d = {key1 : value1, key2 : value2 }

	eg；

	dict = {'a': 1, 'b': 2, 'b': '3'}

	**tuple**

	tup1 = ('physics', 'chemistry', 1997, 2000)


#### offer 19 字符串匹配

```python

class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        m = len(s)
        n = len(p)
        memo = dict()
        def dp(i,j):
            if j == n:
                return i == m
            if i == m:
                if (n-j) %2  == 1:
                    return False
                for k in range(j,n,2):
                    if p[k+1] != '*':
                        return False
                return True

            concat = str(i) +'_' + str(j)
            if concat in memo:
                return memo[concat]
            
            Flag = False
            if s[i] == p[j] or p[j] == '.':
                if j < n-1 and p[j+1] == '*':
                    Flag = dp(i,j+2) or dp(i+1,j)
                else:
                    Flag = dp(i+1,j+1)
            else:
                if j < n-1 and p[j + 1] == '*':
                    Flag = dp(i,j + 2)
                else:
                    Flag = False

            memo[concat] = Flag
            return Flag

        return dp(0,0)

```
		
dp[i][j]:表示s的前i个字符和p中的前j个字符是否匹配。

第i个和实际索引i-1的对应关系问题
		
穷举（写出状态方程，暴力穷举）--->备忘录消除重叠子问题（自顶而下的递归）

python 没有++运算符，只有 a+=1

not bool的结构

递归问题一般结构：判截止---> 查memo --->做选择 --->置memo 

```python

memo模板
    
    memo = dict()
    def dp(i,j):

        #判断截止条件
        if i == m or j == n:
            return 
        
        if (i,j) in memo:
            return memo[i,j]
        
        #状态选择递归
        dp: dp(i,j) --->dp(i+1,j+1)或者其他,计算ret

        置位memo
        memo[i,j] = ret
        
        return ret

```

#### offer 42  连续子数组的最大和

动态规划的变形，思考前缀和数组法

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        
        for i in range(1,len(nums)):
           nums[i] += max(nums[i-1],0)
        
        return max(nums)
```

#### Offer 46. 把数字翻译成字符串

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。


```python
class Solution:
    def translateNum(self, num: int) -> int:
        
        num_str = str(num)
        dp = [0 for i in range(len(num_str) + 1)]
        dp[0] = 1
        dp[1] = 1 

        for i in range(2,len(num_str)+ 1):
            if '10' <= num_str[i-2:i] <= '25':
                dp[i] = dp[i-1] + dp[i-2]
            else:
                dp[i] = dp[i - 1]

        return dp[len(num_str)]
```

含义：d[i],对于num_str的i-1，dp[m]、memo[m],长度是m + 1
 
str 切片和比较大小的方法


#### Offer 47. 礼物的最大价值


```python
class Solution:
    def maxValue(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        dp = [[0]*n for i in range(m)]

        dp[0][0] = grid[0][0]
        for i in range(1,m):
            dp[i][0] = grid[i][0] + dp[i-1][0]
        for j in range(1,n):
            dp[0][j] = grid[0][j] + dp[0][j-1]

        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]) + grid[i][j]
                
        return dp[m-1][n-1]
```

dp解法
		
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


#### Offer 49. 丑数

一维动归方程

```python

class Solution:
    def nthUglyNumber(self, n: int) -> int:

        dp = [0 for i in range(n+1)]
        dp[1] = 1

        p2 = 1
        p3 = 1
        p5 = 1
        for i in range(2,n+1):
            dp[i] = min(dp[p2]*2,dp[p3]*3,dp[p5]*5)
            if dp[i] == dp[p2]*2:
                p2 += 1
            if  dp[i] == dp[p3]*3:
                p3 += 1
            if dp[i] == dp[p5]*5:
                p5 += 1
            
        return dp[n]
```
一维动归，转移方程

####  Offer 63. 股票的最大利润


```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        dp = [0]*len(prices)
        cost = prices[0]
        for i in range(1,len(prices)):
            dp[i] = max(dp[i-1],prices[i] - cost)
            cost = min(cost,prices[i])

        return dp[len(prices)-1]
```

一维动归，股票利润问题

#### Offer 64. 求1+2+…+n

DFS + 逻辑符短路+ self变量存储状态

```python

class Solution:
    def __init__(self):
        self.res = 0
    def sumNums(self, n: int) -> int:
        n > 1 and self.sumNums(n - 1)
        self.res += n 
        return self.res
```
###  Offer 62. 圆圈中最后剩下的数字

约瑟夫环问题: 

	动态规划的递推公式


	下标递推公式：
	n-1,m   --> n,m

	x  --> （x+t)%n    t = m%n-1
	
	f(n) = （f(n-1) + t)%n 
		 = （f(n-1) + m)%n
	
	dp[n] = (dp[n-1] + m)%n

```python

class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        
        x = 0 #dp[1] = 0
        #dp[i] 含义 长度为i 删除留下的数
        for i in range(2,n + 1):
            x  = (x + m)%i
        
        return x

```

#### Offer 60. n个骰子的点数

class Solution:
    def dicesProbability(self, n: int) -> List[float]:
		
		#一个骰子
        dp = [1/6] * 6
		
		#递推筛子数量 2->n
        for i in range(2,n+1):
			# n --- 6n 范围内的取值
            tmp = [0]*(5*i + 1)
			
			#递推取值情况，dp: 0-->len(dp),骰子： 0-5
            for j in range(len(dp)):
                for k in range(6):
                    tmp[j+k] += dp[j] *(1/6)
			#赋值回去
            dp = tmp        
        return dp





## 4 回溯（DFS）

### 4.2 实例

#### offer 12 矩阵中的路径

 ```python
    class Solution:
        def exist(self, board: List[List[str]], word: str) -> bool:

            def dfs(i,j,k):
                if not  0 <= i < len(board) or  not   0 <= j < len(board[0]) or board[i][j]!= word[k]:
                    return False 

                if k  == len(word) - 1:
                    return True

                board[i][j] = ''
                res = dfs(i-1,j,k+1) or dfs(i+1,j,k+1) or dfs(i,j-1,k+1) or dfs(i,j+1,k+1)
                board[i][j] = word[k]

                return res 

            for i in range(len(board)):
                for j in range(len(board[0])):
                   if dfs(i,j,0):
                       return True

            return False
 ```           
**Notes**

注意这里的board[i][j] = '' 操作，再board[i][j]=word[k]撤销操作

#### offer 38. 字符串的排列
	
```python

class Solution:
    def permutation(self, s: str) -> List[str]:
        
        res = []
        vis = [False for i in range(len(s))]


        def backtrack(s,track):
            if len(track) == len(s):
                res.append("".join(track))
                return

            for i in range(len(s)):
                if vis[i]  or (i > 0 and not vis[i-1] and s[i-1] == s[i]):  ##从左到右选，前面没选不选后面
                    continue
            
                vis[i] = True
                track.append(s[i])
                backtrack(s,track)
                track.pop()
                vis[i] = False

        track = []
        s_list = list(s)
        s_list.sort()
        backtrack(s_list,track)
        return res
```	
	
重复字符：从左到右依次选择


#### Offer 34. 二叉树中和为某一值的路径

```python
class Solution:
    def pathSum(self, root: TreeNode, target: int) -> List[List[int]]:

        paths = []
        one_path = []
        def backtrack(root,target):
            if not root:
                return

            target -= root.val
            one_path.append(root.val)

            if target == 0 and not root.left and not root.right:
                paths.append(list(one_path))

            backtrack(root.left,target)
            backtrack(root.right,target)
            one_path.pop()
    
        backtrack(root,target)
        
        return paths
```

要考虑可能有负数节点

list存的时候用list(),因为传的引用，这样后面就不会变了

必须减去之后就判断，否则进入left right的backtrack后就因为root为none加不上了


## 5 BFS

### 5.2 实例

####  Offer 32 - I. 从上到下打印二叉树
```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        
        if not root:
            return []
        queue = []
        res = []

        queue.append(root)

        while queue:
            sz = len(queue)
            for i in range(sz):
                cur = queue.pop(0)
                res.append(cur.val)
                if cur.left != None:
                    queue.append(cur.left)
                if cur.right !=None:
                    queue.append(cur.right)
                
        return res
```

给定二叉树: [3,9,20,null,null,15,7],

输出： [3,9,20,15,7]


剑指 Offer 32 - I. 从上到下打印二叉树

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        
        if root == []:
            return []
        queue = []
        res = []

        queue.append(root)

        while queue:
            sz = len(queue)
            tmp = []
            for i in range(sz):
                cur = queue.pop(0)
                tmp.append(cur.val)
                if cur.left != None:
                    queue.append(cur.left)
                if cur.right !=None:
                    queue.append(cur.right)
            res.append(tmp)

        return res
```		
输出： 
[[3],[9,20],[15,7]]


## 6 双指针

### 6.2 实例


#### offer 21 调整数组顺序使奇数位于偶数前面

```python
class Solution:
    def exchange(self, nums: List[int]) -> List[int]:
        
        left = 0 
        right = len(nums) -1
        while left <= right:
            if nums[left] & 1 == 1:
                left += 1
                continue 
            if nums[right] & 1 == 0:
                right -= 1
                continue
            nums[left],nums[right] = nums[right],nums[left]
        return nums
```


判断奇偶的方法： nums[left] & 1 == 1 是奇数，否则是偶数

双指针：首尾双指针，快慢双指针


#### offer  22 链表中倒数第k个节点

```python

# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:

        if head == None:
            return None

        former = head
        for i in range(k):
            former = former.next

        latter = head 
        while former:
            former = former.next
            latter = latter.next

        return latter
```


双指针:former later 快慢指针（倒数计数）


#### offer 57. 和为s的两个数字

```python

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        a_index = 0
        b_index = len(nums) - 1
        while a_index <= b_index:
            if nums[a_index] + nums[b_index]  == target:
                return [nums[a_index],nums[b_index]]
            elif  nums[a_index] + nums[b_index] < target:
                a_index += 1
            else:
                b_index -= 1
        
        return []
```

双指针法，两数之和,首尾指针

#### Offer 52. 两个链表的第一个公共节点

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:

        A = headA
        B = headB
        while A!= B:
            if A:
                A = A.next
            else:
                A = headB
            if B:
                B = B.next
            else:
                B = headA

        return A
```

----
    |-------
----

a + b - c 

b + a - c

双指针算法


#### Offer 58 - I. 翻转单词顺序

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip()

        res = []
        i = len(s) - 1
        j = len(s) - 1
        while i >=0:
            while i >= 0 and s[i] != ' ':
                i -= 1

            slice = s[i+1:j+1]
            res.append(slice)

            while s[i] == ' ':
                i -= 1
            j = i 

        return ' '.join(res)
```

双指针 i  j  反向迭代

翻转问题

####  Offer 48. 最长不含重复字符的子字符串

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        if len(s) == 1:
            return 1

        maxsub = 0
        map = dict()
        i = -1
        for j in range(len(s)):
            if s[j] in map:
                i = max(i,map[s[j]])
            maxsub = max(maxsub,j - i)
            map[s[j]] = j

        return maxsub
```

map存索引

i 存最大不重复右侧





## 7 二分搜索

### 7.2 实例

#### offer  11. 旋转数组的最小数字

```python
class Solution:
    def minArray(self, numbers: List[int]) -> int:

        low = 0
        high = len(numbers)-1

        while low < high:
            mid = int((high + low )/2)
            if numbers[mid] > numbers[high]:
                low = mid +1 
            elif numbers[mid] < numbers[high]:
                high = mid
            else:
                high = high - 1

        return numbers[low]
```

#### 45. 把数组排成最小的数

快速排序变形

```python

class Solution:
    def minNumber(self, nums: List[int]) -> str:
        
        def quick_sort(l,r):
            if l >= r:
                return
            i = l
            j = r
            pivot = num_strs[l]
            while i < j:
                while num_strs[j] + pivot >= pivot + num_strs[j] and i < j:
                    j -= 1
                while num_strs[i] + pivot <= pivot + num_strs[i] and i < j:
                    i += 1

                num_strs[i],num_strs[j] = num_strs[j],num_strs[i]
            num_strs[i],num_strs[l] = num_strs[l],num_strs[i]

            quick_sort(l,i - 1)
            quick_sort(i + 1,r)
        

        num_strs = [str(num) for num in nums]
        quick_sort(0,len(nums)-1)

        return ''.join(num_strs)

```

基础快排改进

python 字符串比较大小的规则

pivot选择l的时候，快排为什么j先走:

    https://blog.csdn.net/lkp1603645756/article/details/85008715
	
    i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
	

## 8 滑动窗口

### 8.2 实例 

####  Offer 57 - II. 和为s的连续正数序列

双指针滑窗法

```python
class Solution:
    def findContinuousSequence(self, target: int) -> List[List[int]]:
        i = 1
        j = 2
        ret = []
        tmp = [i,j]
        sum = i + j
        while i < j and j < target:
            if sum == target:
                ret.append(list(tmp))
                tmp.pop(0)
                sum -= i
                i += 1
            elif sum < target:
                j += 1
                tmp.append(j)
                sum += j
            else:
                tmp.pop(0)
                sum -= i
                i += 1

        return ret 
```




## 9 其他高频

### 9.2 实例

位运算、幂运算、表格分区（计算）、字符串相关问题



**位运算**

#### offer 15 二进制中1的个数

```python

class Solution:
    def hammingWeight(self, n: int) -> int:
        sum = 0
        while n:
            n = n&(n-1)
            sum = sum + 1
                    
        return sum
```
		
二进制串读入

一定要先枚举分析题，举例测试题	


#### offer 44  数字序列中某一位的数字

```python
class Solution:
    def findNthDigit(self, n: int) -> int:

        start = 1
        count = 9
        digit = 1
        while n > count:
            n -= count
            start *= 10
            digit += 1
            count = start*digit*9

        num = start + (n - 1)/digit
        return int(str(num)[(n-1)%digit])
```

#### Offer 50. 第一个只出现一次的字符

```python
class Solution:
    def firstUniqChar(self, s: str) -> str:
        if s == "":
            return " "

        map = dict()
        order = []
        for char in s:
            if char in map:
                map[char] +=1
            else:
                order.append(char)
                map[char] = 1

        for char in order:
            if map[char] == 1:
                return char

        return " "
```

#### Offer 56 - I. 数组中数字出现的次数

```python
class Solution:
    def singleNumbers(self, nums: List[int]) -> List[int]:
        one_num = 0
        for num in nums:
            one_num = one_num ^ num
        div = 1
        while one_num & div == 0:
            div <<= 1
        a,b= 0,0 
        for num in nums:
            if num & div:
                a ^= num
            else:
                b ^= num
        return [a,b]


```


#### Offer 56 - II. 数组中数字出现的次数


```python

位运算法：one -- two 计算

class Solution:
    def singleNumber(self, nums: List[int]) -> int:

        one,two,no_three = 0,0,0
        for num in nums:
            two |= one & num
            one ^= num

            no_three = ~(one & two)
            one = one & no_three
            two = two & no_three

        return one


统计某位个数法：

class Solution:
    def singleNumber(self, nums: List[int]) -> int:

        num_count = [0 for i in range(32)]
        for num in nums:
            index = 0
            while num:
                if num & 1:
                    num_count[index] += 1
                index +=1
                num = num >> 1
        one = 1
        ret = 0
        for num in num_count:
            if num % 3 == 1:
                ret |= one
            one = one << 1

        return ret	
```

python的位运算相关

	& 按位与

	| 按位或

	^ 位异或

	~ 按位取发

	<< 左移
	
	>> 右移

**幂运算**

#### offer 14 减绳子

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        
        
        if n <=3: 
            return  n - 1
        
        a = n // 3
        b = n % 3
        if b == 0:
            return int(math.pow(3,a))   
        if b == 1:
            return int(math.pow(3,a-1)*4)
        
        return int(math.pow(3,a)*b)
``` 
 
notes: 

求幂 math.pow()

枚举归纳法

#### offer 16 数值的整数次方

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        
        if n < 0:
            x = 1/x
            n = -n
        
        result = 1.0
        while n:
            if n&1:
                result = result * x
            n = n >> 1
            x = x * x
        
        return result  
```

快速幂解析（二分法角度）

移位 + 快速幂

#### offer 17 打印从1到最大的n位数

```python
class Solution:
    def printNumbers(self, n: int) -> List[int]:
        res = []
        for i in range(1, 10 ** n):
            res.append(i)
        return res
```

**  乘方


#### Offer 66. 构建乘积数组

```python

class Solution:
    def constructArr(self, a: List[int]) -> List[int]:
        b = [1] * len(a)
        tmp = 1

        for i in range(1,len(a)):
            b[i] *= b[i-1] * a[i - 1]
        for i in range(len(a)-2,-1,-1):
            tmp *= a[i+1]
            b[i] *= tmp
        return b
```

表格分区、上下三角

#### Offer 61. 扑克牌中的顺子

```python

class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        
        nums.sort()
        joker = 0
        for i in range(4):
            if nums[i] == 0:
                joker += 1
            elif nums[i] == nums[i+1]:
                return False
        return nums[4] - nums[joker] < 5
```

先排序，再逐步排除，理解这里 < 5 


**字符串问题 **

#### offer 20  表示数值的字符串

```python
class Solution:
    def isNumber(self, s: str) -> bool:
        
        n = len(s)
        index = 0
        has_num = has_e = has_dot = has_sign = False
        while index < n and  s[index]== ' ':
            index = index + 1
        while index < n:
            while (index < n and '0' <= s[index] <= '9'):
                index = index + 1
                has_num = True
            if index == n:
                break
            
            if s[index] == 'e' or s[index] == 'E':
                if has_e or not has_num:
                    return False
                has_e = True
                has_num = has_dot = has_sign = False
            elif s[index] == '+' or s[index] == '-':
                if has_num or has_dot or has_sign:
                    return False
                has_sign = True
            elif s[index] == '.':
                if has_dot or has_e:
                    return False
                has_dot = True
            elif s[index] == ' ':
                break
            else:
                return False
            index = index + 1
            
        while index < n and s[index] == ' ':
            index = index + 1
        return has_num and index == n 
```

bool_after_dot = (len(s[index+1:]) >= 1) and ('0' <= s[index + 1] <= '9') 

bool 条件，兼容判断长度

bool 标记法


#### Offer 67. 把字符串转换成整数

ord()  相当于char(),获取ascii值

ord(c) - ord(0)

字符串、数字问题

```python

class Solution:
    def strToInt(self, str: str) -> int:
        if not str: return 0
        index = 0
        while str[index] == ' ':
            index += 1
            if index == len(str):
                return 0
        
        sign = 1
        if str[index] == '-':
            sign = -1
            index += 1
        elif str[index] == '+':
            index += 1

        
        #python3 中使用  sys.maxsize 作为int 最大值
        #有效范围： 2147483647
        int_max = 2**31 -1
        int_min = -2**31
        boundary = 2**31 // 10
        
        num = 0
        #break前置
        while index < len(str):
            if str[index] < '0' or str[index] >'9':
                break
            if num > boundary or (num == boundary and str[index] > '7'):
                if sign == -1:
                    return int_min
                else:
                    return int_max
            else:
                num = num * 10 + ord(str[index]) - ord('0')
                index += 1
        
        return num*sign
```

#### Offer 51. 数组中的逆序对

归并排序：分开 --> 合并

合并阶段：每当左子数组元素 > 右子数组元素时，[左子数字当前元素-末尾元素] 与右子数组当前元素

组成若干逆序对

归并排序加一行count

```python

class Solution:
    def __init__(self):
        self.count = 0
    def reversePairs(self, nums: List[int]) -> int:

        def mergeSort(nums,left,right):
            if left >= right:
                return
            mid = left + (right - left)// 2
            mergeSort(nums,left,mid)
            mergeSort(nums,mid+1,right)
            merge(nums,left,mid,right)       

        def merge(nums,left,mid,right):

            i = left 
            j = mid + 1
            tmp_nums = []
            while i <= mid  and j <= right:
                if nums[i] <= nums[j]:
                    tmp_nums.append(nums[i])
                    i += 1
                else:
                    tmp_nums.append(nums[j])
                    j += 1
                    self.count += mid - i + 1
            
            # extend 扩展
            # append 会形成[ [  ]]
            if i > mid and j <= right:
                tmp_nums.extend(nums[j:right+1])
            if i <= mid and j > right:
                tmp_nums.extend(nums[i:mid+1])
            
            nums[left:right+1] = tmp_nums


        mergeSort(nums,0,len(nums) - 1)
        return self.count
```

####  Offer 29. 顺时针打印矩阵

left 、right、top、bottom 四边缩进画图看

```python

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix: return []
        l,r,t,b = 0,len(matrix[0]) - 1,0,len(matrix) - 1 
        res = []
        while True:
            for i in range(l,r+1):
                res.append(matrix[t][i])
            t += 1
            if t > b:break
            for i in range(t,b+1):
                res.append(matrix[i][r])
            r -= 1
            if l > r:break
            for i in range(r,l-1,-1):
                res.append(matrix[b][i])
            b -= 1
            if t > b:break
            for i in range(b,t-1,-1):
                res.append(matrix[i][l])
            l += 1
            if l > r:
                break

        return res
```
