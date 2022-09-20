---
draft: true # 是否为草稿
title: 栈与队列
date: 2020-09-28 08:59:48
tags: DataStructure
categories: DataStructure
status: public
---

# 栈

只允许在一端进行插入或删除的线性表（弹匣）

## 顺序栈

### 顺序栈结构

```c
#define MaxSize 10
typedef struct{
    ElemType data[MaxSize]; //存放栈中元素
    int top					//栈顶游标
}SqStack;
```

### 初始化顺序栈

将栈顶游标指向-1,表示栈内没有元素

```c
void InitSqStack(SqStack &S){
    SqStack->top = -1;
}
```



### 入栈

即从栈顶插入元素

```c
bool Push(SqStack &S,int e){
    if (S.top==MaxSize-1){
        return false;
    }
    S.data[++S.top] = e;
    return true;
}
```

### 出栈

```c
bool Pop(SqStack &S,int &e){
     if(S.top==-1){
         return false;
     }
    e = S.data[S.top--];
    return true
}
```



### 查询栈顶

```c
bool GetTop(SqStack S,int e){
    if (S.top == -1){
        return false;
    }
    e = S.data[S.top];
    return true;
}
```





## 链栈

即 使用链表实现的栈（阉割版链表）,所以结构与单链表几乎相同

* 链表头为栈顶，表尾为栈底
* 入栈通过头插法入栈
* 头节点为栈顶指针(*Top)



### 链栈结构

```c
typedef struct LinkNode{
    ElemType data;	//数据
    LinkNode *next;	//存放下一个节点的地址
}LinkNode,*LiStack;
```



### 初始化链栈

```c
bool InitLinkStack(LiStack &S){
    S = (LiStack)malloc(sizeof(LinkNode));	//头节点不存放数据
    S->next = NULL;							//后继为空（判断栈空的条件）
    return true;
}
```



### 入栈

使用头插法插入元素，即从表头加入元素

```c
bool Push(LiStack &S,int e){
    LinkNode *p = (LinkNode *)malloc(sizeof(LinkNode));
    p->data = e;		//存入数据
    p->next = S->next;	//将后继连向原栈顶
    S->next = p;		//将节点设为栈顶
    return true;
}
```



### 出栈

从表头弹出元素，得到值后释放节点

```c
bool Pop(LiStack &S,int &e){
    if (S->next == NULL){
        return false;
    }
    LinkNode *p = S->next;
    S->next = p->next;		//将栈顶节点移出
    e = p->data;			//将栈顶节点的值保存
    free(p);				//释放节点
    return true;
}
```



### 查询栈顶

类似出栈操作，区别在于不会删除栈顶元素

```c
bool GetTop(LiStack S,int &e){
    if (S->next ==NULL){
        return false;
    }
    e = S->next->data;	//取出栈顶元素的值
    return true;
}
```



### 判断空栈

当头节点的后继为空，即只有一个头节点时，栈为空

```c
bool isStackEmpty(LiStack S){
    if (S->next == NULL){
        return true;
    }else{
        return false;
    }
} 
```



# 队列

## 顺序队列

### 顺序队列结构

```c
#define MaxSize 50	//队列的大小
typedef struct {
    ElemType data[MaxSize];
    int front,rear;	//队头游标和队尾游标
}SqQueue;
```



### 初始化顺序队列

队头每出队一个元素就`-1`，当队列没有元素时，`队头指向0`，即从`第0个元素`开始出队

队尾每插入一个元素就`+1`，当队列中没元素时，`队尾指向-1`

```c
bool InitQueue(SqQueue &Q){
    Q->front = 0;	//队头
    Q->rear = -1;	//队尾
}
```



### 循环队列

将顺序队列的数组看作一个环，通过`模除取余(%)`将数组的首尾相关联

结构与顺序队列相同，`区别在于满队和空队的条件不同`

有三种方式区分队空和队满：

1. 牺牲一个队员来区分队空和队满
2. 定义一个数据表示元素个数
3. 定义一个数据用于表示数据的输入和输出状态



## 链式队列

### 链式队列结构

```c
typedef struct LinkNode{
    ElemType data;
    LinkNode *next;
}LinkNode;

typedef struct{
    LinkNode *front,*rear;
}QueueLink;
```



### 初始化链式队列

```c
bool InitQueue(QueueLink &Q){
    Q.front = Q.rear = (QueueLink)malloc(sizeof(LinkNode));
    Q.front->next = NULL;
}
```