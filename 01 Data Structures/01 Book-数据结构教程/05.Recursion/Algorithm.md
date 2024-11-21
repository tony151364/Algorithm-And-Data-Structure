## 课本例题

### 5.1
```C++
// 题目描述：以下是求n!（n为正整数）的递归函数，它属于什么类型的递归？
int fun (int n)
{
	if(n==1)
		return (1);
	else
		return (fun(n-1)*n);
} 
// 答：这个递归即是直接递归，又属于尾递归 
```

### 5.2
```C++
// 题目描述：采用递归算法求实数数组A[0..n-1]中的最小值
double Min(double A[], int index)
{
	if (index == 0)
	{
		return A[0];
	}
	else
	{
		double min = Min(A, i-1);

		return (min > A[i]) ? A[i] : min;
	}
}
```

### 5.3
```C++
// 题目描述：求顺序表(a1, a2, ..., an)中的最大值。
typedef struct 
{	
	ElemType data[MaxSize];		//存放顺序表元素
   	int length;					//存放顺序表的长度
} SqList;						//顺序表的类型

ElemType Max(SqList L,int i,int j)  //求顺序表L中最大元素
{
	int mid;
	ElemType max,max1,max2;
	if (i==j) 
		max=L.data[i];				//递归出口
	else
	{	
		mid=(i+j)/2;
		max1=Max(L,i,mid);			//递归调用1
		max2=Max(L,mid+1,j); 		//递归调用2
		max=(max1>max2)?max1:max2;
	}
	return(max);
}
```

### 5.4
```C++
// 题目描述：释放一个不带头结点的单链表L中所有结点。
#include "linklist.cpp"
void release(LinkNode *&L)
{
	if (L!=NULL)
	{
		release(L->next);
		free(L);
	}
}
int main()
{
	LinkNode *h;
	int a[]={1,2,3,4};
	InitList(h);		//h为带头结点的单链表
	ListInsert(h,1,1);
	ListInsert(h,2,2);
	ListInsert(h,3,3);
	ListInsert(h,4,4);
	printf("单链表:");
	DispList(h);
	printf("释放单链表\n");
	release(h->next);
	free(h);			//释放头结点
	return 1;
}

```
