## 课本例题

### 6.1
```C++
/*【例6.1】利用数组求解约瑟夫问题:设有n个人站成一圈，其编号
从1~n。从编号为1的人开始按顺时针方向“1，2，3，4，..”循环报数
数到m的人出列，然后从出列者的下一个人重新开始报数，数到m的人又
出列，如此重复进行，直到n个人都出列为止。要求输出这n个人的出列
顺序。*/
#include <stdio.h>
#define MaxSize 100
void josephus(int n,int m)
{
	int p[MaxSize];
	int i,j,t;
	for (i=0;i<n;i++)			//构建初始序列
		p[i]=i+1;
    
	t=0;						//首次报数的起始位置
	printf("出列顺序:");
	for (i=n;i>=1;i--)			//i为数组p中的人数
	{
		t=(t+m-1)%i;			//t为出列者的编号
		printf("%d ",p[t]);		//编号为t的元素出列
		for (j=t+1;j<=i-1;j++)	//后面的元素前移一个位置
			p[j-1]=p[j];
	}
	printf("\n");
}

int main()
{
	josephus(8,4);
	return 1;
}
```

### 6.2
```C++
/*
*/
```

### 6.3
- 两种方法殊途同归
```C++
//【例6.3】的算法：求广义表g的原子个数
#include "glist.cpp"

//解法1的方法
int Count1(GLNode *g)					//求广义表g的原子个数
{	int n=0;
	GLNode *g1=g->val.sublist;
	while (g1!=NULL)					//对每个元素进行循环处理
	{	if (g1->tag==0)					//为原子时
			n++;						//原子个数增1
		else							//为子表时
			n+=Count1(g1);				//累加元素的原子个数

		g1=g1->link;					//累加兄弟的原子个数
	}
	return n;							//返回总原子个数
}

//解法2的方法
int Count2(GLNode *g)					//求广义表g的原子个数
{	int n=0;
	if (g!=NULL)						//对每个元素进行循环处理
	{	
		if (g->tag==0)					//为原子时
			n++;						//原子个数增1
		else							//为子表时
			n+=Count2(g->val.sublist);	//累加元素的原子个数
		n+=Count2(g->link);				//累加兄弟的原子个数
	}
	return n;							//返回总原子个数
}

int main()
{
	GLNode *g;
	char *str="((a),b,(c,d))";
	g=CreateGL(str);
	DispGL(g);
	printf("\n");
	printf("解法1原子个数：%d\n",Count1(g));
	printf("解法2原子个数：%d\n",Count2(g));
	DestroyGL(g);
	return 1;
}
```