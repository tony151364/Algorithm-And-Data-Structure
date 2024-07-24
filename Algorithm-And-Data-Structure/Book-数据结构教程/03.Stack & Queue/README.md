# 第3章 栈和队列
## 3.1 栈
### 3.1.1 栈的定义
- 栈是只能在一端进行插入和删除的线性表
- 栈也称为先进后出（First In Last Out）表
- 题目：出栈次序可可能性

### 3.1.2 栈的顺序存储及其基本运算非实现
- 采用顺序存储结构的栈称为**顺序栈**(sequentual stack)

```C++
typedef struct 
{	
	ElemType data[MaxSize];
	int top;				//栈指针
} SqStack;	
```
```C++
// 1.初始化栈
void InitStack(SqStack *&s)
{
	s=(SqStack *)malloc(sizeof(SqStack));
	s->top=-1;
} 
```
```C++
// 2.销毁栈
void DestroyStack(SqStack *&s)
{
	free(s);
}
```
```C++
 //3.判断栈是否为空
bool StackEmpty(SqStack *s)
{
	return(s->top==-1);
}
```
```C++
// 4.进栈
bool Push(SqStack *&s,ElemType e)
{
	if (s->top==MaxSize-1)    //栈满的情况，即栈上溢出
		return false;
	s->top++;
	s->data[s->top]=e;
	return true;
}
```
```C++
 // 5.出栈
bool Pop(SqStack *&s,ElemType &e)
{
	if (s->top==-1)		//栈为空的情况，即栈下溢出
		return false;
	e=s->data[s->top];
	s->top--;
	return true;
} 
```
```C++
// 6.取栈顶元素
bool GetTop(SqStack *s,ElemType &e)
{
	if (s->top==-1) 		//栈为空的情况，即栈下溢出
		return false;
	e=s->data[s->top];
	return true;
}
```
- 【例 3.4】设计一个算法利用顺序栈判断一个字符串是否为对称串
```C++
bool symmetry(ElemType str[])
{
	int i;  ElemType e;
	SqStack *st;
	InitStack(st);				//初始化栈
	for (i=0;str[i]!='\0';i++)	//将串所有元素进栈
		Push(st,str[i]);		//元素进栈
	for (i=0;str[i]!='\0';i++)
	{
		Pop(st,e);				//退栈元素e
		if (str[i]!=e)			//若e与当前串元素不同则不是对称串
		{
			DestroyStack(st);	//销毁栈
			return false;
		}
	}
	DestroyStack(st);			//销毁栈
	return true;
}
// 关键：对称串的进栈顺序和出栈顺序相同
```
#### 共享栈
- 当需要用到两个**相同类型**栈的时候，可能出现一个栈满，再进栈就溢出了，而另一个栈有很多空闲空间。这时候可以使用**共享栈**来解决资源分配问题
```C++
typedef struct
{
	ElemType data[MaxSize];
	int top1, top2;
}DStack;
```
- 共享栈四要素：
	- 栈空条件：栈1空为 ``` top1 == -1; ``` 栈2空为 ``` top2 == MaxSize; ```
	- 栈满条件：``` top1 == top2 - 1; ```
	- 进栈：栈1为 ``` top1++; data[top1] = x; ``` 栈2为 ``` top2--; data[top2] = x; ```
	- 出栈：栈1为 ``` x = data[top1]; top1--; ``` 栈2为 ``` x = data[top2]; top++; ```

### 3.1.3 栈的链式存储结构及其基本运算
- 采用链式存储结构的栈称为**链栈**(Linked Stack)

```C++
typedef struct linknode
{	
	ElemType data;				//数据域
	struct linknode *next;		//指针域
} LinkStNode;					//链栈类型
```

- 链栈4要素：
	- 栈空：``` s->next == NULL ```
	- 栈满：内存溢出才会栈满，所以通常不存在栈满
	- 进栈：新建一个结点设置data值，插入到头结点之后。
	- 出栈：取出首结点的data值并删除

