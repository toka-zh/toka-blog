---
draft: true # 是否为草稿
title: 链表
date: 2020-09-26 21:46:31
tags: DataStructure
categories: DataStructure
status: public
---

# 单链表

线性表的链式存储

## 节点结构

节点内存放元素的信息(data)和指向后继的指针(next)

```c
typedef struct LNode{
    ElemType data;  //数据域 ElemType为数据类型，如int
    LNode *next;    //指针域
}LNode,*LinkList;	//LNode的别名
```



每次创建节点需要申请大小为`LNode`的空间

```c
LNode *p = (LNode *)malloc(sizeof(LNode));
```

每次删除节点需要释放被删除节点的空间

```c
free(p);
```



节点声明，两行作用相同，通过使用别名强调节点的作用

```c
LNode *L;		//强调这个是节点
LinkList L;		//强调这个是单链表
```



### 别名相关知识

`typedef`关键字--数据类型重命名

```c
typedef <数据类型> <别名>;
```

（别名前带有`*`号表示该类型的指针的别名）





## 头指针

头指针始终指向链表第一个节点，通常使用带头节点的链表，便于操作

### 带头节点

头节点是链表第一个节点，通常不存放数据，头节点指向的下一个节点是存放数据的第一个节点，头节点类似于数组结构中的第0个元素，可以使`在表头的插入操作更方便`

```c
L = (LinkList)malloc(sizeof(LNode));
```



### 不带头节点

直接指向第一个节点，通常不用这个

```c
L = NULL;
```



## 主要操作(接口)

其他的操作基本都基于这几个函数，无非就是基本函数间嵌套使用罢了

```c
//头节点生成（即初始化链表）
bool InitHeadNode(LinkList &L);

//单个节点的前后插入操作
//参数：*p(链表节点的指针类型)
//		 e(节点数据类型，整形)
//功能：在节点p的前驱(后继)插入一个值为e的节点
//说明：开辟一个LNode内存，存放e,连入p的前驱(后继)
bool InsertPriorNode(LNode *p,int e);
bool InsertNextNode(LNode *p,int e);

//查找元素位置
//参数：L(链表类型，即等同于LNode* 类型的链表头节点指针)
//		i(整型，要查找节点的位置)
//		e(节点数据型，查找的第一个值为e的节点)
//功能：查找链表第找到第i个或者第一个值为e的节点
//说明：传入位置或值，遍历链表，找到并返回指向这个节点的指针
LNode *GetElem(LinkList L,int i);
LNode *LocateElem(LinkList L,int e);

//删除元素
//参数：*p(要删除的节点的前驱节点)
//功能：删除p的后继节点
//说明：将p的后继节点s取出，把p的后继指向s的后继，然后释放s的内存
bool DeleteNextNode(LNode *p);

bool TailCreate(LinkList &L);
bool HeadCreate(LinkList &L);
```



## 操作实现

### 初始化

初始化一个带头节点的单链表，此时链表内只有一个头节点，头节点内不存放数据，且后继为空

```c
bool InitLinkList(LinkList &L){
    L = (LNode *)malloc(sizeof(LNode));	//创建头节点
    L.next = NULL;	//后继为空
    return true;
}
```



### 插入

#### 前插

在节点前插入

假设有链表中的节点p,现在要将元素e插入到节点p前面

##### 方法一

直接在节点前插入

需要遍历到被插节点的前一个节点，然后使用后插插入，因为需要遍历一次链表，所以时间复杂度为O(n),较为费时，通常使用方法二

##### 方法二

创建`节点s`存放`节点p`的值

在`节点p`后插入`节点s`

最后将`节点p`的值`替换为e`



这样的时间复杂度为O(1)

```c
bool HeadInsert(LNode *p,int e){
    if (L == NULL){
        return false;
    }
    LNode *s = (LNode*)malloc(sizeof(LNode));	//新建节点s
    s->data = p->data;	//将p的数据存入到s中
    p->data = e;		//将p的数据替换为e
    s->next = p->next;	//将s指向p的下一个节点
    p->next = s;		//将s设为p的后继
    retuen true;
}
```



#### 后插

创建一个新节点，插入到传入节点的后继

```c
bool TailInsert(LNode *L,int e){
    if (L == NULL){
        return false;
    }
    LNode *p = (LNode *)malloc(sizeof(LNode));
    p->data = e;
    p->next = L->next;
    L->next = p;
    return true;
}
```



### 查询

#### 按序号查找节点

