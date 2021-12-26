###   1 线性表（数组、链表、字符串）

#### 链表基本操作

##### 2. 两数相加

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        head = None
        tail = None
        carry = 0 
        while l1 or l2:
            l1_val = l1.val if l1 else 0 
            l2_val = l2.val if l2 else 0
            sum = l1_val + l2_val + carry
            carry = sum // 10
            if not head:
                tail = ListNode(sum % 10)
                head = tail
            else:
                tail.next = ListNode(sum % 10)
                tail = tail.next
            
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
        if carry:
            tail.next = ListNode(carry)
        
        return head
```


#####  25. K 个一组翻转链表

```python
	
class Solution:
    def reverse(self, head: ListNode,end: ListNode) -> ListNode:
        if not head:
            return None
        pre = None 
        cur = head
        while cur != end:
            nxt = cur.next
            cur.next = pre
            pre = cur 
            cur = nxt
        return pre

    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if not head:
            return None

        a = head 
        b = head
        for i in range(k):
            if not b:
                return head
            b = b.next

        last = self.reverse(a,b)
        head.next = self.reverseKGroup(b,k)
        return last
```


思考顺序：

ListNode reverse(ListNode a) --> ListNode reverse(ListNode a, ListNode b) -->  

	ListNode reverseKGroup(ListNode head, int k)






##### 148. 排序链表

```python

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head

        middle = self.findMiddle(head)
        head2 = middle.next
        middle.next = None
        L1 = self.sortList(head)
        L2 = self.sortList(head2)

        return self.merge(L1,L2)

    def findMiddle(self,head:ListNode):
        if not head:
            return head

        slow = head
        fast = head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def merge(self,L1:ListNode,L2:ListNode):

        dummy = ListNode(0)
        head = dummy
        while L1 and L2:
            if L1.val < L2.val:
                head.next = L1
                L1 = L1.next
            else:
                head.next = L2
                L2 = L2.next
            head = head.next

        if L1:head.next = L1
        if L2:head.next = L2
        return dummy.next

```

##### 160. 相交链表
```python

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if not headA or not headB:
            return None

        nodeA = headA
        nodeB = headB
		#要么都走完，要么相遇
        while nodeA != nodeB:
            if not nodeA:
                nodeA = headB
            else:
                nodeA = nodeA.next
            if not nodeB:
                nodeB = headA
            else:
                nodeB = nodeB.next
        return nodeA
```

#### 链表翻转

##### 206 反转链表

________
 A        |
pre ---- cur ---- next 
|_________B _______ C


1  nxt 保存

2  cur.next 指向pre 

3  pre = cur

4  cur = nxt

```python
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        if not pHead:
            return 
        pre = None
        cur = pHead
        while cur:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        return pre
```


#### 数组类问题

##### 448. 找到所有数组中消失的数字

```python
	
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        
        n = len(nums)
        for num in nums:
            x = (num -1) % n
            nums[x] += n
        res = []
        for k in range(len(nums)):
            if nums[k] <= n:
                res.append(k+1)
        
        return res
```



##### 48. 旋转图像

旋转问题

```python

