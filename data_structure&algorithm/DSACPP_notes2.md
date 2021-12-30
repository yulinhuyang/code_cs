
[邓俊辉数据结构与算法学习笔记](https://blog.csdn.net/xiaodidadada/category_10348319.html)

[算法可视化](http://algorithm-visualizer.org/)

## 第7章 搜索树
 

## 第8章 高级搜索树 高级搜索树
 
## 第9章 词典
 

## 第10章 优先级队列

PQ定义:
```cpp
template <typename T> struct PQ { //优先级队列PQ模板类
   virtual void insert ( T ) = 0; //按照比较器确定的优先级次序插入词条
   virtual T getMax() = 0; //取出优先级最高的词条
   virtual T delMax() = 0; //删除优先级最高的词条
};
```

PQ_Complheap
```cpp
//PQ_ComplHeap.h

#include "Vector/Vector.h" //借助多重继承机制，基于向量
#include "PQ/PQ.h" //按照优先级队列ADT实现的
template <typename T> struct PQ_ComplHeap : public PQ<T>, public Vector<T> { //完全二叉堆
   /*DSA*/friend class UniPrint; //演示输出使用，否则不必设置友类
   PQ_ComplHeap() { } //默认构造
   PQ_ComplHeap ( T* A, Rank n ) { copyFrom ( A, 0, n ); heapify ( _elem, n ); } //批量构造
   void insert ( T ); //按照比较器确定的优先级次序，插入词条
   T getMax(); //读取优先级最高的词条
   T delMax(); //删除优先级最高的词条
}; //PQ_ComplHeap
template <typename T> void heapify ( T* A, Rank n ); //Floyd建堆算法
template <typename T> Rank percolateDown ( T* A, Rank n, Rank i ); //下滤
template <typename T> Rank percolateUp ( T* A, Rank i ); //上滤


//PQ_ComplHeap_Heapify.h

template <typename T> void heapify ( T* A, const Rank n ) { //Floyd建堆算法，O(n)时间
   for ( int i = n/2 - 1; 0 <= i; i-- ) //自底而上，依次
/*DSA*///{
      percolateDown ( A, n, i ); //下滤各内部节点
/*DSA*///for ( int k = 0; k < n; k++ ) {
/*DSA*///  int kk = k; while ( i < kk ) kk = (kk - 1) / 2;
/*DSA*///  i == kk ? print(A[k]) : print("    " );
/*DSA*///}; printf("\n");
/*DSA*///}
}

//PQ_ComplHeap_insert.h
template <typename T> void PQ_ComplHeap<T>::insert ( T e ) { //将词条插入完全二叉堆中
   Vector<T>::insert ( e ); //首先将新词条接至向量末尾
   percolateUp ( _elem, _size - 1 ); //再对该词条实施上滤调整
}

//PQ_ComplHeap_percolateDown.h
//对向量前n个词条中的第i个实施下滤，i < n
template <typename T> Rank percolateDown ( T* A, Rank n, Rank i ) {
   Rank j; //i及其（至多两个）孩子中，堪为父者
   while ( i != ( j = ProperParent ( A, n, i ) ) ) //只要i非j，则
      { swap ( A[i], A[j] ); i = j; } //二者换位，并继续考查下降后的i
   return i; //返回下滤抵达的位置（亦i亦j）
}

//PQ_ComplHeap_percolateUp.h
//对向量中的第i个词条实施上滤操作，i < _size
template <typename T> Rank percolateUp ( T* A, Rank i ) {
   while ( 0 < i ) { //在抵达堆顶之前，反复地
      Rank j = Parent ( i ); //考查[i]之父亲[j]
      if ( lt ( A[i], A[j] ) ) break; //一旦父子顺序，上滤旋即完成；否则
      swap ( A[i], A[j] ); i = j; //父子换位，并继续考查上一层
   } //while
   return i; //返回上滤最终抵达的位置
}

//PQ_ComplHeap_getMax.h
template <typename T> T PQ_ComplHeap<T>::getMax() {  return _elem[0];  }

//PQ_ComplHeap_delMax.h
template <typename T> T PQ_ComplHeap<T>::delMax() { //删除非空完全二叉堆中优先级最高的词条
   T maxElem = _elem[0]; _elem[0] = _elem[ --_size ]; //摘除堆顶（首词条），代之以末词条
   percolateDown ( _elem, _size, 0 ); //对新堆顶实施下滤
   return maxElem; //返回此前备份的最大词条
}
```
 
## 第11章 串

KMP排序

next[0] = -1, next构建过程就是P匹配自己的过程

```cpp
int match ( char* P, char* T ) {  //KMP算法
   int* next = buildNext ( P ); //构造next表
   int n = ( int ) strlen ( T ), i = 0; //文本串指针
   int m = ( int ) strlen ( P ), j = 0; //模式串指针
   while ( j < m  && i < n ) //自左向右逐个比对字符
      /*DSA*/{
      /*DSA*/showProgress ( T, P, i - j, j );
      /*DSA*/printNext ( next, i - j, strlen ( P ) );
      /*DSA*/getchar(); printf ( "\n" );
      if ( 0 > j || T[i] == P[j] ) //若匹配，或P已移出最左侧（两个判断的次序不可交换）
         { i ++;  j ++; } //则转到下一字符
      else //否则
         j = next[j]; //模式串右移（注意：文本串不用回退）
      /*DSA*/}
   delete [] next; //释放next表
   return i - j;
}

int* buildNext ( char* P ) { //构造模式串P的next表
   size_t m = strlen ( P ), j = 0; //“主”串指针
   int* N = new int[m]; //next表
   int t = N[0] = -1; //模式串指针
   while ( j < m - 1 )
      if ( 0 > t || P[j] == P[t] ) { //匹配
         j ++; t ++;
         N[j] = t; //此句可改进...
      } else //失配
         t = N[t];
   /*DSA*/printString ( P ); printf ( "\n" );
   /*DSA*/printNext ( N, 0, m );
   return N;
}

int* buildNext ( char* P ) { //构造模式串P的next表（改进版本）
   size_t m = strlen ( P ), j = 0; //“主”串指针
   int* N = new int[m]; //next表
   int t = N[0] = -1; //模式串指针
   while ( j < m - 1 )
      if ( 0 > t || P[j] == P[t] ) { //匹配
         N[j] = ( P[++j] != P[++t] ? t : N[t] ); //注意此句与未改进之前的区别
      } else //失配
         t = N[t];
   /*DSA*/printString ( P ); printf ( "\n" );
   /*DSA*/printNext ( N, 0, strlen ( P ) );
   return N;
}

```
 
## 第12章 排序
