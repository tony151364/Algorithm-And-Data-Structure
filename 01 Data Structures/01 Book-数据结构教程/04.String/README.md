# 第4章 串
- 串可以看做一种特殊的线性表，线性表的很多操作串也适用。例如C++的STL中的Vector。

## 4.1 串的基本概念
- **串**（string）是由零个或多个字符组成的有序数列

- 含零个字符的串称为**空串**，用空集的符号表示。

- **两个串相等**当且仅当这两个串的长度相等并且各对应位置上的字符都相同

- **字串**：一个串中任意个字符组成的序列。字串含空串和自己本身

- 串的抽象数据类型如下：
```C++
// 基本运算：
StrAssign(&s,cstr) //将字符串常量cstr赋给串s，即生成其值等于cstr的串s。
DestroyStr(&s) // 销毁串，释放为串s分配的存储空间
StrCopy(&s,t) //串复制，将串t赋值给串s
StrEqual(s,t)  //判断串是否相等，若两个串相等返回真；否则返回假
StrLength(s)  //求串长，返回串s中字符的个数。
Concat(s,t)  //串连接，返回由两个串s和t连接在一起形成的新串。
SubStr(s,i,j)  //求子串，返回串s中从第i（1<=i<=n）个字符开始的由连续j个字符组成的子串
InsStr(s1,i,s2)  // 子串插入，将串s2插入到串s1的第i（1<=i<=n+1)个位置
DelStr(s,i,j)  // 子串删除，从串s中删去从第i（i<=i<=n）个字符开始的长度为j的子串，并返回产生的新串。
RepStr(s,i,j,t)  // 子串替换，在串s中将第i （i<=i<=n）个字符开始的j个字符构成的子串用串t替换，并返回产生的新串
DispStr(s)  // 串输出，输出串s的所有字符值 
```

## 4.2 串的存储结构

### 4.2.1 串的顺序存储结构——顺序串

- 顺序串的存储方式有两种：
	- 非紧缩格式：每个字只存一个字符。这种方式比较浪费内存空间 ，但处理单个字符或一组字符比较方便。 
	- 紧缩格式：每个字存放多个字符。这种方式节省内存空间，但处理单个字符不太方便，运算效率低，需要花费时间从一个字中分离字符 

- 现代计算机内存都比较大了，为了算法方便，一般都用非紧缩格式。

- 非紧缩格式的类型声明如下：
```C++
typedef struct
{
	char data[MaxSize];
	int length;
}SqString;
```
- 顺序串与线性表：
  - 相同点：二者基本算法类似
  - 不同点：顺序串采用直接传递顺序串的方法，第二章的顺序表算法采用的是顺序表指针。（老师：目的在于让同学熟悉两种不同的设计方式）

```C++
//1.生成串
void StrAssign(SqString &s, char cstr[])  //cstr[] 相当于 *cstr,s为引用型参数 
{
	int i;
	for(i=0;cstr[i]!='\0';i++)
		s.data[i]=cstr[i];
	s.length=i;   // 设置串的长度 
} 
```

- 这一章的顺序串是直接采用顺序串本身来表示的，而不是顺序串指针，它的存储空间由操作系统管理，即由操作系统分配其存储空间，并在超过作用域时释放其存储空间，所以这里的销毁串不包含任何操作
```C++
// 2.销毁串
void DestroyStr(SqString &s)
{
}
```

```C++
// 3.串的复制
void StrCopy(SqString&s,SqString t)
{
	int i;
	for(i=0;i<t.lenght;i++)
		s.data[i]=t.data[i];
	s.lenght=t.length;
} 
```

```C++
// 4.判断串相等
bool StrEqual(SqString s, SqString t)
{
	bool same=true;int i;
	if(s.lenght!=t.lenght)
		return false;
	else
		for(i=0;i<t.lenght;i++)
			{
				if(s.data[i]!=t.data[i])
					seme=false;
					break;
			}
	return same;
} 
```

