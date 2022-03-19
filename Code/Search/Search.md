# 待办
- [ ] 看分块查找视频

# 一、查找的概念

## 1.查找的性能指标 - ASL
- 查找的时间主要花费在关键字比较上，平均查找长度**ASL(Average Search Length)** 是衡量一个算法效率优劣的标准
- ASL成功是指：找到T中任一记录，平均需要的关键字比较次数
- ASL失败：没有查找到表中元素，平均需要的关键字比较次数

## 2.本章涉及到的算法
- 线性表的查找
  - 顺序查找
  - 折半查找
  - 索引存储结构和分块查找
  
- 树表查找
  - 二叉排序树
  - 平衡二叉树
  - B-树
  - B+树
  
# 二、线性表的查找

- 顺序表：静态查找
- 链表：动态查找和静态查找都有

```C

typedef int KeyType;
typedef struct
{   keyType key;
    InfoType data;
}RecType;

```

## 1.顺序查找
- **顺序查找** (sequential search)：从表的一端到另一端逐个将元素的关键字和给定值k比较，若相等，则查找成功，给出该元素在查找表中的位置；若查找表扫描结束后扔未找到关键字等于k的元素，则查找失败。

```C

// ASL成功 = (1 + n) / 2；ASL失败 = n
// 平均时间复杂度：O(n)
int SeqSearch(RecType R[], int n, KeyType k)
{   int i=0;
    while(i < n && R[i].key != k)
        i++;
    
    if(i >= n)
        return 0;
    else
        return i+1;
}

```

```C
// 在末尾增加“哨兵”，无需再判断越界
int SeqSearch1(RecType R[], int n, KeyType k)
{   int i = 0;
    R[n].key = k;
    
    while(R[i].key != k)
        i++;
    if(i == n)
        return 0;
    else
        return i+1;
}
```


## 2.折半查找
- 折半查找(binary search)：又称二分查找，它是一种效率较高的查找算法。但是，折半查找要求线性表是有序表， 即表中的元素按关键字。

```C
// ASL = log2^(n + 1) - 1
// O(log2^n)
int BinSearch(RecType R[], int n, KeyType k)
{   int low = 0, high = n-1, mid;
   
   while(low <= high)
    {   mid = (low + high) / 2;
        
        if(k == R[mid].key)
            return mid + 1;
        
        if(k < R[mid].key)
            high = mid - 1;
        else 
            low = mid + 1;
    }
    
    return 0;
}
```

## 3.索引存储结构和分块查找
- **索引存储结构**(index storage structure)是在存储数据的同时还建立附加的索引表。索引表中的每一项称为索引项，索引项的一般形式为（关键字，地址）。OS中的内存管理就有这种方式
- **分块查找**(block search)是一种性能介于索引查找和折半查找之间的查找算法。

```C
// 索引表的数据类型声明
# define MAXI<索引表的最大长度>
typedef struct
{
    KeyType key;
    int link;  // 指向对应块的起始下标
}IdxType;  // 索引表元素的类型
```

```C
// 采用折半查找索引表的分块查找算法

```


# 三、树表的查找

## 1.二叉排序树

- 二叉排序树
- 平衡二叉树
- B-树
- B+树


# 四、哈希表查找