从第一个节点出发，顺指针next域逐个往下搜索，知道找到第i个节点为止，否则返回最后一个节点的指针域NULL

```c
LNode *GetElem(LinkList L,int i){
    LNode *p = L->next;	//p指向存放值的第一个节点
    int j = 1;			//计数器，用于记录节点的位置
    if (i==0){			//第0个节点为头节点
        return L;
    }
    
    //遍历链表
    //作用：从第1个节点开始找，查找到第i个节点
    while(p->next!=NULL&&j<i){
        p = p->next;
        j++;
    }
    
    return p;			//返回节点指针
}
```



#### 按值查找表节点

```c
LNode *LocateElem(LinkList L,ElemType e){
    LNode *p = L->next;//p指向存放值的第一个节点
    //遍历链表
    //作用：从第1个节点开始查找data域为e的结点
    while(p->next!=NULL&&p->data!=e){
        p = p->next;
    }
    return p;			//返回节点指针
}
```



### 删除

#### 删除下一个节点

```c
bool DeleteNode(LNode *p,ElemType &e){
    if (L == NULL||L->next == NULL){
        return false;
    }
    LNode *q = p->next; //令q指向p的后继节点
    p->next = q->next;	//令p的后继直线q的后继，即断开q
    e = q->data;		//传出节点q的值
    free(q);			//释放q的存储空间
  	return true;
}
```



#### 删除第i个节点

先使用getElem获得第i-1个节点p(i的前驱节点)

然后用DeleteNode删除p的后继节点，即第i个节点

```c
bool DeleteNNode(LinkList &L,int i){
    int e;
    LNode *p = getElem(L,i-1);
    DeleteNode(p,e);
}
```



## 单链表的建立

### 尾插法

```c
bool TailCreate(LinkList &L){
    LNode *p = GetElem(L,NULL);//获取最后一个元素的指针
    int n;
    scanf("%d",&n);
    if(n!=777){
        InsertNextNode(p,n);
        p = p->next;
        scanf("%d",&n);
    }
    return true;
}
```



### 头插法

常用于列表的***逆置***

```c
bool HeadCreate(LinkList &L){
    int n;
    scanf("%d",&n);
    while (n!=777){
        InsertNextNode(L,n);
        scanf("%d",&n);

    }
    return true;
}
```



## 附录-逆置链表

使用头插法倒置链表

```c
bool ContrastList(LinkList &L){
    LNode *p = L->next;
    while (p->next!=NULL){

        LNode *q = p->next; //取出节点
        p->next = q->next;  //在列表中移除节点，并且后移

        //头插节点
        q->next = L->next;
        L->next = q;
    }
    return true;
}
```





# 双链表

相比与单链表，双链表的每个节点都增加了`指向前驱节点的指针`

## 双列表结构

```c
typedef struct DNode{
    ElemType data;
    DNode *prior,*next;	//前驱指针，后继指针
}DNode,DLinkList;
```



## 初始化双链表

```c
bool InitDLinkList(DLinkList &L){
    L = (DLinkList *)malloc(sizeof(DNode));
    L->prior = NULL;
    L->next = NULL;
}
```



## 删除节点

```c
bool DeleteDNode(DNode *p){
    if (p==NULL){
        return false;
    }
    p->prior->next = p->next;
    if (p->next!=NULL) {
        p->next->prior = p->prior;
    }
    free(p);
    return true;
}
```



# 循环链表 

## 循环单链表

循环列表的`尾节点指针指向头节点`

## 循环双链表

循环双链表的`头节点前驱指针指向尾节点`，`尾节点的后继指针指向头节点`



# 静态链表

使用`数组方式`实现的链表，每个节点中的游标存放的是后继节点的数组`下标`，而非指向后继节点的指针



* 优点：增，删操作不需要大量移动元素

* 缺点：不能随机存取，只能从头节点开始依次向后遍历，`容量固定不变`

使用场景：

1. 不支持指针的低级语言
2. 数据元素数量固定不变的场景（如操作系统的文件分配标FAT）

## 初始化静态链表

写法一

```c
#define MaxSize 10
typedef struct{
    ElemType data;
    int next;    //游标，即指向对应数据的数组下标
} SLinkList[MaxSize];
//SLinkList a;//相当于定义了一个成功定义MaxSize的SNode型数组
```

写法二

```c
#define MaxSize 10
struct SNode{
    ElemType data;
    int next;
};
typedef struct SNode SLinkList[MaxSize];//可用SLinkList定义“一个程度尾MaxSize的SNode型数组”
//struct Node a[MaxSize]
```