```C++
// 5.求串长StrLength(s)
int String(SqString s)
{
	return s.length;
} 
```

```C++
// 6.串的连接
SqString Concat(SqString s,SqString t)
{
	SqString str;  // 定义结果串
	int i;
	str.length=s.length+t.length;
	for(i=0;i<s.length;i++)  // 将s复制到str 
		str.data[i]=s.data[i];
	for(i=0;i<t.length;i++)  // 将t复制到str 
		str.data[s.length+i]=t.data[i];
	return str; 
} 
```

```C++
// 7.求子串
/*for(m=0, n=1;m<j;m++, n++)
	str.data[m] = s.data[n]*/ 
SqString SubStr(SqString s,int i,int j)
{
	int k; 
	SqString str;
	str.lenght=0;
	if(i<=0 ||i>s.length ||j<0 || j>s.length)
		return str;
	for(k=i-1;k<i+j-1;k++)
		str.data[k-i+1]=s.data[k];
	str.length=j;
	return str;
} 
```

```C++
// 8.子串的插入InsStr(s1,i,s2)
SqString InsStr(SqString s1,int i,SqString s2)
{
	int j;
	SqString str;
	str.length=0
	if(i<0 || i>s.length+1)
		return str;
	for(j=0;j<i-1;i++)
		str.data[i+j-1] = s1.data[j];
	for(j=0;j<s2.length;j++)
		str.data[str.length+j]=s2.data[j];
	for(j=i-1;s1.length;j++)
		str.data[s2.length+j]=s1.data[j];
	str.length = s1.length + s2.length;
	return str;
} 
```

```C++
// 10.输出串 
void Disp(SqString s)
{
	int i;
	if(s.length>0)
	{
		for(i=0;i<s.length;i++)
			printf("%c ",s.data[i]);
		printf("\n");
	}
	
} 
```

```C++
// 11.删除串
SqString DelStr(SqString s1,int i,int j)
{
	SqString str;
	int n;
	str.length=0;
	if(i<0 ||i>s1.length || i+j>s1.length+1)
		return str;
		
	for(n=0;n<i-1;n++)
		str.data[n]=s1.data[n];
		
	for(n=i+j-1;n<s1.length;n++)
		str.data[n-j]=s1.data[n];
		
	str.length=s1.length-j;
	return str;
} 
```

```C++
// 12.子串的替换
SqString RepStr(SqString s,int i,int j,SqString t)
{
	int k;
	SqString str;
	str.length=0;
	if(i<=0 ||i>s.length || i+j-1>s.length)
		return str;    // 参数不对返回空串
	for(k=0;k<i-1;k++)
		str.data[k]=s.data[k];
	
	for(k=0;k<t.length;k++)
		str.data[k+i-1] = t.data[k];
		
	for(k=i-1;k<s.length;k++)
		str.data[k+t.length]=s.data[k+j];
	
	str.length=s.length-j+t.length;
	return str;
 } 
```

- 【例 4.1】见`Alogorithm.md`
- 【例 4.2】见`Alogorithm.md`

### 4.2.2 串的链式存储结构——链串
- 链串中通常**一个节点可以存储多个字符**。通常将链串种每个节点所存储的字符个数称为**节点大小**
- 节点越大，算法越复杂。比如4个节点的链串，插入和删除都很麻烦，要移动后面所有的节点。而节点大小为1的节点是最简单的。插入和删除不需要移动后面的节点。所以都以这里节点为1进行讨论。

```C++
typedef struct snode 
{	
	char data;
	struct snode *next;
} LinkStrNode;
```
- 链串实现的基本运算与单链表类似

```C++
void StrAssign(LinkStrNode *&s,char cstr[])	//字符串常量cstr赋给串s
{
	int i;
	LinkStrNode *r,*p;
	s=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	r=s;						//r始终指向尾结点
	for (i=0;cstr[i]!='\0';i++) 
	{	p=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		p->data=cstr[i];
		r->next=p;r=p;
	}
	r->next=NULL;
}
```

