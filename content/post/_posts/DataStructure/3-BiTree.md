---
draft: true # 是否为草稿
title: 二叉树
date: 2020-09-30 11:33:52
tags: DataStructure
categories: DataStructure
status: public
---

# 二叉树

链式存储二叉树

## 二叉树结构

```c
typedef struct BiTNode{
    ElemType data;
    BiTNode *lchild,*rchild;
}BiTNode,*BiTree;
```



## 二叉树接口

```c
void InitBiTree(BitTree &T);
BTNode* BiTreeCreate(BiTree* a, int* pi);
void BiTreeDestory(BiTNode** root);
int BiTreeSize(BiTNode* root);
int BiTreeLeafSize(BiTNode* root);
int BiTreeLevelKSize(BiTNode* root, int k);
BTNode* BiTreeFind(BiTNode* root, BTDataType x);

// 遍历 递归
void BiTreePrevOrder(BiTNode* root);//前序遍历
void BiTreeInOrder(BiTNode* root);//中序遍历
void BiTreePostOrder(BiTNode* root);//后序遍历
void BiTreeLevelOrder(BiTNode* root);//层序遍历
 
// 遍历 非递归
void BiTreePrevOrderNonR(BiTNode* root);//前序遍历的非递归
void BiTreeInOrderNonR(BiTNode* root);//中序遍历的非递归
void BiTreePostOrderNonR(BiTNode* root);//后序遍历的非递归

```