```C++
// 1.初始化栈
void InitStack(LinkStNode *&s)
{
	s=(LinkStNode *)malloc(sizeof(LinkStNode));
	s->next=NULL;
}
```

```C++
// 2.销毁栈
void DestroyStack(LinkStNode *&s)
{
	LinkStNode *pre=s,*p=s->next;
	while (p!=NULL)
	{	
		free(pre);
		pre=p;
		p=pre->next;
	}
	free(pre);	//s指向尾结点,释放其空间
}
```
```C++
// 3.判断栈是否为空
bool StackEmpty(LinkStNode *s)
{
	return(s->next==NULL);
}
```
```C++
// 4.进栈
void Push(LinkStNode *&s,ElemType e)
{	LinkStNode *p;
	p=(LinkStNode *)malloc(sizeof(LinkStNode));
	p->data=e;				//新建元素e对应的结点p
	p->next=s->next;		//插入p结点作为开始结点
	s->next=p;
}
```
```C++
// 5.出栈
bool Pop(LinkStNode *&s,ElemType &e)
{	LinkStNode *p;
	if (s->next==NULL)		//栈空的情况
		return false;
	p=s->next;				//p指向开始结点
	e=p->data;
	s->next=p->next;		//删除p结点
	free(p);				//释放p结点
	return true;
}
```
```C++
// 6.取栈顶元素
bool GetTop(LinkStNode *s,ElemType &e)
{	if (s->next==NULL)		//栈空的情况
		return false;
	e=s->next->data;
	return true;
}
```

- 【例 3.5】设计一个算法判断输入的表达式中括号是否配对（假设只含有左、右圆括号）
- 算法设计思路：一个表达式中的左右括号是按最近位置配对的，所以利用一个栈来进行求解。这里采用链栈
```C++
// 理解错了，我以为是表达式，其实只是括号配对"((()))"
bool Match(char exp[], int n)
{
	int i=0; char e;
	bool match = true;
	LinkStNode* st;
	InitStack(st);
	
	while (i < n && match)
	{
		if (exp[i]=='(')
			Push(st, exp[i]);
		else if(exp[i]==')')
		{
			if (GetTop(st, e)==true)
			{
				if (e!='(')  // 栈内的元素肯定是左括号，这里是为了便于扩展多种字符
					match=false;
				else
					Pop(st, e);
			}
			else
				match=false;
		}
		i++;
	}

	if (!StackEmpty(st))
		match=false;

	DestroyStack(st);
	return match;
}
```

### 3.1.4 栈的应用

#### 1.简单表达式求值
- 简单表达式采用字符数组，只含有+、-、*、/、正整数和圆括号，使用顺序栈。
- 中缀表达式：“先乘除，后加减，计算从左到右，先括号内，再括号外”。因此不仅依赖运算符优先级，而且还要处理括号。
- 后缀表达式（逆波兰表达式）：运算符在操作数后面。“1+2*3” -> “1 2 3 * +”。
	- 特点1：没有括号，已考虑了运算符的优先级
	- 特点2：只有操作数和运算符，而且越放在前面的运算符越优先执行
- 前缀表达式：运算符在操作符前面。“1+2*3” -> “+ 1 * 2 3”。

