数据结构精选

### 1 排序算法

内部排序模板大汇总： https://www.acwing.com/blog/content/9013/

#### 1. 直接插入排序

```cpp
//直接插入排序（无哨兵） 
void InsertSort(int a[], int n){
    int i, j , temp;
    for(i = 1; i < n; i ++){
        if(a[i] < a[i - 1]){
            temp = a[i];
            for(j = i - 1; j >= 0 && temp < a[j]; j --){
                a[j + 1] = a[j];
            }
            a[j + 1] = temp;
        }
    }
}

//直接插入排序（有哨兵） 
void InsertSort2(int a[], int n){
    int i , j;
    for(i = 2; i <= n; i ++){
        if(a[i] < a[i - 1]){
            a[0] = a[i];
            for(j = i - 1; a[0] < a[j]; j --){
                a[j + 1] = a[j];
            }
            a[j + 1] = a[0];
        }
    }
} 
```

**插入排序**

```C++
void insertion_sort(int a[], const int start, const int end) {
    int tmp;
    int i, j;
    for (i = start + 1; i < end; i++) {
        tmp = a[i];
        for (j = i - 1; tmp < a[j] && j >= start; j--) {
            a[j + 1] = a[j];
        }
        a[j + 1] = tmp;
    }
}
```

#### 2 折半插入排序 

```cpp
//折半插入排序 (带哨兵) 
void BinaryInsertSort(int a[], int n){
    int i, j, low, high, mid;
    for(i = 2; i <= n; i ++){
        a[0] = a[i];//将带插入元素赋给哨兵
        low = 1, high = i - 1;//在1~i-1之间进行二分查找 
        while(low <= high){//high > low循环结束
         mid = low + high >> 1;
         if(a[0] < a[mid]) high = mid - 1;
         else low = mid + 1;//即便元素相等也插入其后，保持排序稳定性
        }
        for(j = i - 1; j >= high + 1; j --){// 待插元素应位于high+1处
         a[j + 1] = a[j];//将有序元素后移 
        }
        a[high + 1] = a[0];//将待插元素插入到正确位置
    }
}
```

#### 3 希尔排序

```cpp
//希尔排序 
void ShellSort(int a[], int n){
    int d, i, j;
    for(d = n / 2; d >= 1; d /= 2){//d为增量,增量序列依次折半(算法原创作者本人推荐) 
        for(i = d + 1; i <= n; i ++){//处理局部序列 
            if(a[i] < a[i - d]){
                a[0] = a[i];//暂存当前待处理元素 
                for(j = i - d; j > 0 && a[0] < a[j]; j -= d){
                    a[j + d] = a[j];//局部元素后移 
                }
                a[j + d] = a[0];//在合适位置处插入当前元素 
            }
        }
    }
}
```

#### 4. 冒泡排序

```cpp
//冒泡排序 
void BubbleSort(int a[], int n){
    for(int i = 0; i < n - 1; i ++){// 控制趟数, n-1趟 
        bool flag = false;//标记每一趟是否有元素发生交换 
        for(int j = n - 1; j > i; j --){//从后往前依次枚举 
            if(a[j] < a[j - 1]){
            swap(a[j], a[j - 1]);
            flag = true;//发生交换 
            } 
        }
        if(flag == false) return;//本趟未发生交换,即所有元素都已经有序 
    }
}
```

```cpp
1.朴素写法

//升序排序
void bubble_sort(int a[], int len){//枚举趟, len为数组长度
    for(int i = 0; i < len; i ++){//枚举比较元素
        for(int j = 0; j < len - i - 1; j ++){
            if(a[j] > a[j + 1]){//逆序,交换
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
            }
        }
    }
}

2.一次优化

设置一个标志位, 用来表示当前第 i 趟是否有交换, 如果有则要进行 i+1 趟, 如果没有, 则说明当前数组
已经完成排序, 一旦发现已经排好序, 立即跳出循环, 减少无谓的比较次数.
//升序排序
void bubble_sort(int a[], int len){
    for(int i = 0; i < len; i ++){
        bool flag = true;//记录是否发生交换
        for(int j = 0; j < len - i - 1; j ++){
            if(a[j] > a[j + 1]){
                cnt ++;
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
                flag = false;//发生交换
            }
        }
        if(flag) break;//某一趟已经完全排好序,直接退出.
    }
}


3.二次优化

利用一个标志位, 记录一下当前第 i 趟所交换的最后一个位置的下标，在进行第 i+1趟的时候, 只需要内循
环到标记位置就可以了, 因为后面位置上的元素在上一趟中没有换位, 这一次也不可能会换位置了.
//升序排序
void bubble_sort(int a[], int len){
    int pos;
    for(int i = 0; i < len; i ++){
        bool flag = true;
        for(int j = 0; j < len - i - 1; j ++){
            if(a[j] > a[j + 1]){
                cnt ++;
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
                pos = j;//记录交换的位置
                flag = false;//发生交换
            }
        }
        len = pos;//记录上一次已比较好的位置,用来更新 len
        if(flag) break;
    }
}
```

