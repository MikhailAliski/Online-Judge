# E5-EX 图中最大的集合
## 题目描述
在一张有向图$G$中，你需要找出节点数最多的一个节点集合$S$，使得$S$中的任意两个节点$A\ B$至少满足“$A$可达$B$”或者“$B$可达$A$”中的一个。如果$A$到$B$有连边，$B$到$C$有连边，那么我们认为$C$对$A$是可达的，即$A$可达$C$。

## 输入格式
第一行为2个整数，$N\ M$，分别代表节点个数、边的个数

接下来$M$行，每行表述一条边的信息，表述为$U_i\ V_i$的形式，表示一条从节点$U_i$到节点$V_i$的边

## 输出格式
输出一个数字，表示满足上述条件的最大的集合包含的节点的个数

## 数据规模：
对于所有数据，满足$1 \leq U_i \leq N$，$1 \leq V_i \leq N$

$40\%$的数据，$2 \leq N \leq 50$，$0 \leq M \leq 500$
$100\%$的数据，$2 \leq N \leq 5000$，$0 \leq M \leq 100000$

# 解题报告
## 初始思路
本题题目描述令人十分困惑，经过对于样例的分析，本题所求的是**有向图的最大单向连通子图**。开始认为这等价于求最长有向轨，给出图中最长可拓扑排序的顶集。

但题目条件并没有限制数据描述的图中**无环**，对于一个非DAG的图，很可能无法给出拓扑排序。进一步分析，每一个**强连通子图**一定包含于所求的的**最大单向连通子图**，而这样的一个强连通子图可以通过*Tarjan*算法进行缩点，缩点之后得到的图即DAG，就可以进行拓扑排序了。

算法有两个主要函数分别是`void Tarjan(int u)`从结点u向下DFS，确定以u为代表的强连通分量，以及拓扑排序`void TopoSort()`确定以各结点起始的最长距离。除此之外，需要额外存储缩点之后的图。

## 提交与修改过程
首次提交即[Accepted](https://202.38.86.171/status/6ce5cddfd1782744f67335009ccfaeb6)

## 算法分析
### 时间复杂度
*Tarjan*算法部分，每个顶点都被访问了一次，进栈出栈各一次，每条边也只被访问了一次，所以消耗$O(N + M)$的时间；基于缩点结果重新建图的过程是对边的遍历，消耗$O(M)$的时间；对于缩点后的拓扑排序，每个顶点（或强连通分量）进栈出栈各一次，更新最长轨和入度的操作在循环语句中总共执行`M'`次，所以消耗$O(N+M)$的时间，因此总的时间复杂度是$O(N + M)$的。

### 空间复杂度
额外存储缩点之后的图，算法消耗的空间是$O(N + M)$的。

## 总结
本题虽然描述较为简单和抽象，但是直接对应到图算法还是有一定困难。结合此前图论课程的知识，在本题的解题过程中，加深了对于强连通分量概念和*Tarjan*算法实现的理解；从题目描述到最长有向轨的转换，强化了对于问题中图理论背景的抽象能力。