- (1) 将算术表达式转换成后缀表达式
```C++
void trans(char *exp,char postexp[])	//将算术表达式exp转换成后缀表达式postexp
{
	char e;
	SqStack *Optr;						//定义运算符栈
	InitStack(Optr);					//初始化运算符栈
	int i=0;							//i作为postexp的下标
	while (*exp!='\0')					//exp表达式未扫描完时循环
	{	switch(*exp)
		{
		case '(':						//判定为左括号
			Push(Optr,'(');				//左括号进栈
			exp++;						//继续扫描其他字符
			break;
		case ')':						//判定为右括号
			Pop(Optr,e);				//出栈元素e
			while (e!='(')				//不为'('时循环
			{
				postexp[i++]=e;			//将e存放到postexp中
				Pop(Optr,e);			//继续出栈元素e
			}
			exp++;						//继续扫描其他字符
			break;
		case '+':						//判定为加或减号
		case '-':
			while (!StackEmpty(Optr))	//栈不空循环
			{
				GetTop(Optr,e);			//取栈顶元素e
				if (e!='(')				//e不是'('
				{
					postexp[i++]=e;		//将e存放到postexp中
					Pop(Optr,e);		//出栈元素e
				}
				else					//e是'(时退出循环
					break;
			}
			Push(Optr,*exp);			//将'+'或'-'进栈
			exp++;						//继续扫描其他字符
			break;
		case '*':						//判定为'*'或'/'号
		case '/':
			while (!StackEmpty(Optr))	//栈不空循环
			{
				GetTop(Optr,e);			//取栈顶元素e
				if (e=='*' || e=='/')	//将栈顶'*'或'/'运算符出栈并存放到postexp中
				{
					postexp[i++]=e;		//将e存放到postexp中
					Pop(Optr,e);		//出栈元素e
				}
				else					//e为非'*'或'/'运算符时退出循环
					break;
			}
			Push(Optr,*exp);			//将'*'或'/'进栈
			exp++;						//继续扫描其他字符
			break;
		default:				//处理数字字符
			while (*exp>='0' && *exp<='9') //判定为数字
			{	postexp[i++]=*exp;
				exp++;
			}
			postexp[i++]='#';	//用#标识一个数值串结束
		}
	}
	while (!StackEmpty(Optr))	//此时exp扫描完毕,栈不空时循环
	{
		Pop(Optr,e);			//出栈元素e
		postexp[i++]=e;			//将e存放到postexp中
	}
	postexp[i]='\0';			//给postexp表达式添加结束标识
	DestroyStack(Optr);			//销毁栈		
}
```
- (2) 后缀表达式求值
	- 数字字符：转换数值并进栈
	- 运算符：退栈两个操作数，计算后将结果进栈
```C++
double compvalue(char *postexp)	//计算后缀表达式的值
{
	double d,a,b,c,e;
	SqStack1 *Opnd;				//定义操作数栈
	InitStack1(Opnd);			//初始化操作数栈
	while (*postexp!='\0')		//postexp字符串未扫描完时循环
	{	
		switch (*postexp)
		{
		case '+':				//判定为'+'号
			Pop1(Opnd,a);		//出栈元素a
			Pop1(Opnd,b);		//出栈元素b
			c=b+a;				//计算c
			Push1(Opnd,c);		//将计算结果c进栈
			break;
		case '-':				//判定为'-'号
			Pop1(Opnd,a);		//出栈元素a
			Pop1(Opnd,b);		//出栈元素b
			c=b-a;				//计算c
			Push1(Opnd,c);		//将计算结果c进栈
			break;
		case '*':				//判定为'*'号
			Pop1(Opnd,a);		//出栈元素a
			Pop1(Opnd,b);		//出栈元素b
			c=b*a;				//计算c
			Push1(Opnd,c);		//将计算结果c进栈
			break;
		case '/':				//判定为'/'号
			Pop1(Opnd,a);		//出栈元素a
			Pop1(Opnd,b);		//出栈元素b
			if (a!=0)
			{
				c=b/a;			//计算c
				Push1(Opnd,c);	//将计算结果c进栈
				break;
			}
			else
		    {	
				printf("\n\t除零错误!\n");
				exit(0);		//异常退出
			}
			break;
		default:				//处理数字字符
			d=0;				//将连续的数字字符转换成对应的数值存放到d中
			while (*postexp>='0' && *postexp<='9')   //判定为数字字符
			{	
				d=10*d+*postexp-'0';  
				postexp++;
			}
			Push1(Opnd,d);		//将数值d进栈

			break;
		}
		postexp++;				//继续处理其他字符
	}
	GetTop1(Opnd,e);			//取栈顶元素e
	DestroyStack1(Opnd);		//销毁栈		
	return e;					//返回e
}
```
- 思想是这么个思想，实现可以有多种方式

