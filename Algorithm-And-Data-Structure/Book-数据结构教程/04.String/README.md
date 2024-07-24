## 一、串的基本概念
```C++
//串（string）是由零个或多个字符组成的有序数列
/*
1.含零个字符的串称为空串，用空集的符号表示。
2.串所含字符的个数称为该串的长度（或串长）。
3.两个串相等当且仅当这两个串的长度相等并且各对应位置上的字符都相同
4.字串：一个串中任意个字符组成的序列。字串含空串和自己本身
5.顺序串的存储方式有两种：
非紧缩格式：每个字只存一个字符。这种方式比较浪费内存空间 ，但处理单个字符或一组字符比较方便。 
紧缩格式：每个字存放多个字符。这种方式节省内存空间，但处理单个字符不太方便，
运算效率低，需要花费时间从一个字中分离字符*/ 
#include"stdafx.h"
#include"stdlib.h"
typedef int ElemType;
typedef struct
{
	char data[MaxSize];
	int length;
}SqString;

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
//1.生成串
void StrAssign(SqString &s, char cstr[])  //cstr[] 相当于 *cstr,s为引用型参数 
{
	int i;
	for(i=0;cstr[i]!='\0';i++)
		s.data[i]=cstr[i];
	s.length=i;   // 设置串的长度 
} 
// 2.销毁串
/*这一章的顺序串是直接采用顺序串本身来表示的，而不是顺序串指针，它的存储空间由操作系统管理，
即由操作系统分配其存储空间，并在超过作用域时释放其存储空间，所以这里的销毁串不包含任何操作*/
void DestroyStr(SqString &s)
{
} 
// 3.串的复制
void StrCopy(SqString&s,SqString t)
{
	int i;
	for(i=0;i<t.lenght;i++)
		s.data[i]=t.data[i];
	s.lenght=t.length;
} 
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
// 5.求串长StrLength(s)
int String(SqString s)
{
	return s.length;
} 
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
## 二、串的存储结构

## 三、串的模式匹配
### 4.3.2 KMP算法
```C++

```
### 4.3.3 改进的KMP算法
```C++

```
