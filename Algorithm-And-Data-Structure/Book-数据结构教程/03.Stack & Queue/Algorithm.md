## 题目列表
- 1.十进制整数转二进制
- 2.指定出栈序列

### 1.十进制整数转二进制
- 之前在LeeCode做的十进制转二进制就可以用栈来实现啊！
```C++
#include "sqstack1.cpp"

void trans(int n)
{
    int e;
    SqStack*st;
    InitStack(st);

    while(n > 0)
    {
        Push(st, n % 2);
        n /= 2;
    }

    while (!StackEmpty(st))  // 输出对应的二进制数
    {
        Pop(st, e);
        printf("%d", e);
    }

    printf("\n");
}
```

### 2.指定出栈序列
- 假设输入序列是1、2、...、n。设计一个算法判断通过一个栈能否得到由a\[0..n-1](为1、2、...、n的某个排列)指定出栈序列。
```C++
bool Validseq(int a[], int n)
{
    int i, e, k = 0;
    bool flag;
    SqStack* st;
    InitStack(st);

    for(i = 1; i <= n; i++)  // 处理输入序列1、2、...、n
    {
        Push(st, i);
        while (!StackEmpty(st))
        {
            GetTop(st, e);
            if(a[k] == e)
            {
                Pop(st, e);
                k++;
            }
            else
                break;  // 不匹配时退出while循环
        }
    }
    flag = StackEmpty(st);
    DestroyStack(st);
    return flag;
}
```