```C++
void DestroyStr(LinkStrNode *&s)
{	LinkStrNode *pre=s,*p=s->next;	//pre指向结点p的前驱结点
	while (p!=NULL)					//扫描链串s
	{	free(pre);					//释放pre结点
		pre=p;						//pre、p同步后移一个结点
		p=pre->next;
	}
	free(pre);						//循环结束时,p为NULL,pre指向尾结点,释放它
}
```

```C++
void StrCopy(LinkStrNode *&s,LinkStrNode *t)	//串t复制给串s
{
	LinkStrNode *p=t->next,*q,*r;
	s=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	r=s;						//r始终指向尾结点
	while (p!=NULL)				//将t的所有结点复制到s
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
}
```

```C++
bool StrEqual(LinkStrNode *s,LinkStrNode *t)	//判串相等
{
	LinkStrNode *p=s->next,*q=t->next;
	while (p!=NULL && q!=NULL && p->data==q->data) 
	{	p=p->next;
		q=q->next;
	}
	if (p==NULL && q==NULL)
		return true;
	else
		return false;
}
```

```C++
int StrLength(LinkStrNode *s)	//求串长
{
	int i=0;
	LinkStrNode *p=s->next;
	while (p!=NULL) 
	{	i++;
		p=p->next;
	}
	return i;
}
```

```C++
LinkStrNode* Concat(LinkStrNode *s,LinkStrNode *t)	//串连接
{
	LinkStrNode *str,*p=s->next,*q,*r;
	str=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	r=str;
	while (p!=NULL)				//将s的所有结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	p=t->next;
	while (p!=NULL)				//将t的所有结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
	return str;
}
```

```C++
LinkStrNode* SubStr(LinkStrNode *s,int i,int j)	//求子串
{
	int k;
	LinkStrNode *str,*p=s->next,*q,*r;
	str=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	str->next=NULL;
	r=str;						//r指向新建链表的尾结点
	if (i<=0 || i>StrLength(s) || j<0 || i+j-1>StrLength(s))
		return str;				//参数不正确时返回空串
	for (k=0;k<i-1;k++)
		p=p->next;
	for (k=1;k<=j;k++) 			//将s的第i个结点开始的j个结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
	return str;
}
```

```C++
// 插入子串
LinkStrNode* InsSubStr(LinkStrNode *s,int i,LinkStrNode *t)
{
	int k;
	LinkStrNode *str,*p=s->next,*p1=t->next,*q,*r;
	str=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	str->next=NULL;
	r=str;								//r指向新建链表的尾结点
	if (i<=0 || i>StrLength(s)+1)		//参数不正确时返回空串
		return str;
	for (k=1;k<i;k++)					//将s的前i个结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	while (p1!=NULL)					//将t的所有结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p1->data;
		r->next=q;r=q;
		p1=p1->next;
	}
	while (p!=NULL)						//将结点p及其后的结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
	return str;
}
```

```C++
//删除子串
LinkStrNode* DelSubStr(LinkStrNode* s,int i,int j)
{
	int k;
	LinkStrNode *str,*p=s->next,*q,*r;
	str=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	str->next=NULL;
	r=str;						//r指向新建链表的尾结点
	if (i<=0 || i>StrLength(s) || j<0 || i+j-1>StrLength(s))
		return str;				//参数不正确时返回空串
	for (k=0;k<i-1;k++)			//将s的前i-1个结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	for (k=0;k<j;k++)				//让p沿next跳j个结点
		p=p->next;
	while (p!=NULL)					//将结点p及其后的结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
	return str;
}
```

