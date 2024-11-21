# 第2章 线性表
## 2.1 线性表及其逻辑结构
### 2.1.1 线性表定义
- 线性表(linear list)是具有相同特性的数据元素的一个有限序列。

### 2.1.2 线性表的抽象数据类型类型描述

- 【例 2.2】假设有两个集合A和B，分别用两个线性表LA和LB表示，即线性表中的数据元素即为集合元素。利用线性表的基本运算算法求一个新的集合C=A∪B，即两个集合的并集放在线性表LC中。
```C++
void UnionList(List LA, List LB, List& LC)
{
	int lena, i;
	ElemType e;
	InitList(LC);  // 初始化LC

	for (i = 1; i <= ListLength(LA); i++)  // 将LA中的所有元素复制到LC中
	{
		GetElem(LA, i, e);
		ListInsert(LC, i, e);
	}

	lena = ListLength(LA);
	for(i = 1; i < ListLength(LB); i++)
	{
		GetElem(LB, i, e);
		if (!LocateElem(LA, e))
			ListInsert(LC, ++lena, 3);
	}
};
```

## 2.2 线性表的顺序存储结构

### 2.2.1 线性表的顺序存储结构——顺序表
- 线性表的顺序存储结构：把线性表中的素有元素按照顺序存储方法进行存储。可以理解为：按逻辑顺序依次存储到存储器中一片连续的存储空间中。
- 线性表的顺序存储结构简称为**顺序表**（Linear List）

```C++
#include <studio.h>
#include <malloc.h>
#define MaxSize 50

typedef int ElemType;
typedef struct{
	ElemType data[MaxSize];
	int length;
}SqList;
```
### 2.2.2 顺序表基本运算的实现
#### 1.建立线性表
```C++
//建立顺序表
void CreatList(SqList* &L, ElemType a[], int n)
{
	int i = 0,k = 0;
	L = (SqList *)malloc(sizeof(SqList));
	while(i<n)
	{
		L->data[k] = a[i];
		k++;i++;
	}
	L->length = k;
}
```
#### 2.顺序表基本运算算法
```C++
//初始化线性表。构造一个空的线性表L。
void InitList(SqList *&L)
{
	L = (SqList *)malloc(sizeof(SqList));
	L->length = 0;
}

//销毁线性表。释放线性表L占用的内存空间。
void DsetoryList(SqList * &L)
{
	free(L);
}

//判断线性表是否为空表。若L为空表，则返回真，否则返回假。
bool ListEmpty(SqList *L)
{
	return(L->length == 0); //时间复杂的为O（1）
}

//求线性表的长度。返回L中元素的个数
int ListLength(SqList *L)
{
	return(L->length);
}

//输出线性表。当线性表L不为空时，顺序显示L中各节点的值域。
void DispList(SqList *L)
{
	for(int i;i<L->length;i++)
		printf("%d",L->data[i]);
	printf("\n");
}

//求线性表中某个数据元素值。用e返回L中第i(1<= i<= n)个元素的值。
bool GetElem(SqList *L,int i,ElemType &e)
{
	if (i<1 || i>L->length)
		return false;
	e=L->data[i-1];
	return true;
}

//按元素值查找，返回L中第一个值域与e相等的元素的序号，若这样的元素不存在，则返回值为0.
int LocateElem(SqList *L, ElemType e)
{
	int i=0;
	while (i<L->length && L->data[i]!=e) i++;
	if (i>=L->length)
		return 0;
	else
		return i+1;
}

//插入数据元素就，在L的第i(1<= i<= n+1)个位置插入一个新的元素e，L的长度增1。
bool ListInsert(SqList *&L,int i,ElemType e)
{
	int j;
	if (i<1 || i>L->length+1)
		return false;
	i--;						//将顺序表位序转化为elem下标
	for (j=L->length;j>i;j--) 	//将data[i]及后面元素后移一个位置
		L->data[j]=L->data[j-1];
	L->data[i]=e;
	L->length++;				//顺序表长度增1
	return true;
}

//删除数据元素，删除L的第i(1<= i<= n)个元素，并用e返回其值，L的长度减1。
bool ListDelete(SqList *&L,int i,ElemType &e)
{
	int j;
	if (i<1 || i>L->length)
		return false;
	i--;						//将顺序表位序转化为elem下标
	e=L->data[i];
	for (j=i;j<L->length-1;j++)	//将data[i]之后的元素前移一个位置
		L->data[j]=L->data[j+1];
	L->length--;				//顺序表长度减1
	return true;
}

int main()
{
	SqList* h;
	int n = 5;
	ElemType a[] = {1, 2, 1, 4, 2};

	CreateList(h, a, n);
	// ...

	return 0;
}
```