#### 2.求解迷宫问题
- 所求路径必须是简单路径，即路径不重复
```C++
#include <stdio.h>
#include <malloc.h>
#define MaxSize 100
#define M 8
#define N 8
int mg[M+2][N+2]=
{	
	{1,1,1,1,1,1,1,1,1,1},
	{1,0,0,1,0,0,0,1,0,1},
	{1,0,0,1,0,0,0,1,0,1},
	{1,0,0,0,0,1,1,0,0,1},
	{1,0,1,1,1,0,0,0,0,1},
	{1,0,0,0,1,0,0,0,0,1},
	{1,0,1,0,0,0,1,0,0,1},
	{1,0,1,1,1,0,1,1,0,1},
	{1,1,0,0,0,0,0,0,0,1},
	{1,1,1,1,1,1,1,1,1,1}
};

typedef struct
{
	int i;				//当前方块的行号
	int j;				//当前方块的列号
	int di;				//di是下一可走相邻方位的方位号
} Box;
typedef struct
{
	Box data[MaxSize];	//存放方块
    int top;			//栈顶指针
} StType;				//定义栈类型
```
```C++
bool mgpath(int xi,int yi,int xe,int ye)	//求解路径为:(xi,yi)->(xe,ye)
{
	Box path[MaxSize], e;
	int i,j,di,i1,j1,k;
	bool find;
	StType *st;								//定义栈st
	InitStack(st);							//初始化栈顶指针
	e.i=xi; e.j=yi;	e.di=-1;				//设置e为入口
	Push(st,e);								//方块e进栈
	mg[xi][yi]=-1;							//入口的迷宫值置为-1避免重复走到该方块
	while (!StackEmpty(st))					//栈不空时循环
	{
		GetTop(st,e);						//取栈顶方块e
		i=e.i; j=e.j; di=e.di;
		if (i==xe && j==ye)					//找到了出口,输出该路径
		{ 
			printf("一条迷宫路径如下:\n");
			k=0;
			while (!StackEmpty(st))
			{
				Pop(st,e);					//出栈方块e
				path[k++]=e;				//将e添加到path数组中
			}
			while (k>=1)
			{
				k--;
				printf("\t(%d,%d)",path[k].i,path[k].j);
				if ((k+2)%5==0)				//每输出每5个方块后换一行
					printf("\n");  
			}
			printf("\n");
			DestroyStack(st);				//销毁栈
			return true;					//输出一条迷宫路径后返回true
		}
		find=false;
		while (di<4 && !find)				//找相邻可走方块(i1,j1)
		{	
			di++;
			switch(di)
			{
			case 0:i1=i-1; j1=j;   break;
			case 1:i1=i;   j1=j+1; break;
			case 2:i1=i+1; j1=j;   break;
			case 3:i1=i;   j1=j-1; break;
			}
			if (mg[i1][j1]==0) find=true;	//找到一个相邻可走方块，设置find我真
		}
		if (find)							//找到了一个相邻可走方块(i1,j1)
		{	

			st->data[st->top].di=di;		//修改原栈顶元素的di值
			e.i=i1; e.j=j1; e.di=-1;
			Push(st,e);						//相邻可走方块e进栈
			mg[i1][j1]=-1;					//(i1,j1)的迷宫值置为-1避免重复走到该方块
		}
		else								//没有路径可走,则退栈
		{	
			Pop(st,e);						//将栈顶方块退栈
			mg[e.i][e.j]=0;					//让退栈方块的位置变为其他路径可走方块
		}
	}
	DestroyStack(st);						//销毁栈
	return false;							//表示没有可走路径,返回false
}
```
```C++
int main()
{
	if (!mgpath(1, 1, N, N))
	{
		printf("该迷宫问题没有解！");
	}
	return 1;
}
```

## 3.2 队列

### 3.2.1 栈的定义
- 队列(queue)简称队，它也是一种操作受限的线性表，其限制为仅允许在表的一端进行插入操作，而在表的另一端进行删除操作 。
- 插入的一端叫做**队尾**，删除的一端叫做**队头**或**队首**。
- 插入新元素称为**进队**或**入队**(enqueue)，删除元素称为**出队**或**离队**(dequeue)。元素出队以后，其直接后继元素就成为队首元素
- 队列又被成为**先进先出表**(First In First Out)

