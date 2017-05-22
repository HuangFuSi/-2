# -2


#include <iostream>
#include <algorithm>
#include "stdio.h"
#include <math.h>
#include "malloc.h"
#include <stack>
#define TRUE 1
#define FALSE 0
#define OK  1
#define ERROR  0
#define INFEASIBLE -1
#define OVERFLOW -2
#define STACK_INIT_SIZE 100 // 存储空间初始分配量
#define STACKINCREMENT 10 // 存储空间分配增量
using namespace std;
typedef int  Status;
int n,m;
typedef int  ElemType;
typedef struct BiTNode
{
    ElemType data;
    struct BiTNode *lchild,*rchild;//左右孩子指针
} BiTNode,*BiTree;
int Depth(BiTree T);
Status SearchBST(BiTree T,int key,BiTree f,BiTree *p);
Status InsertBST(BiTree *T,int key);
Status CreateBST(BiTree &T)
{
   T=NULL;
   int i,j;
   for(i=0;i<m;i++)
   {
       scanf("%d",&j);
       InsertBST(&T,j);
   }
    return OK;
}
Status PrintElement( ElemType e )    // 输出元素e的值
{
    printf("%d ", e );
    return OK;
}// PrintElement
Status SearchBST(BiTree T,int key,BiTree f,BiTree *p)
{
    if(!T)
    {
        *p=f;
        return FALSE;
    }
    else if(key==T->data)
    {
        *p=T;
        return TRUE;
    }
    else if(key<T->data)
    {
        return SearchBST(T->lchild,key,T,p);

    }
    else return SearchBST(T->rchild,key,T,p);
}



Status InsertBST(BiTree *T,int key)
{
    BiTree p,s;
    if(!SearchBST(*T,key,NULL,&p))
    {
        s=(BiTree)malloc(sizeof(BiTNode));
        s->data=key;
        s->lchild=s->rchild=NULL;
        if(!p)
            *T=s;
        else if(key<p->data)
            p->lchild=s;
        else
            p->rchild=s;
        return TRUE;
    }
    else
        return FALSE;
}
Status PreOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {
        if(PrintElement(T->data))
            if(PreOrderTraverse(T->lchild,Visit))
                if(PreOrderTraverse(T->rchild,Visit))
                    return OK;
        return ERROR;
    }
    else return OK;

}
Status InOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {
        if(InOrderTraverse(T->lchild,Visit))
            if(PrintElement(T->data))
                if(InOrderTraverse(T->rchild,Visit))
                    return OK;
        return ERROR;
    }
    else return OK;

}
Status PostOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    if(T)
    {

        if(PostOrderTraverse(T->lchild,Visit))
            if(PostOrderTraverse(T->rchild,Visit))
                if(Visit(T->data))
                    return OK;
        return ERROR;
    }
    else return OK;

}

Status FInOrderTraverse(BiTree T,Status(*Visit)(ElemType e))
{
    //SqStack S,q;
    //InitStack(S);
    stack<BiTree> S;
    BiTNode *p,*p2;
    p=T;
    //printf("A");
    ElemType e;
    while(p||!S.empty())
    {
        if(p)
        {
            S.push(p);
            p=p->lchild;
            //printf("GET\n");
        }
        else
        {
            //printf("DE\n");
            p=S.top();
            Visit(p->data);
            S.pop();
            //printf("OK%d ",e);
            p=p->rchild;
        }
    }
    return OK;
}
void PrintNodeAtlevel(BiTree T,int level)
{
    if(NULL==T||level<1)return;
    if(1==level){
    cout<<T->data<<" ";
    return ;
    }
    PrintNodeAtlevel(T->lchild,level-1);
    PrintNodeAtlevel(T->rchild,level-1);
}
int Depth(BiTree T)
{
    int ld=0,rd=0;
    if(T!=NULL)
    {
        ld=Depth(T->lchild);
        rd=Depth(T->rchild);
    }
    else return 0;
    return ld>rd?ld+1:rd+1;
}
void LevelTraverse(BiTree T)
{
    if(NULL==T)
    return;
    int depth=Depth(T);
    //printf("%d\n",depth);
    for(int i=1;i<=depth;i++)
    {
        PrintNodeAtlevel(T,i);
    }
}
int leaf_number(BiTree T,int &num)             //求叶子总数
{
	if(T)
	{
		if(T->rchild==NULL && T->lchild==NULL) num++;
		else
		{
			leaf_number(T->lchild,num);
			leaf_number(T->rchild,num);
		}
	}
	return OK;
}
int swap_tree(BiTree &T)
{
	BiTree temp;
	if(T!=NULL)
	{
		temp = T->lchild;
		T->lchild = T->rchild;
		T->rchild = temp;
		swap_tree(T->lchild);
		swap_tree(T->rchild);
	}
	return OK;
}
int main()
{
int u,o;
   BiTree T,p;
   scanf("%d",&m);
   CreateBST(T);
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);
   printf("\n");
   scanf("%d",&u);
   printf("%d\n",SearchBST(T,u,NULL,&p));
   scanf("%d",&u);
   printf("%d\n",SearchBST(T,u,NULL,&p));
   scanf("%d",&u);
   InsertBST(&T,u);
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);
   printf("\n");
   FInOrderTraverse(T,PrintElement);
   printf("\n");
  LevelTraverse(T);
   swap_tree(T);
   printf("\n");
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);

   swap_tree(T);
   printf("\n");
   PreOrderTraverse(T,PrintElement);
   printf("\n");
   InOrderTraverse(T,PrintElement);
   printf("\n");
   PostOrderTraverse(T,PrintElement);
   printf("\n");
   int deep,num=0;
   deep=Depth(T);
   leaf_number(T,num);
   printf("%d\n%d\n",deep,num);


}
/*
5
7 4 9 5 8
5
6
2


标准输出答案:
   1|7 4 5 9 8
   2|4 5 7 8 9
   3|5 4 8 9 7
   4|1
   5|0
   6|7 4 2 5 9 8
   7|2 4 5 7 8 9
   8|2 5 4 8 9 7
   9|2 4 5 7 8 9
  10|7 4 9 2 5 8
  11|7 9 8 4 5 2
  12|9 8 7 5 4 2
  13|8 9 5 2 4 7
  14|7 4 2 5 9 8
  15|2 4 5 7 8 9
  16|2 5 4 8 9 7
  17|3
  18|3


你的错误输出结果:
   1|7 4 5 9 8
   2|4 5 7 8 9
   3|5 4 8 9 7
   4|1
   5|0
   6|7 4 2 5 9 8
   7|2 4 5 7 8 9
   8|2 5 4 8 9 7
   9|2 4 5 7 8 9
  10|7 4 9 2 5 8
  11|
  12|7 9 8 4 5 2
  13|9 8 7 5 4 2
  14|8 9 5 2 4 7
  15|7 4 2 5 9 8
  16|2 4 5 7 8 9
  17|2 5 4 8 9 7
  18|4
  19|3

*/