#### 3.顺序表的应用示例
- 【例 2.3】假设一个线性表采用顺序表表示，设计一个算法，删除其中所有值等于x的元素，要求算法的时间复杂度为O(n)、空间复杂度为O(1)
```C++
// 解法一：
void delnode1(SqList* &L, ElemType x)
{
	int k=0, i;
	for (i=0; i<L->length;i++)
		if(L->data[i]!=x)
		{
			L->data[k]=L->data[i];
			k++;
		}
	L->length=k;
}
```
```C++
// 解法二：
void delnode2(SqList* &L, ElemType x)
{
	int k=0,i=0;
	while (i<L->lengyh)
	{
		if (L->data[i]==x)
			k++;
		else
			L->data[i-k]=L->data[i];
		i++;
	}
	L->length-=k;
}
```
- 【例 2.4】有一个顺序表L，假设元素类型ElemType为整型，设计一个尽可能高效的算法，以第一个元素为分界线（基准），将所有小于等于它的元素移到该基准的前面，将所有大于它的元素移到该基准的后面。
```C++
// 解法一：其实就是一次选择排序
void partition1(SqList* &L)
{
	int i=0,j=L->length-1;
	ElemType pivot=L->data[0];			//以data[0]为基准
	while (i<j)						    //从区间两端交替向中间扫描,直至i=j为止
	{	
		while (i<j && L->data[j]>pivot)
			j--;						//从右向左扫描,找一个小于等于pivot的元素
		while (i<j && L->data[i]<=pivot)
			i++;						//从左向右扫描,找一个大于pivot的元素
		if (i<j)
			swap(L->data[i],L->data[j]);//将L->data[i]和L->data[j]进行交换
	}
	swap(L->data[0],L->data[i]);		//将L->data[0]和L->data[i]进行交换
}
```
```C++
// 解法二：就是改了下元素交换的方式
void partition2(SqList *&L)
{	int i=0,j=L->length-1;
	ElemType pivot=L->data[0];	//以data[0]为基准
	while (i<j)  				//从顺序表两端交替向中间扫描,直至i=j为止
	{	
		while (j>i && L->data[j]>pivot)
			j--;				//从右向左扫描,找一个小于等于pivot的data[j]
		L->data[i]=L->data[j];	//找到这样的data[j],放入data[i]处
		while (i<j && L->data[i]<=pivot)
			i++;				//从左向右扫描,找一个大于pivot的记录data[i]
		L->data[j]=L->data[i];	//找到这样的data[i],放入data[j]处
	}
	L->data[i]=pivot;
}
```

- 【例 2.5】有一个顺序表L，假设元素类型ElemType为整型，设计一个尽可能高效的算法，将所有奇数移动到偶数的前面
```C++
// 解法一：类似2.4解法一的思路
void move1(SqList *&L)
{
	int i=0,j=L->length-1;
	while (i<j)
	{
		while (i<j && L->data[j]%2==0)
			j--;					//从右向左扫描,找一个奇数元素
		while (i<j && L->data[i]%2==1)
			i++;					//从左向右扫描,找一个偶数元素
		if (i<j)					//若i<j，将L->data[i]和L->data[j]交换
			swap(L->data[i],L->data[j]);
	}
}
```
```C++
// 解法二：类似于插入排序，构建一个“有序区”
void move2(SqList *&L)
{	int i=-1,j;
	for (j=0;j<=L->length-1;j++)
		if (L->data[j]%2==1)		//j指向奇数时
		{
			i++;					//奇数区间个数增1
			if (i!=j)				//若i、j不相等
				swap(L->data[i],L->data[j]);//L->data[i]和L->data[j]交换
		}
}
```
## 2.3 线性表的链式存储结构
- 线性表每个元素只有一个前驱元素和后继元素
### 2.3.1 线性表的链式存储结构——链表
### 2.3.2 单链表
```C++
typedef int ElemType;
typedef struct LNode  
{
	ElemType data;
	struct LNode *next;		//指向后继结点
} LinkNode;					//声明单链表结点类型
```
单链表增加一个头结点的优点：
- 第一个结点的操作和表中其他节点的操作一致，无需进行特殊处理。
- 无论链表是否为空，都有一个头结点，因此空表和非空表的处理也就统一了

#### 1. 插入和删除节点的操作
#### 2. 建立单链表
- 头插法
```C++
void CreateListF(LinkNode *&L,ElemType a[],int n)
//头插法建立单链表
{
	LinkNode *s;
	L=(LinkNode *)malloc(sizeof(LinkNode));  	//创建头结点
	L->next=NULL;
	for (int i=0;i<n;i++)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));//创建新结点s
		s->data=a[i];
		s->next=L->next;			//将结点s插在原开始结点之前,头结点之后
		L->next=s;
	}
}
```
- 尾插法
```C++
void CreateListR(LinkNode *&L,ElemType a[],int n)
//尾插法建立单链表
{
	LinkNode *s,*r;
	L=(LinkNode *)malloc(sizeof(LinkNode));  	//创建头结点
	L->next=NULL;
	r=L;					//r始终指向终端结点,开始时指向头结点
	for (int i=0;i<n;i++)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));//创建新结点s
		s->data=a[i];
		r->next=s;			//将结点s插入结点r之后
		r=s;
	}
	r->next=NULL;			//终端结点next域置为NULL
}
```