**冒泡排序优化版**

```C++
void bubbleSort(vector<int> &nums) {

	int sortBorder = nums.size() - 1;
	int lastExchangeIndex = 0;
	for (int i = 0; i < nums.size(); i++) {
		bool isSorted = true;
		for (int j = 0; j < sortBorder; j++) {
			//j的相邻原始比较向上冒泡
			if (nums[j] > nums[j + 1]) {
				swap(nums[j], nums[j + 1]);
				isSorted = false;
				//设置有序边界,扩大有序区长度
				lastExchangeIndex = j;
			}
		}
		if (isSorted) {
			break;
		}
		sortBorder = lastExchangeIndex;
	}
}
```

#### 5. 快速排序

**快速排序随机优化版**

```C++
class Solution {
public:
    int partition(vector<int> &nums, int low, int high) {
        //gen random index
        int index = (rand()%(high - low + 1))+1;
        swap(nums[low],nums[index]);

        int pivot = nums[low];
        int i = low;
        int j = high;
        while (i < j) {
            //i在大于基准数的地方停下，j在小于基准数的地方停下，如果i先走，最后停下跟基准数交换时，总是大于基准数的
            //j先走
            while (i < j && nums[j] >= pivot) {
                j--;
            }
            while (i < j && nums[i] <= pivot) {
                i++;
            }
            swap(nums[i], nums[j]);
        }
        swap(nums[low], nums[i]);
        return i;
    }

    void quickSort(vector<int> &nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int index = partition(nums, start, end);
        quickSort(nums, start, index - 1);
        quickSort(nums, index + 1, end);
    }
}
```

快排模板

```cpp
void quick_sort(int q[], int l, int r)
{
    if(l >= r)   return;
    int x = q[l + r >> 1], i = l - 1, j = r + 1;
    while(i < j)
    {
        do i++; while(q[i] < x);
        do j--; while(q[j] > x);
        if(i < j)   swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

```

#### 6. 选择排序

```cpp
//交换两个元素 
void swap(int &a, int &b){
    int temp = a;
    a = b;
    b = temp;
}
//选择排序 
void SelectSort(int a[], int n){
    for(int i = 0; i < n; i ++){
        int min = i;//min记录最下元素的下标 
        for(int j = i; j < n; j ++){
            if(a[j] < a[min]) min = j;
        }
        if(min != i) swap(a[i], a[min]);//交换两个值 
    }
}
```

#### 7. 归并排序

```cpp
//归并排序
void merge_sort(int a[], int l, int r){
    if(l >= r) return;
    int mid = l + r >> 1;
    merge_sort(a, l, mid), merge_sort(a, mid + 1, r);//递归划分

    int k = 0, i = l, j = mid + 1;
    while(i <= mid && j <= r){
        if(a[i] <= a[j]) q[k ++] = a[i ++];
        else q[k ++] = a[j ++];
    }
    while(i <= mid) q[k ++] = a[i ++];
    while(j <= r) q[k ++] = a[j ++];

    for(int i = l, j = 0; i <= r; i ++, j ++) a[i] = q[j];//合并
}
```
#### 8. 堆排序

```cpp
//将以k为根结点的树调整为大根堆 
void HeadAdjust(int a[], int k, int len){//注：除k结点外其他已经有序 
    a[0] = a[k];//a[0]暂存子树根节点 
    for(int i = 2 * k; i <= len; i *= 2){//沿较大子结点筛选 
        if(i < len && a[i] < a[i + 1]) i ++;//i为较大子结点下标 （i<len表示k有右孩子） 
        if(a[0] >= a[i]) break;//筛选结束 
        else{
            a[k] = a[i];//递归处理子结点 
            k = i;
        }
    }
    a[k] = a[0];
}

//建大根堆 时间复杂度---O(n)  
void BuildHeap(int a[], int len){//从下往上建堆,从最后一个叶子结点的根节点开始 
    for(int i = len / 2; i > 0; i --){//处理所有非终端结点 
        HeadAdjust(a, i , len);
    }
}//建堆过程关键字的比较次数不超过4n(定理) 
```

#### 9. 计数排序

