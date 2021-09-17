
python实现 链表、快排和二分查找


### 1  链表 ，第0个节点
 
sentinel 0 | index 0 | index 1 | index 2 ...-> None

```python 

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
```

 
1 有sentinel==0的哨兵节点,然后index 从0 开始

2 所有的插入和删除都是针对index=0 开始的，哨兵节点不动

3 get的时候，需要range index +1 ,add的时候，range index

4 主要边界条件，delete < size。
	
5  头插，尾插，初始化，增加删除查询index

### 2 排序

快速排序 随机法：

```python

    def random_partition_right_pivot(self,nums,l,r)-> None:

        pivot_index = random.randint(l,r)
        nums[r],nums[pivot_index] = nums[pivot_index],nums[r]
        i = l -1 
        for j in range(l,r):
            if nums[j] < nums[r]:
            i = i + 1 
            nums[i],nums[j] = nums[j],nums[i]
        i = i + 1 
        nums[i],nums[r] = nums[r],nums[i]
        return i

    def random_partition_left_pivot(self,nums,l,r)-> None:

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
        
        return i

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

random partition: 随机选，换右边，左循环，i,j挪左边

交换两个数： a,b = b,a


pivot选择l的时候，快排为什么j先走： 

    https://blog.csdn.net/lkp1603645756/article/details/85008715
	
    i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的

###  3 二分查找

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
			