#### 3.线性表基本运算在单链表中的实现
```C++
//初始化线性表,时间复杂度为O(1)
void InitList(LinkNode *&L)
{
	L=(LinkNode *)malloc(sizeof(LinkNode));  	//创建头结点
	L->next=NULL;
}

//摧毁线性表，时间复杂度为O(n)
void DestroyList(LinkNode *&L)
{
	LinkNode *pre=L,*p=pre->next;
	while (p!=NULL)
	{	free(pre);
		pre=p;
		p=pre->next;
	}
	free(pre);	//此时p为NULL,pre指向尾结点,释放它
}

//判断线性表是否为空表
bool ListEmpty(LinkNode *L)
{
	return(L->next == NULL);
}

//求线性表的长度，时间复杂度为O(n)
int ListLength(LinkNode *L)
{
	int length=0;
	LinkNode *p=L;   //p指向头结点，n置为0（头结点的序号为0）
	while(p->next != NULL)
	{
		length++;
		p = p->next;
	}
	return(length);  //头结点不算在长度里边
}

//输出线性表Displist
void DispList(LinkNode *L)
{
	LinkNode *p=L->next;
	while (p!=NULL)
	{	printf("%d ",p->data);
		p=p->next;
	}
	printf("\n");
}

//求线性表中某个元素的值
bool GetElem(LinkNode *L,int i,ElemType &e)
{
	int j=0;
	LinkNode *p=L;
	if (i<=0) return false;		//i错误返回假
	while (j<i && p!=NULL)
	{	j++;
		p=p->next;
	}
	if (p==NULL)				//不存在第i个数据结点
		return false;
	else						//存在第i个数据结点
	{	e=p->data;
		return true;
	}
	// 链表不再具有随机存储特性
}

//按元素值查找
int LocateElem(LinkNode *L,ElemType e)
{
	LinkNode *p=L->next;
	int n=1;
	while (p!=NULL && p->data!=e)
	{	p=p->next;
		n++;
	}
	if (p==NULL)
		return(0);
	else
		return(n);
}

//插入数据元素
bool ListInsert(LinkNode *&L,int i,ElemType e)
{
	int j=0;
	LinkNode *p=L,*s;
	if (i<=0) return false;			//i错误返回假
	while (j<i-1 && p!=NULL)		//查找第i-1个结点p
	{	j++;
		p=p->next;
	}
	if (p==NULL)					//未找到位序为i-1的结点
		return false;
	else							//找到位序为i-1的结点*p
	{	s=(LinkNode *)malloc(sizeof(LinkNode));//创建新结点*s
		s->data=e;
		s->next=p->next;			//将s结点插入到结点p之后
		p->next=s;
		return true;
	}
}

//删除数据元素
bool ListDelete(LinkNode *&L,int i,ElemType &e)
{
	int j=0;
	LinkNode *p=L,*q;
	if (i<=0) return false;		//i错误返回假
	while (j<i-1 && p!=NULL)	//查找第i-1个结点
	{	j++;
		p=p->next;
	}
	if (p==NULL)				//未找到位序为i-1的结点
		return false;
	else						//找到位序为i-1的结点p
	{	q=p->next;				//q指向要删除的结点
		if (q==NULL) 
			return false;		//若不存在第i个结点,返回false
		e=q->data;
		p->next=q->next;		//从单链表中删除q结点
		free(q);				//释放q结点
		return true;
	}
}
```

#### 4.单链表的应用示例
- 【例 2.6】有一个带头结点的单链表L=(a1, b1, a2, b2, ..., an, bn)，设计一个算法将其拆分成两个带头结点的单链表L1和L2，其中L1=(a1, a2, ..., an)，L2=(b1, b2, ..., bn)，要求L1使用L的头结点。
```C++
void split(LinkNode *&L,LinkNode *&L1,LinkNode *&L2)
{	
	LinkNode *p=L->next,*q,*r1;	//p指向第1个数据结点
	L1=L;					//L1利用原来L的头结点
	r1=L1;					//r1始终指向L1的尾结点
	L2=(LinkNode *)malloc(sizeof(LinkNode));	//创建L2的头结点
	L2->next=NULL;			//置L2的指针域为NULL
	while (p!=NULL)
	{	
		r1->next=p;			//采用尾插法将结点p(data值为ai)插入L1中
		r1=p;
		p=p->next;			//p移向下一个结点(data值为bi)
		q=p->next;			//由于头插法修改p的next域,故用q保存结点p的后继结点
		p->next=L2->next;	//采用头插法将结点p插入L2中
		L2->next=p;
		p=q;				//p重新指向ai+1的结点
	}
	r1->next=NULL;			//尾结点next置空
```

- 【例 2.7】 设计一个算法，删除一个单链表L中元素值最大的节点。（假设这样的节点唯一）
```C++

```

- 【例 2.8】有一个带头结点的单链表L（至少有一个数据节点），设计一个算法让其元素递增有序排列
```C++

```

### 2.3.3 双链表
- 双链表的优点：  
	- 从任一节点出发可以快速找到其前驱结点和后继结点
	- 从任一节点出发可以访问其他节点
