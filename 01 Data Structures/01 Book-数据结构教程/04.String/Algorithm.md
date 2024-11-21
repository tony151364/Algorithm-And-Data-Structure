
## 【例 4.1】
题目描述：
- 假设串采取顺序存储结构，设计一个算法Strcmp(s, t)按字典顺序标胶两个串s和t的大小。

```C++
int Strcmp(SqString s,SqString t)
{
	int i,comlen;
	if (s.length<t.length) 
        comlen=s.length;	//求s和t的共同长度
	else 
        comlen=t.length;

	for (i=0;i<comlen;i++)			//在共同长度内逐个字符比较
		if (s.data[i]>t.data[i])
			return 1;
		else if (s.data[i]<t.data[i])
			return -1;
	if (s.length==t.length)			//s==t
		return 0;
	else if (s.length>t.length)		//s>t
		return 1;
	else  return -1;				//s<t
}
// 字符比较

// 长度比较
```
- 感觉课本给出的示例代码还有可以改进的地方。所以自己又重新写了一版。
```C++
int Strcmp(SqString s,SqString t)
{    
    //字符比较
	for (int i = 0; (i < s.length && i < t.length); i++)			
    {
        if (s.data[i] > t.data[i])
        {
            return 1;
        }
		else if (s.data[i] < t.data[i])
        {
            return -1;
        }
    }

    // 长度比较
	if (s.length == t.length)
    {
        return 0;
    }
	else if (s.length > t.length)
    {
        return 1;
    }
	else  
    {
        return -1;
    }
}
```

## 【例 4.2】
题目描述：
- 假设串采用顺序串存储，设计一个算法求串s中出现的第一个最长的连续相同字符构成的平台

```C++
void LongestString(SqString s,int &index,int &maxlen)
{
	int length,i=1,start;		//length保存平台的长度
	index=0,maxlen=1;			//index保存最长平台在s中的开始位置,maxlen保存其长度
	while (i<s.length)
	{
		start=i-1;				//查找局部重复子串
		length=1;
		while (i<s.length && s.data[i]==s.data[i-1])
		{	
			i++;
			length++;
		}
		if (maxlen<length)		//当前平台长度大,则更新maxlen
		{	
			maxlen=length;
			index=start;
		}
		i++;
	}
}
```

## 【例 4.3】
题目描述：
- 假设串采用链串存储，设计一个算法把串s中最先出现的的子串"ab"改为"xyz"
  
```C++
void Repl(LinkStrNode *&s)
{
	LinkStrNode *p=s->next,*q;
	int find=0;
	while (p->next!=NULL && find==0)	//查找'ab'子串
	{
		if (p->data=='a' && p->next->data=='b')   	//找到了ab子串
		{	
			p->data='x';p->next->data='z';			//替换为xyz
			q=(LinkStrNode *)malloc(sizeof(LinkStrNode));
			q->data='y';q->next=p->next;p->next=q;
			find=1;
		}
		else p=p->next; 
	}
}
```

## 【视频例题 1】
题目描述：
- 假设串采用顺序串存储结构。设计一个算法求串t在串s中出现的次数，如果不是子串返回0。采用BF算法求解。

解题思路：
- 使用count累计t在串2中出现的次数（初始值为0）
- 采用BF方法，在s中找到子串t后不是退出，而是count增加1，i置k并继续查找，知道整个字符串查找完毕。
```C++
int StrCount1(SqString s, SqString t)
{
	int i=0,j, k, count=0;

	while (i <= (s.length - t))
	{
        for (k = i, j = 0; k < s.length && j < t.length && s.data[k] == t.data[j]; k++, j++);

		if (j == t.length)
		{
			count++;
            i = k;
		}
        else
            i++;
	}

	return count;
}
```

## 【例 4.5】
题目描述：
- 设目标串s="aaaaab", 模式串t = "aaab"。给出KMP进行模式匹配的过程。

```C++
// 视频讲解的很清晰，这里就不描述了，这道题不需要代码。
```

## 【视频例题 2】
题目描述：
- 假设串采用顺序串存储结构。设计一个算法求串t在串s中出现的次数，如果不是子串返回0。采用KMP算法求解。（之前题目的用KMP求解）

解题思路：
- 使用count累计t在串2中出现的次数（初始值为0）
- 采用KMP方法，当匹配成功时，count增加1，并且置j为0重新开始比较。
```C++
int StrCount1(SqString s,SqString t)
{
	int count = 0,i = 0,j = 0;
	int next[MaxSize];
	
	GetNext(t,next);

	while (i<s.length && j<t.length) 
	{
		if (j==-1 || s.data[i]==t.data[j]) 
		{
			i++;
			j++;
		}
		else 
			j=next[j];

		if (j>=t.length)
		{
			count++;
			j = 0;  // j设置为0，继续匹配
		}
    }

	return count;        		//返回不匹配标志
}
```

## 【例 4.6】
题目描述：设目标串 s="abcaabbabcabaacbacba", 模式串 t = "abcabaa"，计算模式串t的nextvalue函数值，并给出利用改进KMP算法进行模式匹配的过程。
```C++
// 无代码，略
```

## 【视频例题 3】

题目描述：
假设串采用字符数组存储。修改BF和KMP算法，在一个字符串s中查找子串t的出现次数。例如，对于s=“abcabcabcd”，t=“abcab”。这里认为t在s中出现2次（考虑重叠部分）。
```C++
// BF：不跳过重叠字符。
int SubstrCount1(char* s, char* t)
{
	int i, j, k, count = 0;
	int m = strlen(s);
	int n = strlen(t);

	for (i = 0; i <= (m - n); i++)
	{
		for (k = i, j = 0; k < m && j < n && s[k] == t[j]; k++, j++);

		if (j == n)
		{
			count++;
		}
	}

	return count;
}
```

```C++

void GetNext1(char* t,int next[])		//由模式串t求出next值
{
	int j, k;
	int n = strlen(t);
	j=0; k=-1; next[0]=-1;

	while (j < n)  // 改为需要求next[n]
	{	
		if (k==-1 || t.data[j]==t.data[k])
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

int SubstrCount12(SqString s,SqString t)  //KMP算法
{
	int next[MaxSize],i=0, j=0, count = 0;
	GetNext1(t,next);
	while (s[i] && t[j]) 
	{
		if (j==-1 || s[i]==t[j]) 
		{
			i++;j++; 
		}
		else j=next[j];

		if (!t[j])
		{
			count++;
			j = next[j];  // 改为将j设置为next[j]，继续匹配
		}
    }
	
	return count;
}
```