### 3.2.2 队列顺序存储结构及其基本运算的实现
- 采用顺序存储结构的队列简称为**顺序队**(sequential queue)
- 逻辑结构 -> 存储结构

```C++
// 顺序队类型SqQueue
typedef struct
{
	ElemType data[MaxSize];
	int front,rear;  // 队首和队尾指针
 }SqQueue;
 ```

 #### 1.顺序队中实现队列的基本运算
 - 四个重要操作：
 	- 队空条件：```q->front == q->rear```
 	- 队满条件：```q->rear == MaxSize-1```
 	- 元素e的进队操作：先将rear增1，然后将元素e放在data数组的rear位置
 	- 出队操作：先将front增1，然后取出data数组中front位置的元素
 
 ```C++
 // 1.初始化队列
void InitQueue(SqQueue *&q)
{
	q=(SqQueue *)malloc(sizeof(SqQueue));
	q->front=q->rear=-1;
}

// 2.销毁队列
void DestroyQueue(SqQueue *&q)
{
	free(q);
}

//3.判断对列是否为空
bool QueueEmpty(SqQueue *q)
{
	return(q->front == q->rear);  // 若为空返回true 
}

//4.进队
bool enQueue(SqQueue *q, ElemType &e)
{
	if(q->rear == MaxSize-1)
		return false;
		
	q->rear++;
	q->data[q->rear]=e;
	return true;
}

//5.出队
bool deQueue(SqQueue *q, ElemType &e)
{
	if(q->front == q->rear)
		return false;
		
	q->front++;
	e=q->data[q->front];
	return true;
 } 
 // 以上5个算法的时间复杂度均为 O(1) 
```

#### 2.环形队列中实现队列的基本运算
- ```q->rear == MaxSize-1```成立时也有可能是假溢出，解决的办法就是头尾相连形成循环队列。
- 循环队列进出队：
	- 队空：```q->rear==q->front```
	- 队满：```(q->rear + 1) % MaxSize == q->front```
	- 队头指针front增1：```front = (front + 1) % MaxSize```
	- 队尾指针rear增1：```rear = (rear + 1) % MaxSize```

- 因为队满和队空都是```q->rear==q->front```，所以让整个队列最多只有MaxSize-1个元素

```C++
// 1.初始化队列
void InitQueue(SqQueue *&q)
{
	q=(SqQueue *)malloc(sizeof(SqQueue));
	q->front=q->rear=0;
} 

// 2.销毁队列
void DestroyQueue(SqQueue *&q)
{
	free(q);
}

//3.判断对列是否为空
bool QueueEmpty(SqQueue *q)
{
	return (q->front == q->rear);
} 

//4.进队
bool enQueue(SqQueue *q, ElemType &e)
{
	// 队满上溢出 
	if((q->rear+1)%MaxSize ==q->front )
		return false;
		
	q->rear=(q->rear+1)%MaxSize; 
	q->data[q->rear]=e;
	return true;
}

//5.出队
bool deQueue(SqQueue *q, ElemType &e)
{
	if(q->front == q->rear)
		return false;
		
	q->front=(q->front+1)%MaxSize;
	e=q->data[q->front];
	return true;
} 
```
- 已知front、rear，求count： ```count = (rear - front + MaxSize) % MaxSize```
- 已知front、count，求rear： ```rear = (front + count) % MaxSize```
- 已知rear、count，求front： ```front = (rear - count + MaxSize) % MaxSize```

【例 3.7】对于环形队列来说，如果知道队头指针和元素个数，就可以知道队尾指针的位置。也就是说，可以用队列中的元素个数代替队尾指针。请设计出这种队列的初始化、进队、出队和判队空的算法。
```C++
typedef struct
{
	ElemType data[MaxSize];
	int front;
	int count;
} QuType;
```
 
- 队尾指针计算公式：```rear=(front+count)%MaxSize```