- 与单链表相比，双链表主要是**插入**和**删除**运算不同
```C++
//双链表基本运算算法
#include <stdio.h>
#include <malloc.h>
typedef int ElemType;
typedef struct DNode		//定义双链表结点类型
{
	ElemType data;
	struct DNode *prior;	//指向前驱结点
	struct DNode *next;		//指向后继结点
} DLinkNode;

void CreateListF(DLinkNode *&L,ElemType a[],int n)
//头插法建双链表
{
	DLinkNode *s;
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  	//创建头结点
	L->prior=L->next=NULL;
	for (int i=0;i<n;i++)
	{	
		s=(DLinkNode *)malloc(sizeof(DLinkNode));//创建新结点
		s->data=a[i];
		s->next=L->next;			//将结点s插在原开始结点之前,头结点之后
		if (L->next!=NULL) 
			L->next->prior=s;
		L->next=s;
		s->prior=L;
	}
}

void CreateListR(DLinkNode *&L,ElemType a[],int n)
//尾插法建双链表
{
	DLinkNode *s,*r;
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  	//创建头结点
	L->prior=L->next=NULL;
	r=L;					//r始终指向终端结点,开始时指向头结点
	for (int i=0;i<n;i++)
	{	
		s=(DLinkNode *)malloc(sizeof(DLinkNode));//创建新结点
		s->data=a[i];
		r->next=s;
		s->prior=r;	//将结点s插入结点r之后
		r=s;
	}
	r->next=NULL;				//尾结点next域置为NULL
}

void InitList(DLinkNode *&L)
{
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  	//创建头结点
	L->prior=L->next=NULL;
}

void DestroyList(DLinkNode *&L)
{
	DLinkNode *pre=L,*p=pre->next;
	while (p!=NULL)
	{
		free(pre);
		pre=p;
		p=pre->next;
	}
	free(pre);
}

bool ListEmpty(DLinkNode *L)
{
	return(L->next==NULL);
}

int ListLength(DLinkNode *L)
{
	DLinkNode *p=L;
	int i=0;
	while (p->next!=NULL)
	{
		i++;
		p=p->next;
	}
	return(i);
}

void DispList(DLinkNode *L)
{
	DLinkNode *p=L->next;
	while (p!=NULL)
	{
		printf("%d ",p->data);
		p=p->next;
	}
	printf("\n");
}

bool GetElem(DLinkNode *L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p=L;
	if (i<=0) return false;		//i错误返回假
	while (j<i && p!=NULL)
	{
		j++;
		p=p->next;
	}
	if (p==NULL)
		return false;
	else
	{
		e=p->data;
		return true;
	}
}

int LocateElem(DLinkNode *L,ElemType e)
{
	int n=1;
	DLinkNode *p=L->next;
	while (p!=NULL && p->data!=e)
	{
		n++;
		p=p->next;
	}
	if (p==NULL)
		return(0);
	else
		return(n);
}

// 另外解法：在双链表中，也可以直接查找第i个结点，在它前面插入
bool ListInsert(DLinkNode *&L,int i,ElemType e)
{
	int j=0;
	DLinkNode *p=L,*s;
	if (i<=0) return false;		//i错误返回假
	while (j<i-1 && p!=NULL)
	{
		j++;
		p=p->next;
	}
	if (p==NULL)				//未找到第i-1个结点
		return false;
	else						//找到第i-1个结点p
	{
		s=(DLinkNode *)malloc(sizeof(DLinkNode));	//创建新结点s
		s->data=e;	
		s->next=p->next;		//将结点s插入到结点p之后
		if (p->next!=NULL) 
			p->next->prior=s;
		s->prior=p;
		p->next=s;
		return true;
	}
}

bool ListDelete(DLinkNode *&L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p=L,*q;
	if (i<=0) 
		return false;		//i错误返回假
	while (j<i-1 && p!=NULL)
	{
		j++;
		p=p->next;
	}
	if (p==NULL)				//未找到第i-1个结点
		return false;
	else						//找到第i-1个结点p
	{
		q=p->next;				//q指向要删除的结点
		if (q==NULL) 
			return false;		//不存在第i个结点
		e=q->data;
		p->next=q->next;		//从单链表中删除*q结点
		if (p->next!=NULL) 
			p->next->prior=p;
		free(q);				//释放q结点
		return true;
	}
}
```
- 【例 2.9】有一个带头结点的双链表L，设计一个算法
```C++

```
- 【例 2.10】
```C++

```

### 2.3.4 循环链表
- 与单链表相比，循环单链表：
	- 链表中没有空指针域
	- p所指结点为尾结点的条件：p->next == L
