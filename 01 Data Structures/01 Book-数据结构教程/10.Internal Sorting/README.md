- [ ] 归并排序
- [ ] 堆排序
- [ ] 基数排序
- [ ] [题目练习](https://www.nowcoder.com/test/question/done?tid=53222626&qid=98665)
- [ ] 默写算法 + 推到时空复杂度

# 第10章 内排序
## 10.1 排序的基本概念

### 1.什么是排序
- 所谓排序，是整理表中的记录，使之按关键字递增(或递减)有序排列

### 2.排序算法的稳定性
- 当待排序记录的关键字均不相同时，排序的结果是唯一的。
- stable：相同的关键字经过排序后，相对顺序保持不变。反之则为unstable

### 3.内排序和外排序
- 在排序过程中，若整个表都是放在内存中处理，排序时不涉及数据的内、外存交换，则称之为**内排序**;
- 反之，若排序过程中要进行数据的内、外存交换，则称之为**外排序**

### 4.基于比较的排序算法
基于比较的排序算法：
- 插入排序
  - 直接插入
  - 折半插入
  - 希尔排序
   
- 交换排序
  - 冒泡排序
  - 快速排序
  
- 选择排序
  - 简单选择排序
  - 堆排序
  
- 归并排序
  - 二路归并
  - 多路归并

不基于比较的排序算法：
- 基数排序

排序最好的性能也只能是：\(O(n \log n)\)（见课本），也就是说**不可能有更优的排序算法**。
- ![image.png](res/02.png)


### 5.内排序的数据组织
- 待排序的顺序表的数据元素类型声明如下：
```C
typedef int KeyType;  //关键字类型
typedef struct
{
    KeyType key;  //关键字（学生的学号）
	InfoType data;  //其他信息（学生的姓名、年龄、成绩等）
}RecType;  //记录类型
```

- 全局有序：该区域的元素的相对位置不在改变
- 正序和反序：定义略。有一些排序算法与初始序列的**正序或反序有关**，另一些排序算法
与初始序列的情况无关。


## 10.2 插入排序
- 插入排序：每次将一个待排序的元素按其关键字大小插入到前面已经排好序的子表中的适当位置，直到全部元素插入完成为止

- 有序区类型：
	- 局部有序区：其中元素在后面排序中会发生位置的改变。
	- 全局有序区：其中元素在后面排序中**不再**发生位置的改变。

- 插入排序都是局部有序区，而非全局有序区

### 1.直接插入排序
```C
// 直接插入排序(升序）- O(n^2)
void InsertSort(RecType R[], int n)
{   int i, j; RecType tmp;
    
    for(i = 1; i < n; i++)
    {   if(R[i].key < R[i-1].key)
        {   tmp = R[i];
            j = i - 1;

			do
            {   R[j+1] = R[j];
                j--;
            } while(j >=0 && R[j].key > tmp.key);
            R[j+1] = tmp;
        }
    }
}
```
- 平均时间复杂度：O(n^2)（比较次数+移动次数）
	- ![image.png](res/03.png)

### 2.折半插入排序
- **折半插入排序**(binary insert sort)：也称二分插入排序。由于有序区的元素是有序的，可以采用折半查找方法先在R[0..i-1]中找到插入位置，再通过移动元素进行插入。第 i 躺在R[low..high](初始时 low = 0, high = i - 1)中采用折半查找方法插入R[i]的位置为R[high+1], 再将R[high+1 .. i-1] 元素后移一个位置，并置R[high+1] = R[i]。high = i - 1 --> i = high + 1
```C

void BinInsertSort(RecType R[], int n)
{
    int i, j, low, high, mid;
    RecType tmp;
    for(i = 1; i < n; i++)
    {
        if(R[i].key < R[i-1].key)   // 反序时
        {
            tmp = R[i];
            low = 0; high = i - 1;
            while(low <= high)  // 在R[low ... high]中查找插入的位置
            {
                mid = (low + high)/2;
                if(tmp.key < R[mid].key)
                    high = mid - 1;
                else
                    low = mid + 1;
            }
            
            for(j = i - 1; j >= high+1; j--)  // high+1是插入位置
                R[j+1] = R[j];  // 为待插入元素腾出位置
            R[high+1] = tmp;  // 插入tmp
            
        }
    }
}
```
- Self：这个感觉和直接插入非常像。
- ![image.png](res/04.png)

### 3.希尔排序
- 基本思路：
    - ① d = n / 2
    - ② 将排序序列分成d个组，在各组内进行**直接插入排序**
    - ③ 递减 d = d / 2（向下取整），重复②，直到d = 1

```C
// 希尔排序算法 O(n^1.3)，不稳定
void ShellSort(RecType R[], int n) 
{
    int i, j, d;
    RecType tmp;
    d = n / 2;  // 增量置初值
    
    while(d > 0)  // 相比于直接插入，对d进行循环，每个记录都参加了排序
    {
        for(i = d; i < n; i++)
        {
            tmp = R[i];  // 对相隔d位置的元素组进行直接插入排序
            j = i - d;
            while(j >= 0 && tmp.key < R[j].key)
            {
                R[j+d] = R[j];
                j = j - d;
            }
            R[j+d] = tmp;
        }
        d = d / 2;
    }
} 
    
// 部分直接插入排序，为了与希尔排序比较
for( i = 1; i < n; i++)
{
    tmp = R[i];
    j = i - 1;
    while( j >= 0 && tmp.key < R[j].key)
    {
        R[j+1] = R[j];
        j = j - 1;
    }
    R[j+1] = tmp;
}

```
- 为什么希尔排序比直接插入排序好？
	- ![image.png](res/05.png)
	- ![image.png](res/06.png)

- 希尔排序算法是**不稳定**的
	- ![image.png](res/07.png)

`【例 10.2】略`  

- 插入排序产生的有序区都是局部有序区，**不是全局有序**区。

## 10.3 交换排序
- **交换排序**：基本思想是

### 1.冒泡排序
- **冒泡排序**(bubble sort)：通过无序区中相邻元素关键字间的比较和位置交换是关键字最小的元素如气泡一般逐渐往上“漂浮”直至“水面”

```C
// 冒泡排序算法
void BubbleSort(RecType R[], int n)
{   int i, j;
 
    for(i=0; i<n-1; i++)
        for(j=n-1; j>i; j++)
            if(R[j].key < R[j-1].key)
                swap(R[j], R[j-1]);
}
// 缺陷：排好序之后仍进行后几趟的比较
```
- 冒泡排序是**全局有序区**，排序好后不再改变。

```C
// 改进后的冒泡排序算法（通常说的冒泡排序就是“改进后的排序算法”）
void BubbleSort1(RecType R[], int n)
{   int i, j;
    bool exchange;
    
    for(i=0; i<n-1; i++)
    {   exchange = false;
        for(j=n-1; j>i; j++)
            if(R[j].key < R[j-1].key)
            {   swap(R[j], R[j-1]);
                exchange = true;
            }
        if(!exchange)
            return;
    }
}   
```

```C++
// 冒泡排序第三种写法
void BubbleSort2(RecType R[], int n)
{
	bool exchange = true;
	// 最后一个没有经过排序的元素的下标
	int indexOfLastUnsortedElement = n - 1;

	while (exchange)
	{
		exchange = false;
		int exchangeIndex = -1;

		for (int i = 0; i < indexOfLastUnsortedElement; i++)
		{
			if (R[i].key > R[i + 1].key)
			{
				Swap(R, i, i + 1);

				exchange = true;
				exchangeIndex = i;
			}
		}

		indexOfLastUnsortedElement = exchangeIndex;
	}
}
```
`【例 10.3】略`  

### 2.快速排序
- **快速排序**(quick sort)：是由冒泡排序改进而得，基本思想是在待排序的n个元素中任取一个元素（通常取第一个元素）作为基准，把该元素放入适当位置后，数据序列被此元素划分为两部分。所有关键字比该元素小的元素放置在前一部分，所有比它大的元素放置在后一部分，并把该元素排在这两部分的中间（称为该元素归位），这个过程称为一趟快速排序，即一趟划分。快速排序每趟仅由一个元素归位。
- 特点：第 i 趟，至少有i个元素归位

```C
int partition(RecType R[], int s, int t)
{   int i=s, j=t;    
    RecType tmp = R[i];
  
    while(i < j)
    {   while(j>i && R[j].key >= tmp.key)
            j--;
        
        R[i] = R[j];
        
        while(i<j && R[j] <= tmp.key)
            i++;
        
        R[j] = R[i];
    }
    R[i] = tmp;
    return i;
}

 // s是第一个元素，t是最后一个元素（以第1个元素为基准）
void QuickSort(RecType R[], int s, int t)  
{   int i;
    if(s < t)
    {   i = partition(R, s, t);
        QuickSort(R, s, i-1);
        QuickSort(R, i+1, t);
    }
}

// 快排可以以任意一个元素为基准（如，以中间元素为基准）
void QuickSort1(RecType R[], int s, int t)
{   int i, pivot;
    pivot = (s + t) / 2;
    if(s < t)             // 区间内至少存在两个元素
    {   
		if(pivot != s)  // 若基准不是区间内的第一个元素，将其与第一个元素交换
            swap(R[pivot], R[s]);

        i = partition(R, s, t);
        QuickSort(R, s, i-1);
        QuickSort(R, i+1; t);
    }
}
```

- 随机快速排序：通过随机枢轴，降低最坏情况出现的概率。通过将上述代码中的pivot随机一下即可

`【例 10.4】略`  

- 快速排序的效率：
	- 最好情况：时间复杂的O(nlogn)，空间复杂度的O(logn)，递归调用次数为logn。（恰好划分出两个长度相同的子序列。也就是随机分布的情况）
	- 最坏情况：时间复杂的O(n^2)，空间复杂度的O(n)，递归调用次数为n。（正序或反序的情况）

- 提示：题目没有要求一定有序
	- ![image.png](res/08.png)

## 10.4 选择排序
- **选择排序**(selection sort)：每一趟从待排序的元素中选出关键字最小（或最大）的元素，顺序放在排好序的子表最前（或最后），直到全部元素排序完毕。


### 1.简单选择排序
- **简单选择排序**(simple selection sort)

```C
// 简单选择排序- O(n^2)
void SelectSort(RecType R[], int n)
{   int i, j, k;
    for(i = 0; i < n -1 ; i++)
    {   
		k = i;

        for(j = i + 1; j < n; j++)
		{
			if(R[j].key < R[k].key)
			{
				k = j;
			}
		}
                   
        if(k != i)
		{
			swap(R[i], R[k]);
		}
    }
}
```

### 2.堆排序
- **堆排序**(heap sort)：是一种树形选择排序方法，它的特点是将R[1..n]完全看成是一棵完全二叉树的顺序存储结构。利用完全二叉树中双亲结点和孩子节点之间的位置关系在无序区中选择关键字最大（或最小）的元素。堆排序是由简单选择排序发展而来的，利用了连续多次查找最大记录的特性。

- 堆的特点：
    - **是一颗完全二叉树**
    - 所有父节点的值都要大于（或小于）子节点的值
    
- 大根堆（大顶堆）：所有节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列
- 小根堆（小顶堆）：所有节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列

- 如何判断大根堆？
    - 最后一个根节点的编号是 n/2 （向上取整）
    - 从后往前，依次对分支节点判断是否满足大根堆条件

- 筛选算法建堆（不是堆 -> 堆）：
    - 就是进行节点调整
    - 对一颗左/右子树均为堆的完全二叉树，“调整”根节点使整个二叉树也成为一个堆
    - 仅仅处理从根节点 -> 某个叶子结点路径上的结点
    - n个结点的完全二叉树高度为 log2^(n+1) （向上取整）
    - 所有筛选的时间复杂度为 O(log2^n)

```C
// 筛选算法时间复杂度： O(log2^n)
void sift(RecType R[], int low, int high)
{   int i=low, j = 2 * i;   // R[j] 是 R[i] 的左孩子
    RecType tmp = R[i];
    
    while(j <= high)
    {   
        if(j < high && R[j].key < R[j+1].key)   // 若右孩子较大，把j指向右孩子；否则还是左孩子
            j++;
            
        if(tmp.key < R[j].key)  // 最大孩子是否比双亲大
        {
            R[i] = R[j];    // 将R[j] 调整到双亲位置
            i = j;  // 修改i, j的值，以便继续向下筛选
            j = 2 * i;
        }
        else
            break;  // 双亲大，不再调整；也就说明已经调整完了，满足了大根堆定义
    }
    R[i] = tmp;
}                       
            
void HeapSort(RecType R[], int n)
{   int i;
    for(i = n/2; i >= 1; i--)       // 循环建立初始堆，调用sift算法 n/2 （向下取整）次
        sift(R, i, n);
        
    for(i = n; i >= 2; i--)
    {
        Swap(R[1], R[i]);
        sift(R, 1, i-1);    // 重新进行筛选，得到 i-1 个节点的堆
    }
}
```



## 10.5 归并排序
- **归并排序**(merge sort)：多次将两个或两个以上的有序表合并成一个新的有序表；先划分，再排序。
- **二路归并排序**(2-way merge sort)：多次将相邻两个的有序表合并成一个新有序表的排序方法，是最简单的归并排序

```C++
//自底向上的二路归并排序算法
// 1.一次二路归并，将两个相邻的有序子序列归并成为一个有序序列
void Merge(RecType R[], int low, int mid, int high)
{
	RecType* R1;
	int i = low, j = mid + 1, k = 0;  // k是R1的下标，i 和 j分别是第1,2段的下标

	R1 = (RecType*)malloc((high - low + 1) * sizeof(RecType));

	while (i <= mid && j <= high)
	{
		if (R[i].key <= R[j].key)  // 将第1段中的元素放入R1
		{
			R1[k] = R[i];
			i++; k++;
		}
		else  // 将第2段中的元素放入R1
		{
			R1[k] = R[j];
			j++; k++;
		}
	}

	while (i <= mid)  // 将第1段余下的部分复制到R1
	{
		R1[k] = R[i];
		i++; k++;
	}

	while (j <= high)  // 将第2段余下的部分复制到R1
	{
		R1[k] = R[j];
		j++; k++;
	}

	for (k = 0, i = low; i <= high; k++, i++)  // R1复制到R[low ... high]中
	{
		R[i] = R[k];
	}

	free(R1);
}

// 2. 进行一趟二路归并排序
void MergePass(RecType R[], int length, int n)
{
	int i;

	// 这里应该是已经进行了划分，通过i + 2 * length - 1
	for (i = 0; i + 2 * length - 1 < n; i += 2 * length)  // 归并length长的两相邻子表
	{
		Merge(R, i, i + length - 1, i + 2 * length - 1);

		if (i + length - 1 < n - 1)  // 余下两个子表，后者长度小于length
		{
			Merge(R, i, i + length - 1, n - 1);  // 归并这两个子表
		}
	}
}

// 3. 二路归并排序算法
void MergeSort(RecType R[], int n)
{
	int length;

	for (length = 1; length < n; length *= 2)
	{
		MergePass(R, length, n);  // log2n取上界趟
	}
}
```
- 用 **树** 结构来表示归并排序的过程可以形成 **二路归并树** 

```C++
// 自顶层向下的递归二路归并排序算法
void MergeSortDC(RecType R[],int low,int high)
//对R[low..high]进行二路归并排序
{
	int mid;
	if (low<high)
	{	mid=(low+high)/2;
		MergeSortDC(R,low,mid);
		MergeSortDC(R,mid+1,high);
		Merge(R,low,mid,high);		
	}
}

void MergeSort1(RecType R[],int n)	//自顶向下的二路归并算法
{
	MergeSortDC(R,0,n-1);
}

```

-  **二路归并**可以推广到 **多路归并** 
- [ ] 三路归并
```C++
// 三路归并
void
```
- [ ] k路归并
```C++
// k路归并
void
```

## 10.6 分布排序（改）
### 基数排序
- **基数排序**(radix  sort)是通过“分配”和“收集”过程来实现排序，不需要进行关键字间的比较，是一种借助于多关键字排序的思想对单关键字排序的方法。
- 基数排序有两种：最低位优先（LSD）和 最高位优先（MSD）
- 基数排序是一位一位地排序，适合**字符串排序** 和 **多关键字排序**。如有数学、语文成绩、排序方式：按数学成绩递减排序，数学成绩相同按语文成绩递减排序

```C++
//最低位优先的基数排序算法
#include <stdio.h>
#include <malloc.h>
#include <string.h>
#define MAXE 20			//线性表中最多元素个数
#define MAXR 10			//基数的最大取值
#define MAXD 8			//关键字位数的最大取值

typedef struct node
{	
	char data[MAXD];	//记录的关键字定义的字符串
    struct node *next;
} NodeType;

void CreaLink(NodeType *&p,char *a[],int n);
void DispLink(NodeType *p);
void RadixSort(NodeType *&p,int r,int d) //实现基数排序:p为待排序序列链表指针,r为基数,d为关键字位数
{
	NodeType *head[MAXR],*tail[MAXR],*t;//定义各链队的首尾指针
	int i,j,k;
	for (i=0;i<=d-1;i++)           		//从低位到高位循环
	{	
		// for (j=r-1;j>=0;j--) 
		for (j=0;j<r;j++) 				//初始化各链队首、尾指针
			head[j]=tail[j]=NULL;
		while (p!=NULL)          		//对于原链表中每个结点循环
		{	
			k=p->data[i]-'0';   		//找第k个链队
			if (head[k]==NULL)   		//进行分配
			{
				head[k]=p;
				tail[k]=p;
			}
          	else
			{
              	tail[k]->next=p;
				tail[k]=p;
			}
            p=p->next;             		//取下一个待排序的元素
		}
		
		p=NULL;							//重新用p来收集所有结点
       	for (j=0;j<r;j++)        		//对于每一个链队循环
		{
			if (head[j]!=NULL)         	//进行收集
			{	
				if (p==NULL)
				{
					p=head[j];
					t=tail[j];
				}
				else
				{
					t->next=head[j];
					t=tail[j];
				}
			}
			t->next=NULL;					//最后一个结点的next域置NULL
			printf("  按%s位排序\t",(i==0?"个":"十"));
			DispLink(p);
		}
	}
}
void CreateLink(NodeType *&p,char a[MAXE][MAXD],int n)	//采用后插法产生链表
{
	int i;
	NodeType *s,*t;
	for (i=0;i<n;i++)
	{
		s=(NodeType *)malloc(sizeof(NodeType));
		strcpy(s->data,a[i]);
		if (i==0)
		{
			p=s;t=s;
		}
		else
		{
			t->next=s;t=s;
		}
	}
	t->next=NULL;
}
void DispLink(NodeType *p)	//输出链表
{
	while (p!=NULL)
	{
		printf("%c%c ",p->data[1],p->data[0]);
		p=p->next;
	}
	printf("\n");
}
void main()
{
	int n=10,r=10,d=2;
	int i,j,k;
	NodeType *p;
	char a[MAXE][MAXD];
	int b[]={75,23,98,44,57,12,29,64,38,82};
	for (i=0;i<n;i++)		//将b[i]转换成字符串
	{
		k=b[i];
		for (j=0;j<d;j++)	//例如b[0]=75,转换后a[0][0]='7',a[0][1]='5'
		{
			a[i][j]=k%10+'0';
			k=k/10;
		}
		a[i][j]='\0';
	}
	CreateLink(p,a,n);
	printf("\n");
	printf("  初始关键字\t");		//输出初始关键字序列
	DispLink(p);
	RadixSort(p,10,2);
	printf("  最终结果\t");			//输出最终结果
	DispLink(p);
	printf("\n");
}
```

### 桶排序（补充）
- **桶排序**（Bucket Sort）是一种基于分布的排序算法，适用于将数据分布到有限数量的“桶”中，再对每个桶内的数据进行排序，然后合并桶内数据以得到有序序列。
```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

// 桶排序函数
void bucketSort(vector<float>& arr) {
    int n = arr.size();
    if (n <= 1) return; // 空数组或只有一个元素无需排序

    // 1. 创建空桶
    vector<vector<float>> buckets(n);

    // 2. 分配元素到对应的桶
    for (float num : arr) {
        int index = floor(num * n); // 根据数值计算桶索引 (num 范围假设在 [0, 1))
        buckets[index].push_back(num);
    }

    // 3. 对每个桶进行排序
    for (auto& bucket : buckets) {
        sort(bucket.begin(), bucket.end());
    }

    // 4. 合并所有桶的数据
    arr.clear();
    for (const auto& bucket : buckets) {
        arr.insert(arr.end(), bucket.begin(), bucket.end());
    }
}

int main() {
    // 测试数据
    vector<float> arr = {0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68};

    cout << "原始数组: ";
    for (float num : arr) cout << num << " ";
    cout << endl;

    // 调用桶排序
    bucketSort(arr);

    cout << "排序后数组: ";
    for (float num : arr) cout << num << " ";
    cout << endl;

    return 0;
}
```
- 时间复杂度：
    - 最好情况：𝑂(𝑛+𝑘)（桶划分均匀，桶内排序较快）
    - 最坏情况：𝑂(𝑛<sup>2</sup>)（所有数据集中到一个桶中）
    - 平均情况：𝑂(𝑛+𝑘)（k 为桶数）

- 空间复杂度：
    - O(𝑛+𝑘)，需要额外的桶存储空间

- 稳定性：
    - 桶排序本身是稳定的，但桶内排序方式会影响稳定性


## 10.7 各种排序算法的比较和选择

| 排序方法     | 时间复杂度（平均情况） | 时间复杂度（最坏情况） | 时间复杂度（最好情况） | 空间复杂度 | 稳定性 | 复杂性 |
| ------------ | --------------------- | --------------------- | --------------------- | ---------- | ------ | ------ |
| 直接插入排序 | \(O(n^2)\)            | \(O(n^2)\)            | \(O(n)\)              | \(O(1)\)   | 稳定   | 简单   |
| 折半插入排序 | \(O(n^2)\)            | \(O(n^2)\)            | \(O(n)\)              | \(O(1)\)   | 稳定   | 简单   |
| 希尔排序     | \(O(n^{1.3})\)        |                 	   |                       | \(O(1)\)   | 不稳定 | 较复杂 |
| 冒泡排序     | \(O(n^2)\)            | \(O(n^2)\)            | \(O(n)\)              | \(O(1)\)   | 稳定   | 简单   |
| 快速排序     | \(O(n \log n)\)       | \(O(n^2)\)            | \(O(n \log n)\)       | \(O(\log n)\) | 不稳定 | 较复杂 |
| 简单选择排序 | \(O(n^2)\)            | \(O(n^2)\)            | \(O(n^2)\)            | \(O(1)\)   | 不稳定 | 简单   |
| 堆排序       | \(O(n \log n)\)       | \(O(n \log n)\)       | \(O(n \log n)\)       | \(O(1)\)   | 不稳定 | 较复杂 |
| 二路归并排序 | \(O(n \log n)\)       | \(O(n \log n)\)       | \(O(n \log n)\)       | \(O(n)\)   | 稳定   | 较复杂 |
| 基数排序     | \(O(d(n + r))\)       | \(O(d(n + r))\)       | \(O(d(n + r))\)       | \(O(r)\)   | 稳定   | 较复杂 |
| 桶排序       | \(O(n+k)\)            | \(O(n^2)\)            | \(O(n+k)\)            | \(O(n+k)\) | 不稳定 | 较复杂 |

![sort_performance](res/sort_performance.png)