该环形队列的4要素：
- 队空条件：```count == 0```
- 队满条件：```count == MaxSize```
- 进队e操作：```rear=(rear+1)%MaxSize; 将e放在rear处```
- 出队操作：```front=（front+1%MaxSize; 去除front处元素e```  

注意：这样的环形队列的最大元素个数为MaxSize。也就是说，可以全部放满了。

```C++
void InitQueue(QuType*&qu)
{
	qu=(QuType*)malloc(sizeof(QuType));
	qu->front=0;
	qu->count=0;
}

void DestroyQueue(QuType* &qu)
{
	free(qu);
}

bool EnQueue(QuType*&qu, ElemType x)
{
	if (qu->count==MaxSize)
		return false;
	else
	{
		int rear;
		rear=(qu->front+qu->count)%MaxSize;
		rear=(rear+1)%MaxSize;					//队尾循环增1
		qu->data[rear]=x;
		qu->count++;							//元素个数增1
		return true;
	}
}

bool DeQueue(QuType *&qu,ElemType &x)			//出队运算算法
{	if (qu->count==0)							//队空下溢出
		return false;
	else
	{	qu->front=(qu->front+1)%MaxSize;		//队头循环增1
		x=qu->data[qu->front];
		qu->count--;							//元素个数减1
		return true;
	}
}

bool QueueEmpty(QuType *qu)						//判队空运算算法
{
	return(qu->count==0);
}
```
- 队列的形式可能多种多样，并不是只有教材中这唯一的解
- 注意：这个例子中，元素是从数组的第二个位置开始存储的，也就是index==1的位置。因为第一次进队的队尾==1

### 3.2.3 队列的链式存储结构及其基本运算的实现
- 链队(linked queue)
- 这里使用不带头结点的单链表来实现，因为头结点被当做队头队尾指针
```C++
typedef char ElemType;
typedef struct DataNode
{	
	ElemType data;
	struct DataNode *next;
} DataNode;				//单链表节点

typedef struct
{	
	DataNode *front;
	DataNode *rear;
} LinkQuNode;			//链队节点
```
- 链队四要素：
	- 队空条件：```front=rear=NULL```
	- 队满条件：不考虑
	- 进队e操作：将包含e的节点插入到单链表表尾
	- 出队操作：删除单链表首数据结点

```C++
// 初始化队列
void InitQueue(LinkQuNode *&q)
{	
	q=(LinkQuNode *)malloc(sizeof(LinkQuNode));
	q->front=q->rear=NULL;
}

// 销毁队列
void DestroyQueue(LinkQuNode *&q)
{
	DataNode *p=q->front,*r;//p指向队头数据结点
	if (p!=NULL)			//释放数据结点占用空间
	{	r=p->next;
		while (r!=NULL)
		{	free(p);
			p=r;r=p->next;
		}
	}
	free(p);				//释放最后一个节点an
	free(q);				//释放链队结点占用空间
}

// 判断队列为空
bool QueueEmpty(LinkQuNode *q)
{
	return(q->rear==NULL);
}

// 进队（需要考虑原队列为空或非空两种情况）
void enQueue(LinkQuNode *&q,ElemType e)
{	
	DataNode *p;
	p=(DataNode *)malloc(sizeof(DataNode));
	p->data=e;
	p->next=NULL;

	if (q->rear==NULL)		//若链队为空,则新结点是队首结点又是队尾结点
		q->front=q->rear=p;
	else
	{	q->rear->next=p;	//将p结点链到队尾,并将rear指向它
		q->rear=p;
	}
}

// 出队
bool deQueue(LinkQuNode *&q,ElemType &e)
{	DataNode *t;
	if (q->rear==NULL)		//队列为空（情况1）
		return false;
	t=q->front;				//t指向第一个数据结点
	if (q->front==q->rear)  //队列中只有一个结点时（情况2）
		q->front=q->rear=NULL;
	else					//队列中有多个结点时（情况3）
		q->front=q->front->next;
	e=t->data;
	free(t);
	return true;
}
```