```C++
//循环单链表基本运算算法
#include <stdio.h>
#include <malloc.h>
typedef int ElemType;
typedef struct LNode		//定义单链表结点类型
{
	ElemType data;
    struct LNode *next;
} LinkNode;

void CreateListF(LinkNode *&L,ElemType a[],int n)
//头插法建立循环单链表
{
	LinkNode *s;int i;
	L=(LinkNode *)malloc(sizeof(LinkNode));  	//创建头结点
	L->next=NULL;
	for (i=0;i<n;i++)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));//创建新结点
		s->data=a[i];
		s->next=L->next;			//将结点s插在原开始结点之前,头结点之后
		L->next=s;
	}
	s=L->next;	
	while (s->next!=NULL)			//查找尾结点,由s指向它
		s=s->next;
	s->next=L;						//尾结点next域指向头结点
}

void CreateListR(LinkNode *&L,ElemType a[],int n)
//尾插法建立循环单链表
{
	LinkNode *s,*r;int i;
	L=(LinkNode *)malloc(sizeof(LinkNode));  	//创建头结点
	L->next=NULL;
	r=L;					//r始终指向终端结点,开始时指向头结点
	for (i=0;i<n;i++)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));//创建新结点
		s->data=a[i];
		r->next=s;			//将结点s插入结点r之后
		r=s;
	}
	r->next=L;				//尾结点next域指向头结点
}
void InitList(LinkNode *&L)
{
	L=(LinkNode *)malloc(sizeof(LinkNode));	//创建头结点
	L->next=L;
}
void DestroyList(LinkNode *&L)
{
	LinkNode *p=L,*q=p->next;
	while (q!=L)
	{
		free(p);
		p=q;
		q=p->next;
	}
	free(p);
}
bool ListEmpty(LinkNode *L)
{
	return(L->next==L);
}
int ListLength(LinkNode *L)
{
	LinkNode *p=L;int i=0;
	while (p->next!=L)
	{
		i++;
		p=p->next;
	}
	return(i);
}
void DispList(LinkNode *L)
{
	LinkNode *p=L->next;
	while (p!=L)
	{
		printf("%d ",p->data);
		p=p->next;
	}
	printf("\n");
}
bool GetElem(LinkNode *L,int i,ElemType &e)
{
	int j=0;
	LinkNode *p;
	if (L->next!=L)		//单链表不为空表时
	{
		if (i==1)
		{
			e=L->next->data;
			return true;
		}
		else			//i不为1时
		{
			p=L->next;
			while (j<i-1 && p!=L)
			{
				j++;
				p=p->next;
			}
			if (p==L)
				return false;
			else
			{
				e=p->data;
				return true;
			}
		}
	}
	else				//单链表为空表时
		return false;
}
int LocateElem(LinkNode *L,ElemType e)
{
	LinkNode *p=L->next;
	int n=1;
	while (p!=L && p->data!=e)
	{
		p=p->next;
		n++;
	}
	if (p==L)
		return(0);
	else
		return(n);
}
bool ListInsert(LinkNode *&L,int i,ElemType e)
{
	int j=0;
	LinkNode *p=L,*s;
	if (p->next==L || i==1)		//原单链表为空表或i==1时
	{
		s=(LinkNode *)malloc(sizeof(LinkNode));	//创建新结点s
		s->data=e;								
		s->next=p->next;		//将结点s插入到结点p之后
		p->next=s;
		return true;
	}
	else
	{
		p=L->next;
		while (j<i-2 && p!=L)
		{
			j++;
			p=p->next;
		}
		if (p==L)				//未找到第i-1个结点
			return false;
		else					//找到第i-1个结点p
		{
			s=(LinkNode *)malloc(sizeof(LinkNode));	//创建新结点s
			s->data=e;								
			s->next=p->next;						//将结点s插入到结点p之后
			p->next=s;
			return true;
		}
	}
}
bool ListDelete(LinkNode *&L,int i,ElemType &e)
{
	int j=0;
	LinkNode *p=L,*q;
	if (p->next!=L)					//原单链表不为空表时
	{
		if (i==1)					//i==1时
		{
			q=L->next;				//删除第1个结点
			e=q->data;
			L->next=q->next;
			free(q);
			return true;
		}
		else						//i不为1时
		{
			p=L->next;
			while (j<i-2 && p!=L)
			{
				j++;
				p=p->next;
			}
			if (p==L)				//未找到第i-1个结点
				return false;
			else					//找到第i-1个结点p
			{
				q=p->next;			//q指向要删除的结点
				e=q->data;
				p->next=q->next;	//从单链表中删除q结点
				free(q);			//释放q结点
				return true;
			}
		}
	}
	else return false;
}

```
- 循环双链表：两个环
```C++
//循环双链表基本运算算法
#include <stdio.h>
#include <malloc.h>
typedef int ElemType;
typedef struct DNode		//定义双链表结点类型
{
	ElemType data;
	struct DNode *prior;	//指向前驱结点
	struct DNode *next;		//指向后继结点
} DLinkNode;
void CreateListF(DLinkNode *&L,ElemType a[],int n)
//头插法建立循环双链表
{
	DLinkNode *s;int i;
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  	//创建头结点
	L->next=NULL;
	for (i=0;i<n;i++)
	{	
		s=(DLinkNode *)malloc(sizeof(DLinkNode));//创建新结点
		s->data=a[i];
		s->next=L->next;			//将结点s插在原开始结点之前,头结点之后
		if (L->next!=NULL) L->next->prior=s;
		L->next=s;s->prior=L;
	}
	s=L->next;	
	while (s->next!=NULL)			//查找尾结点,由s指向它
		s=s->next;
	s->next=L;						//尾结点next域指向头结点
	L->prior=s;						//头结点的prior域指向尾结点

}
void CreateListR(DLinkNode *&L,ElemType a[],int n)
//尾插法建立循环双链表
{
	DLinkNode *s,*r;int i;
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  //创建头结点
	L->next=NULL;
	r=L;					//r始终指向尾结点,开始时指向头结点
	for (i=0;i<n;i++)
	{	
		s=(DLinkNode *)malloc(sizeof(DLinkNode));//创建新结点
		s->data=a[i];
		r->next=s;s->prior=r;	//将结点s插入结点r之后
		r=s;
	}
	r->next=L;				//尾结点next域指向头结点
	L->prior=r;				//头结点的prior域指向尾结点
}
void InitList(DLinkNode *&L)
{
	L=(DLinkNode *)malloc(sizeof(DLinkNode));  	//创建头结点
	L->prior=L->next=L;
}
void DestroyList(DLinkNode *&L)
{
	DLinkNode *p=L,*q=p->next;
	while (q!=L)
	{
		free(p);
		p=q;
		q=p->next;
	}
	free(p);
}
bool ListEmpty(DLinkNode *L)
{
	return(L->next==L);
}
int ListLength(DLinkNode *L)
{
	DLinkNode *p=L;
	int i=0;
	while (p->next!=L)
	{
		i++;
		p=p->next;
	}
	return(i);
}
void DispList(DLinkNode *L)
{
	DLinkNode *p=L->next;
	while (p!=L)
	{
		printf("%d ",p->data);
		p=p->next;
	}
	printf("\n");
}
bool GetElem(DLinkNode *L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p;
	if (L->next!=L)		//双链表不为空表时
	{
		if (i==1)
		{
			e=L->next->data;
			return true;
		}
		else			//i不为1时
		{
			p=L->next;
			while (j<i-1 && p!=L)
			{
				j++;
				p=p->next;
			}
			if (p==L)
				return false;
			else
			{
				e=p->data;
				return true;
			}
		}
	}
	else				//双链表为空表时
		return 0;
}
int LocateElem(DLinkNode *L,ElemType e)
{
	int n=1;
	DLinkNode *p=L->next;
	while (p!=NULL && p->data!=e)
	{
		n++;
		p=p->next;
	}
	if (p==NULL)
		return(0);
	else
		return(n);
}
bool ListInsert(DLinkNode *&L,int i,ElemType e)
{
	int j=0;
	DLinkNode *p=L,*s;
	if (p->next==L)					//原双链表为空表时
	{	
		s=(DLinkNode *)malloc(sizeof(DLinkNode));	//创建新结点s
		s->data=e;
		p->next=s;s->next=p;
		p->prior=s;s->prior=p;
		return true;
	}
	else if (i==1)					//原双链表不为空表但i=1时
	{
		s=(DLinkNode *)malloc(sizeof(DLinkNode));	//创建新结点s
		s->data=e;
		s->next=p->next;p->next=s;	//将结点s插入到结点p之后
		s->next->prior=s;s->prior=p;
		return true;
	}
	else
	{	
		p=L->next;
		while (j<i-2 && p!=L)
		{	j++;
			p=p->next;
		}
		if (p==L)				//未找到第i-1个结点
			return false;
		else					//找到第i-1个结点*p
		{
			s=(DLinkNode *)malloc(sizeof(DLinkNode));	//创建新结点s
			s->data=e;	
			s->next=p->next;	//将结点s插入到结点p之后
			if (p->next!=NULL) p->next->prior=s;
			s->prior=p;
			p->next=s;
			return true;
		}
	}
}
bool ListDelete(DLinkNode *&L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p=L,*q;
	if (p->next!=L)					//原双链表不为空表时
	{	
		if (i==1)					//i==1时
		{	
			q=L->next;				//删除第1个结点
			e=q->data;
			L->next=q->next;
			q->next->prior=L;
			free(q);
			return true;
		}
		else						//i不为1时
		{	
			p=L->next;
			while (j<i-2 && p!=NULL)		
			{
				j++;
				p=p->next;
			}
			if (p==NULL)				//未找到第i-1个结点
				return false;
			else						//找到第i-1个结点p
			{
				q=p->next;				//q指向要删除的结点
				if (q==NULL) return 0;	//不存在第i个结点
				e=q->data;
				p->next=q->next;		//从单链表中删除q结点
				if (p->next!=NULL) p->next->prior=p;
				free(q);				//释放q结点
				return true;
			}
		}
	}
	else return false;					//原双链表为空表时
}

```
- 【例 2.11】
```C++

```
- 【例 2.12】
```C++

```
- 【例 2.13】
```C++

```