```C++
// 替换子串
LinkStrNode *RepSubStr(LinkStrNode *s,int i,int j,LinkStrNode *t)
{
	int k;
	LinkStrNode *str,*p=s->next,*p1=t->next,*q,*r;
	str=(LinkStrNode *)malloc(sizeof(LinkStrNode));
	str->next=NULL;
	r=str;							//r指向新建链表的尾结点
	if (i<=0 || i>StrLength(s) || j<0 || i+j-1>StrLength(s))
		return str;		 			//参数不正确时返回空串
	for (k=0;k<i-1;k++)  			//将s的前i-1个结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;q->next=NULL;
		r->next=q;r=q;
		p=p->next;
	}
	for (k=0;k<j;k++)				//让p沿next跳j个结点
		p=p->next;
	while (p1!=NULL)				//将t的所有结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p1->data;q->next=NULL;
		r->next=q;r=q;
		p1=p1->next;
	}
	while (p!=NULL)					//将结点p及其后的结点复制到str
	{	q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
		q->data=p->data;q->next=NULL;
		r->next=q;r=q;
		p=p->next;
	}
	r->next=NULL;
	return str;
}
```

```C++
void DispStr(LinkStrNode *s)	//输出串
{
	LinkStrNode *p=s->next;
	while (p!=NULL)
	{	printf("%c",p->data);
		p=p->next;
	}
	printf("\n");
}

```

- 【例 4.3】见`Alogorithm.md`

## 4.3 串的模式匹配
- 设有两个串s和t，串t的定位就是要在串s中找到一个与t相等的子串。通常把s称为**目标串**(target string),把t称为**模式串**(pattern string),因此串定位查找也称为**模式匹配**(pattern matching)。

- 我理解串的模式匹配其实就是子串查找。（ChatGTP说我说的非常对^_^）
  
### 4.3.1 Brute-Force算法
- Brute-Force（暴力）简称为BF算法，也称简单匹配算法，采用穷举方法，其基本思想是......。

- 基础算法。许多字符串模式匹配算法都是以它为基础的。 逻辑比较清晰，使用3个变量来实现。
```C++
int index(SqString s, SqString t)
{
	int i, j, k;

	for (i = 0; i < s.length - t.length; i++)
	{
		for (k = i, j = 0; k < s.length && j < t.length && s.data[k] == t.data[j]; k++, j++);

		if (j == t.length)
		{
			return i;
		}
	}
	return -1;
}
```
- 真正的BF算法，为KMP做准备。
```C++
int BF(SqString s,SqString t)
{
	int i=0,j=0;

	while (i<s.length && j<t.length) 
	{
		if (s.data[i]==t.data[j])  	//继续匹配下一个字符
		{	
			i++;				//主串和子串依次匹配下一个字符
			j++;  
		}
		else          			//主串、子串指针回溯重新开始下一次匹配
		{	
			i=i-j+1;			//主串从下一个位置开始匹配
			j=0; 				//子串从头开始匹配
		}
	}

	if (j>=t.length)   
		return(i-t.length);  		//返回匹配的第一个字符的下标
	else  
		return(-1);        		//模式匹配不成功
}
```
- 最好的时间复杂度：`O(m)`
- 最坏的时间复杂度：`O(n x m)`
- 平均的时间复杂度：`O(n x m)`
- 也就是说这个算法是不高效的，因为算法的平均性能接近最坏的情况。

- 【视频例题1】见`Algorithm.md`

### 4.3.2 KMP算法
- KMP（Knuth-Morris-Pratt，来源于3个作者名字的首字母）算法与BF算法相比有较大的改进，主要**消除了主串指针的回溯**，从而使算法效率有了某种效率的提高。

- 算法思路：空间换时间，从模式串t中提取有效信息并进行保存（**寻找最长相等前后缀**）

匹配过程：
- 1.前缀表构建。
- 2.模式匹配