class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        m = len(matrix)
        n = len(matrix[0])
        #上下对换
        for i in range(m//2):
            for j in range(n):
                matrix[i][j],matrix[m - i - 1][j] = matrix[m - i - 1][j],matrix[i][j]
        #对角线对换
        for i in range(m):
            for j in range(i):
                matrix[i][j],matrix[j][i] = matrix[j][i],matrix[i][j] 
        return matrix
```

##### 560. 和为 K 的子数组

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

##### 169.  多数元素

```python

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        #投票法
        canda = nums[0]
        count = 1
        for num in nums[1:]:
            if count == 0:
                canda = num
            if num == canda:
                count += 1
            else:
                count -= 1
        return canda
```



#### 前缀和与差分数组
 
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

#### 字符串操作


### 2 栈与队列（堆）

括号类问题、单调栈问题

#### 栈基本操作


##### 20.有效的括号

```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 == 1:
            return False

        stack = []
        map = {')':'(','}':'{',']':'['}        
        for ch in s:
            if ch in map:
                if not stack or stack[-1] != map[ch]:
                    return False
                stack.pop()
            else:
                stack.append(ch)

        return len(stack) == 0

```


##### 32. 最长有效括号

括号类问题

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = []
        max_ans = 0
        stack.append(-1)
        for i in range(len(s)):
            if s[i] == '(':
                stack.append(i)
            else:
                stack.pop()
                if len(stack) == 0:
                    stack.append(i)
                else:
                    index = stack[-1]
                    max_ans = max(max_ans,i - index)
        return max_ans
```

##### 394. 字符串解码

```
class Solution:
    def decodeString(self, s: str) -> str:
        res = ''
        stack = []
        mul = 0
        for c in s:
            if c == "[":
                stack.append([mul,res])
                mul = 0
                res = ''
            elif c == "]":
                cur_mul,last_res = stack.pop()
                res = last_res + cur_mul * res
            elif  '0' <= c and c <= '9':
                mul = mul * 10 + int(c)
            else:
                res += c
            
        return res
```

#### 单调栈

##### 单调栈  84. 柱状图中最大的矩形    矩形面积


```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        left = [0 for i in range(n)]
        right = [0 for i in range(n)]

        left_stack = []
        for i in range(n):
            while left_stack and heights[left_stack[-1]] >= heights[i]:
                left_stack.pop(-1)
            left[i] = left_stack[-1] if left_stack else -1
            left_stack.append(i)
        
        right_stack = []
        for j in range(n-1,-1,-1):
            while right_stack and heights[right_stack[-1]] >= heights[j]:
                right_stack.pop(-1)
            right[j] = right_stack[-1] if right_stack else n
            right_stack.append(j)

        ret_max  = 0
        for i in range(n):
            ret_max = max(ret_max,(right[i]-left[i] - 1)*heights[i])   
        return ret_max   
```

##### 85. 最大矩形

```python
class Solution:
    def largestRectangleArea(self,heights):
        n = len(heights)
        mono_stack = []
        left = [0 for i in range(n)]
        right = [0 for i in range(n)]
        for i in range(n):
            while mono_stack and heights[mono_stack[-1]] >= heights[i]:
                mono_stack.pop(-1)
            left[i] = mono_stack[-1] if mono_stack else -1
            mono_stack.append(i)
        
        mono_stack = []
        for j in range(n-1,-1,-1):
            while mono_stack and heights[mono_stack[-1]] >= heights[j]:
                mono_stack.pop(-1)
            right[j] = mono_stack[-1] if mono_stack else n
            mono_stack.append(j)
        
        maxArea = 0
        for i in range(n):
            area = (right[i] - left[i] - 1) * heights[i]
            maxArea = max(maxArea,area)                
        return maxArea
    
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        m = len(matrix)
        n = len(matrix[0])
        heights = [0 for i in range(n)]
        maxArea = 0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == '0':
                    heights[j] = 0
                else:
                    heights[j] += int(matrix[i][j])
            
            largeArea = self.largestRectangleArea(heights)                
            maxArea = max(maxArea,largeArea)
        return maxArea
```

接雨水：左边最大、右边最大，min(leftmax,rightMax）- height[i]。 矩形： (右边小于index - 左边小于index  - 1) * height[i]

##### 155. 最小栈

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if self.min:
            self.min.append(min(val,self.min[-1]))
        else:
            self.min.append(val)

    def pop(self) -> None:
        self.stack.pop(-1)
        self.min.pop(-1)

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min[-1]
```

##### 739. 每日温度

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack = []
        res = [0 for i in range(len(temperatures))]
        for i  in range(len(temperatures)-1,-1,-1):
            while stack and temperatures[stack[-1]] <= temperatures[i]:
                stack.pop(-1)
            if stack:
                res[i] = stack[-1] - i
            
            stack.append(i)

        return res
```

#### 单调队列


#### ToP k问题

##### 347. 前 K 个高频元素

堆处理海量数据的 topK，分位数非常合适，优先队列应用在元素优先级排序

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        map = dict()
        for num in nums:
            map[num] = map.get(num,0) + 1
        
        # 堆的用法
        pq = []
        for key,value in map.items():
            if len(pq) < k:
                heapq.heappush(pq,(value,key))
            elif value > pq[0][0]:
                #删除最小的item,同时添加一个新的item
                heapq.heapreplace(pq,(value,key))

        res = []
        for i in range(len(pq)):
            res.append(pq[i][1])
        return res
```

#####  406. 根据身高重建队列

数对问题：第一个元素正向/反向排序，第二个元素反向/正向排序

```python
	
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        res = []
        people = sorted(people,key = lambda x: (-x[0],x[1]))

        for p in people:
            if len(res) < p[1]:
                res.append(p)
            else:
                res.insert(p[1],p)
        return res	

```

#### K-way merge，多路归并

##### 23. 合并K个升序链表

关于虚拟节点(链表类题目)

dummy = ListNode(0)
dummy.next = head  //合并，则用新链表 head = dummy

return dummy.next 

```python

class Solution:
    def mergeTwoLists(self, list1:ListNode,list2:ListNode) -> ListNode:
        
        head1 = list1
        head2 = list2
        dummy = ListNode(0)
        head = dummy
        while head1 and head2:
            if head1.val < head2.val:
                head.next = head1
                head1 = head1.next  
            else:
                head.next = head2
                head2 = head2.next
            head = head.next
        
        if head1:
            head.next = head1
        if head2:
            head.next = head2

        return dummy.next

    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        if not lists:
            return 

        list1 = lists[0]
        for k in range(1,len(lists)):
            list1 = self.mergeTwoLists(list1,lists[k])
        return list1
```


### 3 树 

#### 树的DFS（Tree Depth First Search，stack）

##### 236. 二叉树的最近公共祖先

面向用例编程

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return 
        if root.val == p.val or root.val == q.val:
            return root
        
        left = self.lowestCommonAncestor(root.left,p,q)
        right = self.lowestCommonAncestor(root.right,p,q)
        
		#注意判断的顺序
        if not left: return right
        if not right: return left
        
        return root

```

##### 94. 二叉树的中序遍历


```python
class Solution:
    
    def threeOrders(self , root):
        def preorder(root):
            if not root:
                return []
            res1.append(root.val)
            preorder(root.left)
            preorder(root.right)
        
        def inorder(root):
            if not root:
                return []
            inorder(root.left)
            res2.append(root.val)
            inorder(root.right)
        def postorder(root):
            if not root:
                return []
            postorder(root.left)
            postorder(root.right)
            res3.append(root.val)
        
        res = []
        res1 = []
        res2 = []
        res3 = []
        preorder(root)
        inorder(root)
        postorder(root)
        res.append(res1)
        res.append(res2)
        res.append(res3)
```

##### 96. 不同的二叉搜索树

```python
class Solution:
    def numTrees(self, n: int) -> int:
        G = [0 for i in range(n + 1)]
        G[0] = 1
        G[1] = 1
        for i in range(2,n+1):
            for j in range(1,i+1):
                G[i] += G[j-1] * G[i-j]

        return G[n]
```

##### 98. 验证二叉搜索树

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

##### 101. 对称二叉树

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def recur(left,right):
            if not left and not right:
                return True
            if not left or not right:
                return False
            if left.val != right.val:
                return False
            return recur(left.left,right.right) and recur(left.right,right.left)

        return recur(root.left,root.right)

```


##### 104. 二叉树的最大深度

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        left_max = self.maxDepth(root.left)
        right_max = self.maxDepth(root.right)
        return max(left_max,right_max) + 1
```


##### 105. 从前序与中序遍历序列构造二叉树

```python

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder:
            return None
        
        root = TreeNode(preorder[0])
        index = 0
        while inorder[index]!= preorder[0]:
            index += 1
        root.left = self.buildTree(preorder[1:index+1],inorder[:index])
        root.right = self.buildTree(preorder[index+1:],inorder[index+1:])

        return root
```

##### 114. 二叉树展开为链表


```python
class Solution:
    def flatten(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if not root:
            return None
        self.flatten(root.left)
        right = root.right
        root.right = root.left
        root.left = None
        while root.right:
            root = root.right
        self.flatten(right)
        root.right = right

        return 

```
##### 226. 翻转二叉树

```python

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        left = self.invertTree(root.right)
        right = self.invertTree(root.left)
        root.left = left
        root.right = right

        return root 

```

##### 617. 合并二叉树

```python
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:

        if not root1:
            return root2
        if not root2:
            return root1
        
        mergeNode = TreeNode(root1.val + root2.val)
        mergeNode.left = self.mergeTrees(root1.left,root2.left)
        mergeNode.right = self.mergeTrees(root1.right,root2.right)

        return mergeNode
```

##### 543. 二叉树的直径

python  闭包的全局变量的取法

self.ans，或者list传引用

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.ans = 0
        def depth(root):
            if not root:
                return 0
            L = depth(root.left)
            R = depth(root.right)
            self.ans = max(self.ans,L + R + 1)

            return max(L,R) + 1

        depth(root)
        return self.ans - 1
```
##### 538. 把二叉搜索树转换为累加树

树的遍历与中序遍历；

https://leetcode-cn.com/problems/convert-bst-to-greater-tree/solution/yi-tao-quan-fa-shua-diao-nge-bian-li-shu-de-wen-5/

```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.total = 0
        def dfs(root):
            if not root:
                return 0
            dfs(root.right)
            self.total += root.val
            root.val = self.total
            dfs(root.left)

        dfs(root)
        return root
```
##### 124. 二叉树中的最大路径和

路径和计算方法

```python
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        self.maxSum = -float('inf')
        def maxGain(root):
            if not root:
                return 0
            
            rightsum = max(maxGain(root.right),0)
            leftsum = max(maxGain(root.left),0)

            maxValue = rightsum + leftsum + root.val
            self.maxSum = max(self.maxSum,maxValue)

            return root.val + max(rightsum,leftsum)
            
        maxGain(root)
        return self.maxSum
```


##### 297  二叉树的序列化与反序列化

```python

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:
            return "None,"
        else:
            val = str(root.val)
            left = self.serialize(root.left)
            right = self.serialize(root.right)
            return val + ',' + left + right

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        array = data.split(',')
        root = self.buildTree(array)
        return root    
    
    def buildTree(self,array):
        if array[0] == 'None':
            array.pop(0)
            return None
        
        root = TreeNode(int(array[0]))
        array.pop(0)
        root.left = self.buildTree(array)
        root.right = self.buildTree(array)

        return root
```



#### 树的BFS(Tree Breadth First Search，queue)

##### 102. 二叉树的层序遍历

BFS问题

```python

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        queue = []
        res = []
        queue.append(root)
        while queue:
            size = len(queue)
            lis = []
            for i in range(size):
                node = queue.pop(0)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                lis.append(node.val)
            res.append(lis)    

        return res    
```

#### 前缀树（字典树）

self使用：类中函数的第一个参数是实例对象本身，并且约定俗成，把其名字写为self。其作用相当于java中的this

字符串处理ord

本质是一个26节点的树

###### 208. 实现 Trie (前缀树)

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
	//返回node
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
        return self.searchPrefix(prefix) is not None

```


### 4 动态规划（DFS\DP）

重叠子问题、最优子结构

#### 经典动归

字符串（编辑距离、正则）问题、子序列问题、路径问题

##### 10 正则表达式匹配

```python

class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        m = len(s)
        n = len(p)
        # dp[i][j]    s[0,i - 1] match p[0,j - 1]
        dp = [[False for i in range(n + 1)] for j in range(m + 1)]
        dp[0][0] = True
        for j in range(1,n + 1):
            if p[j - 1] == '*':
                dp[0][j] |= dp[0][j - 2]
                
        for i in range(1,m + 1):
            for j in range(1,n + 1):
                if s[i - 1] == p[j - 1] or p[j - 1] == '.':
                    dp[i][j] |= dp[i - 1][j - 1]
                
                if p[j - 1] == "*":
                    dp[i][j] |= dp[i][j - 2]
                    if s[i - 1] == p[j -2] or p[j - 2] == '.':
                        dp[i][j] |= dp[i - 1][j]                       
        return dp[m][n]
```

#####  72. 编辑距离

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)
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

##### 139. 单词拆分

```python
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # dp[i] 代表dp[i-1] 能否被拆分

        dp = [False for i in range(len(s)+1)]
        dp[0] = True
        for i in range(len(s) + 1):
            for j in range(i):
                if dp[j] and s[j:i] in wordDict:
                    dp[i] = True

        return dp[len(s)]
```

##### 70. 爬楼梯
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [0 for i in range(n+1)]
        dp[0] = 1
        dp[1] = 1
        for i in range(2,n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
```

##### 300. 最长递增子序列

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


##### 62. 不同路径

```python

    def uniquePaths(self, m: int, n: int) -> int:
        #dp[m][n]的含义
        dp = [[0 for i in range(n)] for i in range(m)]
        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]

        return dp[m-1][n-1]

```
##### 64 最小路径和

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return 0
        m = len(grid)
        n = len(grid[0])
        dp = [[0 for i in range(n)] for i in range(m)]
        dp[0][0] = grid[0][0]
        for i in range(1,m):
            dp[i][0] = dp[i-1][0] + grid[i][0]
        for j in range(1,n):
            dp[0][j] = dp[0][j-1] + grid[0][j]
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + grid[i][j]
        
        return dp[m-1][n-1]
```


##### 53 最大子序和

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:

        dp = [0 for i in range(len(nums))]
        dp[0] = nums[0]
        result = nums[0]
        for i in range(1,len(nums)):
			#注意状态转移方程
            dp[i] = max(dp[i-1] + nums[i],nums[i])
            result = max(result,dp[i])
        return result
```
##### 152. 乘积最大子数组

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        maxF = [0 for i in range(len(nums))]
        minF = [0 for j in range(len(nums))]

        maxF[0] = nums[0]
        minF[0] = nums[0]
        ans = nums[0]
        for i in range(1,len(nums)):
            maxF[i] = max(maxF[i-1]*nums[i],minF[i-1]*nums[i],nums[i])
            minF[i] = min(maxF[i-1]*nums[i],minF[i-1]*nums[i],nums[i])
            ans = max(ans,maxF[i])

        return ans
```

##### 55. 跳跃游戏

```python

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        
        farest = 0
        for i in range(len(nums)- 1):
            farest = max(farest,i + nums[i])
            if farest <=i:
                return False
            
        return farest >= len(nums) - 1
```

##### 128. 最长连续序列

```python

class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0
        map = dict()
        for num in nums:
            map[num] = 1
        max_len = 1
        for num in nums:
            if num - 1 not in map:
                nums_len = 1
                num += 1
                while num in map:
                    nums_len += 1
                    num += 1
                max_len = max(max_len,nums_len)            

        return max_len
```

##### 221. 最大正方形

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if len(matrix)==0 or len(matrix[0])==0 :
            return 0
        m = len(matrix)
        n = len(matrix[0])
        #正方形右下角 i j点的边长
        dp = [[0 for i in range(n)] for i in range(m)]

        maxside = 0
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == '1':
                    if i == 0 or j == 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = min(dp[i-1][j],dp[i-1][j-1],dp[i][j-1]) + 1
                    maxside = max(maxside,dp[i][j])
                
        return maxside * maxside
```

##### 238. 除自身以外数组的乘积

```python

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        answer = [0 for i in range(len(nums))]
        answer[0] = 1
        for i in range(1,len(nums)):
            answer[i] = answer[i-1]*nums[i-1]
        
        Rmul = 1
        for i in range(len(nums)-1,-1,-1):
            answer[i] = answer[i] * Rmul
            Rmul = Rmul * nums[i]

        return answer
```


##### 312. 戳气球

```python
class Solution:
    #dp:子问题必须独立
    #dp: i 和 j 的遍历顺序，可以根据base case和最终状态进行推导
    #dp[i][j]:戳破气球 i 和气球 j 之间（开区间，不包括 i 和 j）的所有气球，得到的最高分数
    #dp[i][j] = dp[i][k]+dp[k][j]+ nums[i]*nums[k]*nums[j]
    #这里是斜着 走上三角 进行迭代
    def maxCoins(self, nums: List[int]) -> int:
        #添加虚拟气球
        n = len(nums)
        points = [0 for i in range(n+2)]
        dp = [[0 for i in range(n+2)] for j in range(n+2)]
        #添加辅助气球
        points[0] = 1
        points[n+1] = 1
        for i in range(1,n+1):
            points[i] = nums[i-1]
        for i in range(n-1,-1,-1):
            for j in range(i+1,n+2,1):
                for k in range(i+1,j,1):      
                    dp[i][j] = max(dp[i][j],dp[i][k] + dp[k][j] + points[i]*points[k]*points[j])        
        
        return dp[0][n+1]
```

#### 背包问题

0-1背包

完全背包

##### 279. 完全平方数

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [0 for i in range(n+1)]
        for i in range(n+1):
            dp[i] = i 
            j = 0
            while j * j <= i:
                dp[i] = min(dp[i],dp[i- j*j]+ 1)
                j+=1
        
        return dp[n]
		
```

##### 322. 零钱兑换

完全背包问题

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf') for i in range(amount + 1)]
        dp[0] = 0
        for coin in coins:
            for i in range(coin,amount + 1):
                dp[i] = min(dp[i],dp[i-coin] + 1)

        return dp[amount] if dp[amount] <= amount else -1

```
##### 416. 分割等和子集

0-1背包问题

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


#### 股票问题

##### 121. 买卖股票的最佳时机

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
##### 309. 最佳买卖股票时机含冷冻期

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

#### 打家劫舍问题

##### 198. 打家劫舍

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

##### 337. 打家劫舍 III

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
#### 贪心算法

##### 56. 合并区间

区间类问题

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

##### 435. 无重叠区间

###  5 回溯


子集、排列、组合、floodfill回溯

二叉、多叉


括号组合问题


##### 17. 电话号码的字母组合

```python

class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []

        map ={
        '2':'abc',
        '3':'def',
        '4':'ghi',
        '5':'jkl',
        '6':'mno',
        '7':'pqrs',
        '8':'tuv',
        '9':'wxyz'
        }

        res = []

        def backtrack(digits,index,combine):
            if index == len(digits):
                res.append(''.join(combine))
                return
            
            string = map[digits[index]]
            for i in range(len(string)):
                combine.append(string[i])
                backtrack(digits,index+1,combine)
                combine.pop(-1)
            
        index = 0 
        combine = []
        backtrack(digits,index,combine)

        return res
```


##### 301 删除无效的括号

理解DSAPP栈与括号部分

单种括号计数可以，多种需要使用栈

去重复,判断前后重复，要从start开始

多叉递归回溯 + 剪枝 远快于   <  双分支回溯


```python

class Solution:
    def __init__(self):
        self.res = []

    def isValid(self,s):
        cnt = 0
        #理解DSAPP栈与括号部分
        #单种括号计数可以，多种需要使用栈
        for c in  s:
            if c == '(':
                cnt += 1
            elif c == ')':
                cnt -= 1
            if cnt < 0:
                return False
        
        return True


    def dfs(self,s,start,left_remove,right_remove):
        if left_remove == 0 and right_remove == 0:
            if self.isValid(s):
                self.res.append(s)
            return

        #从start开始，避免)(f重复
        for i in range(start,len(s)):
            #去重复
            if i != start and s[i-1] == s[i]:
                continue
            if left_remove + right_remove > len(s) - i :
                return

            if left_remove > 0 and s[i] == '(':
                self.dfs(s[:i] + s[i+1:],i,left_remove - 1,right_remove)
            elif right_remove > 0 and s[i] == ')':
                self.dfs(s[:i] + s[i+1:],i,left_remove,right_remove -1)
    
        return


    def removeInvalidParentheses(self, s: str) -> List[str]:
        left_remove = 0
        right_remove = 0
        for i in range(len(s)):
            if s[i] == '(':
                left_remove += 1
            elif s[i] == ')':
                if left_remove != 0:
                    left_remove -= 1
                else:
                    right_remove += 1
        
        self.dfs(s,0,left_remove,right_remove)

        return self.res
```



#### 子集问题（排列、组合）

##### 46. 全排列

1  全局visit[bool] 函数

2  swap动态交换，模仿组合排列过程

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

```python

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        
        def backtrack(first):
            if first == len(nums):
                ans.append(list(nums)) #nums[:]  list需要拷贝或者切片
                 

            for i in range(first,len(nums)):
                nums[first],nums[i] = nums[i],nums[first]
                backtrack(first + 1)
                nums[first],nums[i] = nums[i],nums[first]

        ans = []
        backtrack(0)

        return ans

```

##### 39. 组合总和

背包问题，求全部路径的回溯解法

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        
        def backtrack(candidates,target,path,idx):

            if idx == len(candidates):
                return
            if target == 0:
                res.append(list(path))
                return

            backtrack(candidates,target,path,idx+1)

            if target >= candidates[idx]:
                path.append(candidates[idx])
                backtrack(candidates,target - candidates[idx],path,idx)
                path.pop(-1)

        res = []
        path = []
        idx = 0
        backtrack(candidates,target,path,idx)
        return res
```
##### 78. 子集

```python

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
		

#### floodfill问题

##### 79. 单词搜索

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
        visited = set()
        selection = [(0,1),(0,-1),(1,0),(-1,0)] 
        def backtrack(i,j,k):
            if board[i][j] != word[k]:
                return False
            if k == len(word) - 1:
                return True

            visited.add((i,j))
            for sel in selection:
                di,dj = sel
                newi = i + di
                newj = j + dj
                if newi < 0 or newi >= len(board) or newj < 0 or newj >= len(board[0]):
                    continue
                if (newi,newj) in visited:
                    continue
                    
                if backtrack(newi,newj,k+1):
                    return True
            visited.remove((i,j))

            return False

        m = len(board)
        n = len(board[0])
        for i in range(m):
            for j in range(n):
                if backtrack(i,j,0):
                    return True
        return False

```

##### 200. 岛屿数量

flood fill问题

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

			
### 6 BFS

#####  层序遍历

BFS:  queue + while + for 


```python
class Solution:
    def levelOrder(self, root):
        if not root:
            return []
        queue = []
        res = []
        queue.append(root)
        while queue:
            size = len(queue)
            lis = []
            for i in range(size):
                node = queue.pop(0)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                lis.append(node.val)
            res.append(lis)    

        return res
```


###  7 双指针

快慢指针、左右指针、双向遍历、回文问题、归并排序、三指针

##### 141. 环形链表

链表有环问题判断

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:

        slow = head
        fast = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True

        return Fal
```

##### 142. 环形链表 II

```python
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
        
        slow = head
        while fast != slow:
            fast = fast.next
            slow = slow.next
        return slow
```

##### 19. 删除链表的倒数第 N 个结点 

快慢指针

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        
        dummy = ListNode(0)
        dummy.next = head
        fast = head
        slow = dummy
        for i in range(n):
            fast = fast.next 

        while fast:
            slow = slow.next
            fast = fast.next

        slow.next = slow.next.next

        return dummy.next
```

##### 11. 盛最多水的容器

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l = 0 
        r = len(height) -1
        max_area = 0
        while l < r:
            area =  min(height[l],height[r]) * (r - l)
            max_area = max(max_area,area)
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        
        return max_area
```

##### 15. 三数之和

三指针

```python

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        if not nums or n < 3:
            return []
        
        ans = []
        nums.sort()
        for i in range(n):
            if nums[i] > 0 :
                continue
            if i > 0 and nums[i] == nums[i-1]:
                continue
            L = i + 1
            R = n - 1
            while L < R:
                if nums[i] + nums[L] + nums[R] == 0:
                    ans.append([nums[i],nums[L],nums[R]])
                    while L+1 < R and nums[L] == nums[L+1]:
                        L += 1
                    while R-1 > L and nums[R] == nums[R-1]:
                        R -= 1
                    L += 1
                    R -= 1
                elif nums[i] + nums[L] + nums[R] < 0:
                    L += 1
                else:
                    R -= 1
        return ans
```

##### 75. 颜色分类

荷兰国旗问题

```python

class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        p0 = 0
        p2 = n - 1
        i = 0
        #荷兰国旗问题
        while i <= p2:
            #防止换过来的还是2
            while i <= p2 and nums[i] == 2:
                nums[p2],nums[i] = nums[i],nums[p2]
                p2 -= 1
            if nums[i] == 0:
                nums[p0],nums[i] = nums[i],nums[p0]
                p0 += 1
            i += 1
        
        return 
```

##### 3 最长无重复子数组		

```python
	
class Solution:
    def maxLength(self , arr ):
        
        map = dict()
        left = 0 
        right = 0
        maxlen = 0
        while right < len(arr):
            num_right = arr[right]
            right += 1
            map[num_right] = map.get(num_right,0) + 1
            
            while map[num_right] > 1:
                num_left = arr[left]
                map[num_left] -= 1
                left += 1
            maxlen = max(maxlen,right - left)

        return maxlen 
	
```

##### 283. 移动零

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        left = 0
        right = 0
        while right < len(nums):
            if nums[right] != 0:
                nums[right],nums[left] = nums[left],nums[right]
                left += 1
            right += 1
        
        return
```




##### 4. 寻找两个正序数组的中位数

类似归并排序思路

```python

class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        
        def getKelement(nums1,nums2,k):
            m = len(nums1)
            n = len(nums2)
            index1 = 0
            index2 = 0 
            while True:
                if index1 == m:
                    return nums2[index2 + k - 1]
                if index2 == n:
                    return nums1[index1 + k - 1]
                if k == 1:
                    return min(nums1[index1],nums2[index2])
                
                newIndex1 = min(index1 + k//2 -1,m - 1)
                newIndex2 = min(index2 + k//2 -1,n - 1)
                if nums1[newIndex1] < nums2[newIndex2]:
                    k -= newIndex1 - index1 + 1
                    index1 = newIndex1 + 1
                else:
                    k -= newIndex2 - index2 + 1
                    index2 = newIndex2 + 1

        m = len(nums1)
        n = len(nums2)
        sum = m + n 
        if sum % 2 == 1:
            return getKelement(nums1, nums2, sum//2 + 1)
        else:
            return (getKelement(nums1, nums2, sum//2) + getKelement(nums1, nums2, sum//2 + 1))/2.0

```

##### 31. 下一个排列

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        i = n - 2
        j = n - 1
        k = n - 1
        while i >= 0 and nums[i] >= nums[j]:
            i -= 1
            j -= 1
        if i >= 0:
            while k >= 0 and nums[i] >= nums[k]:
                k -= 1
            nums[i],nums[k] = nums[k],nums[i]
        
        index1 = n - 1
        index2 = j
        while index2 < index1:
            nums[index1],nums[index2] = nums[index2],nums[index1]
            index1 -= 1
            index2 += 1
        return
```


##### 215. 数组中的第K个最大元素

快速排序

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def quick_partition(arr,l,h):
            pivot = arr[l]
            i = l
            j = h
            while i < j:
                while arr[j] >= pivot and j > i:
                    j -= 1
                while arr[i] <= pivot and i < j:
                    i += 1
                arr[i],arr[j] = arr[j],arr[i]
            
            arr[l],arr[i] = arr[i],arr[l]
            return i

        def quick_sort(arr,l,h):
            if  l > h:
                return 
            index = quick_partition(arr,l,h)
            quick_sort(arr,l,index - 1)
            quick_sort(arr,index + 1,h)

        l = 0
        h = len(nums) - 1
        quick_sort(nums,l,h)
        return nums[-k]

```


#### 双向遍历

##### 42. 接雨水

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

##### 581. 最短无序连续子数组

双指针+ 双向遍历

双向遍历：柱形面积、接雨水，左边一遍，右边一遍

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        #双指针+双向遍历
        left = 0
        right = 0
        max = -1000
        min = 1000
        for i in range(len(nums)):
            if nums[i] >= max:
                max = nums[i]
            else:
                right = i

        for i in range(right,-1,-1):
            if nums[i] <= min:
                min = nums[i]
            else:
                left = i
        
        return 0 if right == left else right - left +1
```

#### 回文问题

##### 5. 最长回文子串

寻找回文串是从中间向两端扩展，判断回文串是从两端向中间收缩
 
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
            max_str = str1 if len(str1) > len(str2) else str2
            res = res if len(res) > len(max_str) else max_str

            index += 1
        
        return res
```

##### 647. 回文子串

字符串中 回文子串 的数目

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        slen = len(s) 
        num = 2 * slen - 1
        left = 0
        right = 0
        ans = 0 
        for center in range(num):
            left = center // 2 
            right = left + center % 2
            
            while left >= 0 and right < len(s) and  s[left] == s[right]:
                ans += 1
                left -= 1
                right += 1
        return ans 
```

##### 234. 回文链表

```python

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


### 8 二分查找

##### 33. 搜索旋转排序数组

```python

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums)
        left = 0 
        right = n - 1
        while left <= right:
            mid = left + (right - left) //2
            if nums[mid] == target:
                return mid 
				
            #始终去搜索基本有序的数组
            if nums[0] <= nums[mid]:
                if nums[0] <= target and target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target and target <= nums[n - 1]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1
```


##### 34. 在排序数组中查找元素的第一个和最后一个位置

二分查找，查找左右边界

```python

class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def searchleft(target) -> List[int]:
            left = 0 
            right = len(nums) -1
            while left <= right:
                mid = left + (right - left)//2
                if nums[mid] > target:
                    right = mid - 1
                elif nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1

            return left
        
        if not nums:
            return [-1,-1]
        res = []
        left = searchleft(target)
        if left < len(nums) and nums[left] == target:
            right = searchleft(target + 1)
            res.append(left)
            res.append(right - 1)
        else:
            res = [-1,-1]
        return res
```

##### 240. 搜索二维矩阵 II

​```python

class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])

        i = 0
        j = n - 1
        while i < m and j >= 0:
            if matrix[i][j] > target:
                j -= 1
            elif matrix[i][j] < target:
                i += 1
            else:
                return True
        
        return False
```
##### 287. 寻找重复数

```python

class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        #抽屉原理
		#二分查找改进
        left = 0
        right = len(nums) - 1 
        while left < right:
            mid = left + (right - left)//2

            cnt = 0
            for num in nums:
                if num <= mid:
                    cnt += 1
            
            if cnt > mid:
                right = mid
            else:
                left = mid + 1

        return left
```

### 9 滑动窗口


##### 3. 无重复字符的最长子串

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if s == "":return 0
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

```

#### 单调队列 + 滑动窗口

##### 239. 滑动窗口最大值

单调队列 + 滑动窗口

deque

性能对比：

deque: 280 ms	26.9 MB	

list：8164 ms	27.5 MB

```python

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
	#性能比list更好,存索引
        deque = collections.deque()
        res = []
        for i in range(k):
            #单调降序队列
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop()
            deque.append(i)
        
        res.append(nums[deque[0]])
        for i in range(k,len(nums)):
            if deque[0] == i - k:
                deque.popleft()
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop()
                
            deque.append(i)
            res.append(nums[deque[0]])

        return res

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        #存索引
        deque = []
        res = []
        for i in range(0,k):
            #单调降序队列
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop(-1)
            deque.append(i)
        res.append(nums[deque[0]])

        for i in range(k,len(nums)):
            if deque and deque[0] == i - k:
                deque.pop(0)
            while deque and nums[deque[-1]] < nums[i]:
                deque.pop(-1)

            deque.append(i)
            res.append(nums[deque[0]])
        
        return res
```


##### 438. 找到字符串中所有字母异位词

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        need = dict()
        window = dict()
        left = 0 
        right = 0 
        valid = 0 
        start = 0 
        taget_len = len(p)
        res = []
        for char in p:
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
                if right - left == taget_len:
                    res.append(left)
                    
                char = s[left]
                left += 1
                if char in window:
                    if window[char] == need[char]:
                        valid -= 1
                    window[char] -= 1

        return res
```


### 10 图算法及高频


#### 图算法--拓扑排序 

BFS:Indegree入度表、edges邻接表、BFS(queue)遍历

##### 207 课程表

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

##### 621  任务调度器

```python

class Solution:
    def leastInterval(self,tasks,n):
        map = dict()
        maxExec = 0 
        for task in tasks:
            map[task] = map.get(task,0) + 1
            if map[task] > maxExec:
                maxExec = map[task]
        
        maxCount = 0
        for value in map.values():
            if value == maxExec:
                maxCount += 1
        
        minTime = (maxExec - 1) * (n + 1) + maxCount

        return max(minTime,len(tasks))
```

#### 图算法--并查集(Union-Find)


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
###### 399 除法求值

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


#### 哈希表

##### 1. 两数之和

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:

        map = dict()
        for i in range(len(nums)):
            sub = target - nums[i]
            if sub in map:
                return [map[sub],i]
            map[nums[i]] = i
        return []

```
##### 49. 字母异位词分组

collections.defaultdict 使用

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        #map --> value(list)
        map = collections.defaultdict(list)
        for i in range(len(strs)):
            char_str = ''.join(sorted(strs[i]))
            map[char_str].append(strs[i])
        
        return list(map.values())
```



#### 位运算

##### 136. 只出现一次的数字
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        oneTimeNumber = 0
        for i in range(len(nums)):
            oneTimeNumber ^= nums[i]
        
        return oneTimeNumber
```

##### 338. 比特位计数

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        res = []
        for i in range(n + 1):
            count = 0
            tmp = i
            while tmp:
                tmp = tmp & (tmp - 1)
                count += 1
            res.append(count)
        
        return res 
```
##### 461. 汉明距离

```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        s = x ^ y
        count = 0
        while s:
            s  = s &(s - 1)
            count += 1

        return count
```

#### 数据结构设计 LRU/LFU

##### 146. LRU 缓存机制

```python

from typing import List
class DLinkedNode:
    def __init__(self,key = 0,value = 0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        #Dlist相关，伪头伪尾
        self.head = DLinkedNode(0)
        self.tail = DLinkedNode(0)
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0 
        #cachemap相关
        self.cache = dict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.removeNode(node)
        self.addToHead(node)
        #self.cache[key] = node
        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            self.removeNode(node)
            #相当于引用
            node.value = value
            self.addToHead(node)
            #self.cache[key] = node
        else:
            node = DLinkedNode(key,value)
            self.cache[key] = node
            self.addToHead(node)
            self.size += 1
            if self.size > self.capacity:
                key = self.deleteTail()
                self.cache.pop(key)
                self.size -= 1
            
    #DList API
    def removeNode(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def addToHead(self, node):
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def deleteTail(self) ->int:
        node = self.tail.prev
        self.removeNode(node)
        return node.key

```