## 四、线性表的应用
- 顺序表和链表的混合使用
```C++
//线性表的应用:两个表的简单自然连接的算法
#include <stdio.h>
#include <malloc.h>
#define MaxCol  10			//最大列数
typedef int ElemType;
typedef struct Node1		//数据结点类型
{	
	ElemType data[MaxCol];
	struct Node1 *next;		//指向后继数据结点
} DList;
typedef struct Node2		//头结点类型
{	
	int Row,Col;			//行数和列数
	DList *next;			//指向第一个数据结点
} HList;

// 交互式创建单链表（尾插法）
// 与一般尾插法建表的不同：因为头结点与数据结点类型不同！r不能指向头结点
void CreateTable(HList *&h)
{
	int i,j;
	DList *r,*s;
	h=(HList *)malloc(sizeof(HList));		//创建头结点
	h->next=NULL;
	printf("表的行数,列数:");
	scanf("%d%d",&h->Row,&h->Col);
	for (i=0;i<h->Row;i++)
	{	
		printf("  第%d行:",i+1);
		s=(DList *)malloc(sizeof(DList));	//创建数据结点
		for (j=0;j<h->Col;j++)				//输入一行的数据初步统计
			scanf("%d",&s->data[j]);
		if (h->next==NULL)					//插入第一个数据结点
			h->next=s;
		else								//插入其他数据结点
			r->next=s;						//将结点s插入到结点r结点之后
		r=s;								//r始终指向最后一个数据结点
	}
	r->next=NULL;							//表尾结点next域置空
}

// 销毁单链表
void DestroyTable(HList* &h)
{
	DList *pre=h->next, *p=pre->next;
	while (p!=NULL)
	{
		free(pre);
		pre=p;
		p=p->next;
	}
	free(pre);
	free(h);
}

// 输出单链表
void DispTable(HList *h)
{
	int j;
	DList *p=h->next;
	while (p!=NULL)
	{	
		for (j=0;j<h->Col;j++)
			printf("%4d",p->data[j]);
		printf("\n");
		p=p->next;
	}
}

// 实现两个单链表的自然连接
void LinkTable(HList *h1,HList *h2,HList *&h)
{
	int f1,f2,i;
	DList *p=h1->next,*q,*s,*r;
	printf("连接字段是:第1个表位序,第2个表位序:");
	scanf("%d%d",&f1,&f2);
	h=(HList *)malloc(sizeof(HList));
	h->Row=0;
	h->Col=h1->Col+h2->Col;
	h->next=NULL;
	while (p!=NULL)
	{	q=h2->next;
		while (q!=NULL)
		{	if (p->data[f1-1]==q->data[f2-1])		//对应字段值相等
			{	s=(DList *)malloc(sizeof(DList));	//创建一个数据结点
				for (i=0;i<h1->Col;i++)				//复制表1的当前行
					s->data[i]=p->data[i];
				for (i=0;i<h2->Col;i++)
					s->data[h1->Col+i]=q->data[i];	//复制表2的当前行
				if (h->next==NULL)				//插入第一个数据结点
					h->next=s;
				else							//插入其他数据结点
					r->next=s;
				r=s;							//r始终指向最后数据结点
				h->Row++;						//表行数增1
			}
			q=q->next;							//表2下移一个记录
		}
		p=p->next;								//表1下移一个记录
	}
	r->next=NULL;								//表尾结点next域置空
}

int main()
{
	HList *h1,*h2,*h;
	printf("表1:\n");	
	CreateTable(h1);			//创建表1
	printf("表2:\n");  
	CreateTable(h2);			//创建表2
	LinkTable(h1,h2,h);			//连接两个表
	printf("连接结果表:\n");	
	DispTable(h);				//输出连接结果
	DestroyTable(h1);			//销毁单链表h1
	DestroyTable(h2);			//销毁单链表h2
	DestroyTable(h);			//销毁单链表h
	return 1;
}
```
## 五、有序表
- 所谓**有序表**（ordered list）是指所有元素按递增或递减方式有序排列的线性表。其区别主要在于运算实现不同。
- **有序表**是逻辑层面的概念。**顺序表**是物理层面的概念。
- 有序表采用顺序表存储叫做**有序顺序表**，采用链表存储叫做**有序链表**。