补充：
- 前缀：包含首字符，不包含尾字母的所有子串
- 后缀：包含尾字母，不包含首字母的所有子串
```C++
// 模式串t的next数组算法
void GetNext(SqString t,int next[])		//由模式串t求出next值
{
	int j,k;
	j=0; k=-1; next[0]=-1;  // next[0]=-1有特殊含义

	while (j < t.length-1) 
	{	// 从t[1]开始考虑，而不是t[0]。因为t[0]没有前后缀
		if (k==-1 || t.data[j]==t.data[k]) 	//k为-1或比较的字符相等时
		{	
			j++;k++;
			next[j]=k;
       	}
       	else
		{
			k=next[k];
		}
	}
}

int KMPIndex(SqString s,SqString t)  //KMP算法
{
	int next[MaxSize],i=0,j=0;
	GetNext(t,next);
	while (i<s.length && j<t.length) 
	{
		if (j==-1 || s.data[i]==t.data[j]) 
		{
			i++;j++;  			//i,j各增1
		}
		else j=next[j]; 		//i不变,j后退
    }
    if (j>=t.length)
		return(i-t.length);  	//返回匹配模式串的首字符下标
    else  
		return(-1);        		//返回不匹配标志
}
```
KMP算法的正确性：
- 用数学的方式证明了KMP跳过的那些趟是没有必要的，所以KMP是一定正确的。（书上也有证明过程）
- 感叹：数学的魅力啊，直接进行维度上的跃迁，从而大大改进了效率。这三个人也是够厉害的。

KMP算法分析
- 设串s的长度为n，串t的长度为m。在KMP算法中求next数组的时间复杂度为O(m)，在后面的匹配中因主串s的下标不减即不回溯，比较次数可记为n，所以KMP算法平均时间复杂度为**O(n+m)**。
- 最坏的时间复杂度为**O(nxm)**，这和BF算法的平均时间复杂度相同。

【例 4.5】见`Alogorithm.md`

### 4.3.3 改进的KMP算法
- 原因：字符不匹配时，next数组可能进行多次无意义比较。
- 解决：在next数组的基础上，把字母相同的next改为前一个不相同的字母的下标，从而避免重复的无意义比较。也就是说改变next数组的算法就可以了，然后用nextval进行替代。

```C++
//  用nextval取代next，得到改进的KMP算法如下
void GetNextval(SqString t,int nextval[])
	int j=0,k=-1;
	nextval[0]=-1;
   	while (j<t.length) 
	{
       	if (k==-1 || t.data[j]==t.data[k]) 
		{	
			j++;k++;
			if (t.data[j]!=t.data[k]) 
				nextval[j]=k;
           	else  
				nextval[j]=nextval[k];
       	}
       	else  k=nextval[k];    	
	}

}

// 修正的KMP算法
int KMPIndex1(SqString s,SqString t)    
{
	int nextval[MaxSize],i=0,j=0;
	GetNextval(t,nextval);
	while (i<s.length && j<t.length) 
	{
		if (j==-1 || s.data[i]==t.data[j]) 
		{	
			i++;j++;	
		}
		else j=nextval[j];
	}
	if (j>=t.length)  
		return(i-t.length);
	else
		return(-1);
}
```

【例 4.6】见`Alogorithm.md`

### 得到的启示
- 从BF -> KMP 其实是利用了模式串部分的匹配信息，将其保存并运用，从而改进算法效率。

## 本章小结
本章的基本学习要点：
- 理解串和一般线性表之间的差异
- 掌握在顺序串上和链串上实现串的基本运算算法设计
- 掌握串的简单匹配算法，理解KMP算法的高效匹配过程
- 灵活地运用串这种数据结构解决一下综合应用问题

### 1.串的存储
- 1.串是一种特殊的线性表，是线性表的一个子集。（包含顺序串和链串）
- 2.链串只能采用单链表吗？
  - 不一定，视情况而定。
  - 如果需要从某个节点出发，进行前后查找。那么使用双链表更合适。只不过双链表存储密度低了一点。
  - 如果想要快速找到某个尾结点，可以用循环双链表。

### 2.串的算法设计

1）串的基本算法设计
- 借鉴线性表的算法设计方法
- 顺序串 <-> 顺序表
- 链  串 <-> 单链表

2）串的模式匹配算法
- BF算法
- KMP算法。基于BF算法改进，利用了前面的部分匹配信息。但最坏情况下两者时间复杂度相同。如，模式串全都不相同，这时候就退化为了BF算法。
 