```cpp
//朴素版 
void CountSort(int a[], int n){
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){//遍历数组求得最大值和最小值 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int cnt[maxval + 1] = {0};//根据最大值开辟新数组空间 

    for(int i = 0; i < n; i ++) cnt[a[i]] ++;//统计原数组中元素出现的次数 

    for(int i = minval, k = 0; i <= maxval; i ++){
        while(cnt[i] != 0){
            data[k ++] = i;//将排序后的序列赋给原数组 
            cnt[i] --;//i出现的次数减1 
        }
    }
}

//一次优化 
void CountSort(int a[], int n){
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){//遍历数组求得最大值和最小值 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int d = maxval - minval + 1;// 计数数组的实际长度 
    int cnt[d] = {0};//根据最大值开辟新数组空间 

    //统计原数组中元素出现的次数
    for(int i = 0; i < n; i ++) cnt[a[i] - minval] ++;//将元素映射到a[0...d-1] 

    for(int i = 0, k = 0; i <= maxval - minval; i ++){
        while(cnt[i] != 0){
            data[k ++] = i + minval;//将排序后的序列赋给原数组 
            cnt[i] --;//i出现的次数减1 
        }
    }
}

//二次优化 
void CountSort(int a[], int n){
    //遍历数组求得最大值和最小值
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){ 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int d = maxval - minval + 1;// 计数数组的实际长度 
    int cnt[d] = {0};//根据最大值开辟新数组空间 

    //统计原数组中元素出现的次数
    for(int i = 0; i < n; i ++) cnt[a[i] - minval] ++;//将元素映射到a[0...d-1] 

    int sum = 0;
    for(int i = 0; i < d; i ++){//本质为前缀和数组,用于求位次 
        sum += cnt[i];//此处的cnt既为元素又为之前的元素和,即cnt[i] = cnt[i] + cnt[i - 1] 
        cnt[i] = sum;// 比如, cnt[5] = 3,表示分数95, 排名第 3 
    } 

    int sortArray[d];//sortArray[]存元素真实序列 
    for(int i = n - 1; i >= 0; i --){//将原数组元素从后往前遍历 
        sortArray[cnt[a[i] - minval] - 1] = a[i];
        cnt[a[i] - minval] --;
    }

    //将排序后的序列赋给原数组
    for(int i = 0, k = 0; i < d; i ++){
        data[k ++] = sortArray[i];
    }
}

```

#### 10. 桶排序

```cpp
//桶排序 
void BucketSort(int a[], int n){
    int minval = a[0], maxval = a[0];
    for(int i = 0; i < n; i ++){//寻找原序列数组元素的最大值和最小值 
        minval = min(minval, a[i]);
        maxval = max(maxval, a[i]);
    }

    int bnum = 10;//桶中元素个数 
    int m = (maxval - minval) / bnum + 1;//桶的个数 
    vector< vector<int> > bucket(m);

    //收集,将元素入相应的桶中. 减偏移量是为了将元素映射到更小的区间内,省内存 
    for(int i = 0; i < n; i ++) bucket[(a[i] - minval) / bnum].push_back(a[i]);

    //将桶内元素排序 
    for(int i = 0; i < m; i ++) sort(bucket.begin(), bucket.end());

    //收集, 将各个桶中的元素收集到一起 
    for(int i = 0, k = 0; i < m; i ++){
        for(int j = 0; j < bucket[i].size(); j ++){
            data[k ++] = bucket[i][j];
        }
    }
}
```
#### 11. 基数排序

```cpp
//基数排序 
void RadixSort(int a[], int n){
    const int RADIX = 10;//DADIX表示为十进制数 
    int maxval = a[0], minval = a[0], base[10];//base数组存的是十进制数的位权 
    vector< vector<int> > bucket(RADIX);//相当于定义10个桶 

    base[0] = 1;//个位的权值为 1 
    for(int i = 1; i < 10; i ++) base[i] = base[i - 1] * 10;//初始化权值 
    for(int i = 0; i < n; i ++){
        maxval = max(maxval, a[i]);//求出所有元素的最大值 
    }

    int dnum = ceil(log10(maxval + 1));//求出最大的位数,比如38为2位 

    for(int d = 0; d < dnum;  d ++){//枚举每一位,共dnum趟 
        for(int i = 0; i < n; i ++){//枚举每一个数 
            int t = (a[i] / base[d]) % RADIX;//取出该位数值,即对应的桶号 
            bucket[t].push_back(a[i]);//将当前数值放入桶中(对应位) 
        }

        for(int i = 0, j = 0, k = 0; i < n; ){//i为当前元素下标,j为桶的编号
            if(k < bucket[j].size()){//k为桶中的第k号元素 
                a[i ++] = bucket[j][k ++];//依次将桶中元素存入数组(已按当前位有序) 
            }
            else j ++, k = 0;//枚举下一个桶,从该桶中第0号元素开始 
        }
        for(auto &v : bucket) v.clear();//将桶中元素清零, 开始下一趟排序 
    }
}

```