### 2.5.2 有序表的顺序存储结构及其基本运算
- 只有插入算法有所差异
```C++
// 顺序表插入元素
void ListInsert(SqList *&L，ElemType e)
{     int i=0，j;
      while (i<L->length && L->data[i]<e)
		i++;						//查找值为e的元素
      for (j=ListLength(L);j>i;j--)	//将data[i..n]后移一个位置
		L->data[j]=L->data[j-1]; 
      L->data[i]=e;
      L->length++;					//有序顺序表长度增1
}

// 单链表插入元素
void ListInsert(LinkNode *&L，ElemType e)
{  
	LinkNode *pre=L，*p;
    while (pre->next!=NULL && pre->next->data<e)
		pre=pre->next; 				//查找插入结点的前驱结点*pre
	p=(LinkNode *)malloc(sizeof(LinkNode));
	p->data=e;						//创建存放e的数据结点*p
	p->next=pre->next;				//在*pre结点之后插入*p结点
	pre->next=p;
}
```
### 2.5.3 有序表的归并算法
- 【例 2.14】 假设有两个有序表LA和LB，设计一个算法，将它们合并成一个有序表LC（假设每个有序表中和两个有序表间均不存在重复元素），要求不破坏原有表LA和LB。

```C++
// 采用顺序表存放有序表时的二路归并算法如下：
SqList* UnionList(SqList *LA,SqList *LB,SqList *&LC)
{
	int i=0,j=0,k=0;	//i、j、k分别作为LA、LB、LC的下标
	LC=(SqList *)malloc(sizeof(SqList));
	LC->length=0;
	while (i<LA->length && j<LB->length)
	{	
		if (LA->data[i]<LB->data[j])
		{	
			LC->data[k]=LA->data[i];
			i++;k++;
		}
		else				//LA->data[i]>LB->data[j]
		{	
			LC->data[k]=LB->data[j];
			j++;k++;
		}
	}
	while (i<LA->length)	//LA尚未扫描完,将其余元素插入LC中
	{	
		LC->data[k]=LA->data[i];
		i++;k++;
	}
	while (j<LB->length)  //LB尚未扫描完,将其余元素插入LC中
	{	
		LC->data[k]=LB->data[j];
		j++;k++;
	} 
	LC->length=k;
}
```
```C++
// 采用单链表存放有序表时的二路归并算法如下：
void UnionList1(LinkNode *LA,LinkNode *LB,LinkNode *&LC)
{
	LinkNode *pa=LA->next,*pb=LB->next,*pc,*s;
	LC=(LinkNode *)malloc(sizeof(LinkNode));		//创建LC的头结点
	pc=LC;							//pc始终指向LC的最后一个结点
	while (pa!=NULL && pb!=NULL)
	{	
		if (pa->data<pb->data)
		{	
			s=(LinkNode *)malloc(sizeof(LinkNode));//复制pa结点
			s->data=pa->data;
			pc->next=s;pc=s;		//采用尾插法将结点s插入到LC的最后
			pa=pa->next;
		} 
		else
		{	
			s=(LinkNode *)malloc(sizeof(LinkNode));//复制pb结点
			s->data=pb->data;
			pc->next=s;pc=s;		//采用尾插法将结点s插入到LC的最后
			pb=pb->next;
		}
	}
	while (pa!=NULL)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));	//复制pa结点
		s->data=pa->data;
		pc->next=s;pc=s;		//采用尾插法将结点s插入到LC的最后
		pa=pa->next;
	}
	while (pb!=NULL)
	{	
		s=(LinkNode *)malloc(sizeof(LinkNode));	//复制pa结点
		s->data=pb->data;
		pc->next=s;pc=s;		//采用尾插法将结点s插入到LC的最后
		pb=pb->next;
	}
	pc->next=NULL;
}
```

