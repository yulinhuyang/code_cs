
牛蛙点点提高课题解：https://www.acwing.com/user/myspace/activity/52559/

wzc1995: https://www.acwing.com/user/myspace/index/21/

仅存老实人题解：https://www.acwing.com/user/myspace/solution/index/39715/



### DP模板

提高课题单 + 题解(71/220)：https://www.acwing.com/blog/content/7459/

### 单调队列优化的DP问题

- AcWing 154. 滑动窗口: 模板题，https://www.acwing.com/solution/content/104115/
- AcWing 1088 旅行问题：模板题，顺时针(pi-di前缀和)-> 单调队列(min上升队列) ->逆时针(pi-d(i-1)的后缀和)->单调队列
https://www.acwing.com/solution/content/104816/

- AcWing 135 最大子序和：     
- AcWing 1087 修剪草坪：       
- AcWing 1089 烽火传递：单调队列优化DP + 目标状态小优化，https://www.acwing.com/solution/content/67778/     
- AcWing 1090 绿色通道：   
- AcWing 1091 理想的正方形：二维滑动窗口模型，https://www.acwing.com/solution/content/68010/  

### 斜率优化DP 
 
- AcWing 300 任务安排1：线性DP + 费用提前计算思想 https://www.acwing.com/file_system/file/content/whole/index/content/2972413/
```C++
S * (sc[n] - sc[j]) ：费用提前计算
f[i] = min(f[i], f[j] + st[i] * (sc[i] - sc[j]) + S * (sc[n] - sc[j]));
```
- AcWing 301 任务安排2：https://www.acwing.com/solution/content/35208/
```C++
fi = sti × sci + S × scn + min(fj − S × scj − sti × scj)
这里 fj − scj × (S + sti) = 变量1 − 变量2 × (常量S+常量i)，有i x j的项

Andrew 算法求凸包:https://oi-wiki.org/geometry/convex-hull/     
单调栈来维护上下凸壳。     
因为从左向右看，上下凸壳所旋转的方向不同，为了让单调栈起作用，我们首先升序枚举求出下凸壳，然后降序求出上凸壳。
求凸壳时，一旦发现即将进栈的点（P）和栈顶的两个点（S1,S2，其中S1为栈顶）行进的方向向右旋转，即叉积小于S2S1 X S1P < 0，则弹出栈顶，回到上一步，继续检测，直到S2S1 X S1P >= 0或者栈内仅剩一个元素为止。    

去寻找下凸壳上的点构成直线的最小截距即可。单调队列中相邻两点之间构成的直线斜率单增，也就是有效下凸壳点集。    

斜率优化DP模板：  
1 将初始状态入队。   
2 每次使用一条和i相关的直线fi去切维护的凸包，找到最优决策，更新dpi。  
3 加入状态dpi，如果一个状态（即凸包上的一个点）在dpi加入后不再是凸包上的点，需要在dpi加入之前剔除。   
把点插入队列前，先要队列中至少有两个点，新加入的点，必须和原点集构成下凸壳，无效点要先删去。   
```   
- AcWing 302 任务安排3：二分优化斜率优化DP，https://www.acwing.com/solution/content/68118/
任务的执行时间 tt 可能是负数，那么斜率不具有单调性，应该维护整个凸壳。
利用上单调性，用队列维护一个下凸壳的点集。   
则对于ki，找到大于他的最小值就可以二分。   
- AcWing 303 运输小猫：斜率优化DP, https://www.acwing.com/solution/content/68473/    
预处理前缀和：min{fi−1,k + aj ×(j−k)−(sj−sk)}            
f[i][j]提出常量：f[i][j]= j×a[j] − s[j]+min{f[i-1][k]+s[k]−a[j]×k}        
维护下凸壳的点集，求第一个出现在直线上的点     

### 数学知识

#### 筛质数：

AcWing 1292 哥德巴赫猜想     
AcWing 1293 夏洛克和他的女朋友     
AcWing 196 质数距离     

#### 分解质因数：

AcWing 197 阶乘分解     

#### 快速幂：

AcWing 1289 序列的第k个数     
AcWing 1290 越狱     

#### 约数个数：

AcWing 1291 轻拍牛头     
AcWing 1294 樱花     
AcWing 198 反素数     
AcWing 200 Hankson的趣味题     

#### 欧拉函数：

AcWing 201 可见的点     
AcWing 220 最大公约数     

#### 同余：

AcWing 203 同余方程     
AcWing 222 青蛙的约会     
AcWing 202 最幸运的数字     
AcWing 1298 曹冲养猪     

#### 矩阵乘法：
     
AcWing 1303 斐波那契前n项和     
AcWing 1304 佳佳的斐波那契     
AcWing 1305 GT考试     

#### 组合计数：

AcWing 1307 牡牛和牝牛     
AcWing 1308 方程的解     
AcWing 1309 车的放置     
AcWing 1310 数三角形     
AcWing 1312 序列统计     
AcWing 1315 网格     
AcWing 1316 有趣的数列   

#### 高斯消元：

AcWing 207 球形空间产生器          
AcWing 208 开关问题     

#### 容斥原理：

AcWing 214 Devu和鲜花     
AcWing 215 破译密码     

#### 概率与数学期望：

AcWing 217 绿豆蛙的归宿     
AcWing 218 扑克牌     

#### 博弈论：

AcWing 1319 移棋子游戏     
AcWing 1321 取石子     
AcWing 1322 取石子游戏     