### 2 搜索算法

**DFS**

```cpp
//三种状态：unDiscovered、Discovered、visited
template <typename Tv, typename Te> //深度优先搜索DFS算法（全图）
void Graph<Tv, Te>::dfs ( int s ) { //assert: 0 <= s < n
   reset(); int clock = 0; int v = s; //初始化
   do //逐一检查所有顶点
      if ( UNDISCOVERED == status ( v ) ) //一旦遇到尚未发现的顶点
         DFS ( v, clock ); //即从该顶点出发启动一次DFS
   while ( s != ( v = ( ++v % n ) ) ); //按序号检查，故不漏不重
}

template <typename Tv, typename Te> //深度优先搜索DFS算法（单个连通域）
void Graph<Tv, Te>::DFS ( int v, int& clock ) { //assert: 0 <= v < n
   dTime ( v ) = ++clock; status ( v ) = DISCOVERED; //发现当前顶点v
   for ( int u = firstNbr ( v ); -1 < u; u = nextNbr ( v, u ) ) //枚举v的所有邻居u
      switch ( status ( u ) ) { //并视其状态分别处理
         case UNDISCOVERED: //u尚未发现，意味着支撑树可在此拓展
            type ( v, u ) = TREE; parent ( u ) = v; DFS ( u, clock ); break;
         case DISCOVERED: //u已被发现但尚未访问完毕，应属被后代指向的祖先
            type ( v, u ) = BACKWARD; break;
         default: //u已访问完毕（VISITED，有向图），则视承袭关系分为前向边或跨边
            type ( v, u ) = ( dTime ( v ) < dTime ( u ) ) ? FORWARD : CROSS; break;
      }
   status ( v ) = VISITED; fTime ( v ) = ++clock; //至此，当前顶点v方告访问完毕
}
```

**BFS**

```cpp
//三种状态：unDiscovered、Discovered、visited
template <typename Tv, typename Te> //深度优先搜索DFS算法（全图）
void Graph<Tv, Te>::dfs ( int s ) { //assert: 0 <= s < n
   reset(); int clock = 0; int v = s; //初始化
   do //逐一检查所有顶点
      if ( UNDISCOVERED == status ( v ) ) //一旦遇到尚未发现的顶点
         DFS ( v, clock ); //即从该顶点出发启动一次DFS
   while ( s != ( v = ( ++v % n ) ) ); //按序号检查，故不漏不重
}

template <typename Tv, typename Te> //深度优先搜索DFS算法（单个连通域）
void Graph<Tv, Te>::DFS ( int v, int& clock ) { //assert: 0 <= v < n
   dTime ( v ) = ++clock; status ( v ) = DISCOVERED; //发现当前顶点v
   for ( int u = firstNbr ( v ); -1 < u; u = nextNbr ( v, u ) ) //枚举v的所有邻居u
      switch ( status ( u ) ) { //并视其状态分别处理
         case UNDISCOVERED: //u尚未发现，意味着支撑树可在此拓展
            type ( v, u ) = TREE; parent ( u ) = v; DFS ( u, clock ); break;
         case DISCOVERED: //u已被发现但尚未访问完毕，应属被后代指向的祖先
            type ( v, u ) = BACKWARD; break;
         default: //u已访问完毕（VISITED，有向图），则视承袭关系分为前向边或跨边
            type ( v, u ) = ( dTime ( v ) < dTime ( u ) ) ? FORWARD : CROSS; break;
      }
   status ( v ) = VISITED; fTime ( v ) = ++clock; //至此，当前顶点v方告访问完毕
}
```

### 3 数据结构

**哈夫曼树构造**

哈夫曼树是带权路径长度最短的树，权值较大的结点离根较近

构造过程

(0) 补加一些额外的权值为0的叶子节点，使叶子节点的个数n满足（n-1)mod (k-1) = 0，n是节点数，k叉树。

(1) 建立一个小根堆，插入这n个叶子节点的权值；

(2) 从堆中取出最小的两个权权值是w1和w2,令ans += w1 + w2；

(3) 建立一个权值为w1+w2的树节点p，令p成为权值为w1和w2的树节点的父亲；

(4) 重复(2)、(3)步，直到森林中只剩一棵树为止，即堆的大小为1，该树即为所求得的哈夫曼树。变量ans就是wi * li的最小值。

任何一个整数模9同余与它的各数位上的数字之和