### 2.5.4 有序表的应用                                            
- 【例 2.15】已知3个带头结点的单链表LA、LB和LC中的节点元素均依元素值递增排列（假设每个单链表不存在数据值相同的结点，但3个单链表中可能存在数据值相同的结点），设计一个算法，对LA链表进行如下操作：使操作后的链表LA中仅留下3个表中均包含的数据元素的节点，且没有数据值相同的节点，并释放LA中所有的无用节点。要求算法的时间复杂度为O(m + n + p)，其中m、n和p分别为3个表的长度。
```C++
void Commnode(LinkNode *&LA,LinkNode *LB,LinkNode *LC)
{
	LinkNode *pa=LA->next,*pb=LB->next,*pc=LC->next,*q,*r;
	LA->next=NULL;  		//此时LA作为新建单链表的头结点
	r=LA;					//r始终指向新单链表最后一个结点
	while (pa!=NULL)		//查找均包含的公共结点并建立新链表
	{	
		while (pb!=NULL && pa->data>pb->data) //pa所指结点与LB中结点进行比较
			pb=pb->next;
		while (pc!=NULL && pa->data>pc->data) //pa所指结点与LC中结点进行比较
			pc=pc->next;
		if (pb!=NULL && pc!=NULL && pa->data==pb->data && pa->data==pc->data) //pa所指结点是公共结点
		{	
			r->next=pa;			//将*pa结点插入到LA中
			r=pa;		
			pa=pa->next;		//pa移到下一个结点
		}
		else               		//pa所指结点不是公共结点,则删除之
		{	
			q=pa;
			pa=pa->next;		//pa移到下一个结点
			free(q);			//释放非公共结点
		}
	}
	r->next=NULL;			//将新建单链表尾结点的next域置空
}
```

- 【例 2.16】已知一个有序单链表L(允许出现值域重复的结点)，设计一个高效算法删除值域重复的结点，并分析算法的时间复杂度。
```C++
void dels(LinkNode *&L)
{
	LinkNode *p=L->next,*q;
	while (p->next!=NULL) 
	{
		if (p->data==p->next->data)		//找到重复值的结点
		{
			q=p->next;					//q指向这个重复值的结点
			p->next=q->next;			//删除*q结点
			free(q);
		}
		else							//不是重复结点,p指针下移
			p=p->next;
	}
}
```

- 【例 2.17】一个长度为L（L≥1）的升序序列S，处在第[L/2]个位置的数称为S的中位数。
  例如：若序列S1=(11，13，15，17，19)，则S1的中位数是15。
两个序列的中位数是含它们所有元素的升序序列的中位数。例如，若S2=(2，4，6，8，20)，则S1和S2的中位数是11。
现有两个等长的升序序列A和B，试设计一个在时间和空间两方面都尽可能高效的算法，找出两个序列A和B的中位数。
```C++
ElemType M_Search(SqList *A, SqList *B)	//A、B的长度相同
{	
	int i=0, j=0, k=0;
	while (i<A->length && j<B->length)	//两个序列均没有扫描完
	{	k++;							//当前归并的元素个数增1
		if (A->data[i]<B->data[j])		//归并较小的元素A->data[i]
		{	if (k==A->length)			//若当前归并的元素是第n个元素
				return A->data[i];		//返回A->data[i]
			i++;
		}
		else							//归并较小的元素B->data[j]
		{	if (k==B->length)			//若当前归并的元素是第n个元素
				return B->data[j];		//返回B->data[j]
			j++; 
		}
	}
}

// 半天没看到线性表是等长的
```