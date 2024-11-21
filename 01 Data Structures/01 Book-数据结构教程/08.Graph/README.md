## 一、图的基本概念


## 二、图的存储结构和基本运算算法

### 2.1 邻接矩阵存储方法
- **邻接矩阵**(adjacency matrix)：是一种采用邻接矩阵数组表示*顶点之间相邻关系*的存储结构。设 G=(V, E) 是含有 n (n > 0) 个顶点的图。各顶点的编号为 0 ~ (n-1)，则G的邻接矩阵数组A是n阶方阵，其定义如下：（见课本）

### 2.2 邻接表存储方法
- **邻接表**（adjacency list）：是一种顺序与链式存储结构相结合的存储方法。头结点：|data|first|；边结点：|adjvex|nextarc|weight|（具体见课本）

## 三、图的遍历
```C++
//图的邻接矩阵（adjacency matrix）是一种采用邻接矩阵数组表示顶点之间相邻关系的存储结构。
#define MAXV<最大顶点个数>
#define INF 32767   // 定义∞
typedef struct
{
	int no;    // 顶点的编号
	InfoType info  // 顶点的其他信息
}VertexType;  // 顶点的类型
typedef struct
{
	int edges[MAXV][MAXV];
	int n, e;	// 顶点数，边数
	VertexType vexs[MAXV];	// 存放顶点信息
}MatGraph;	// 完整的图邻接矩阵类型
// 邻接表的存储方法
```

## 四、生成树和最小生成树


## 五、最短路径


## 六、拓扑排序


## 七、AOE网与关键路径
