# C-linkList
# <a href="https://www.cnblogs.com/lanhaicode/p/10304567.html">链表（单向链表的建立、删除、插入、打印）</a>
# <a href="https://www.cxyxiaowu.com/1400.html">链表算法面试问题？看我就够了！</a>
c 语言链表专题
# 链表实例1 vs2013版本
// 链表基本操作.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include<malloc.h>

typedef struct Node{
	int data;
	struct Node* next;

}NODE;

//NODE* ArrToList(int arr[], int count){
//	NODE *links[count];//不要使用，这个会报错
//	int j = 0;
//	NODE *tmp = (NODE*)malloc(sizeof(NODE));
//	for (int i = 0; i<count; i++){
//		links[i] = (NODE*)malloc(sizeof(NODE));
//		links[i]->data = arr[i];
//		links[i]->next = NULL;
//		j++;
//	}
//	for (int i = 0; i<j; i++){
//		links[i]->next = links[i + 1];
//	}
//	tmp = links[0];
//	/*   for(int i=0;i<j;i++){
//	free(links[i]);
//	}*/
//	return  tmp;
//}
void InitNode(NODE *head){
	if ((head = (NODE*)malloc(sizeof(NODE))) == NULL){
		return;
	}
	head->next = NULL;
}

NODE *initnode(NODE *pnode, int data)
{
	pnode = (NODE *)malloc(sizeof(NODE));
	pnode->data = data;//初始化数据域
	pnode->next = NULL;//初始化指针域为NULL

	return pnode;
}

NODE *createtnode(int data){
Node* pnode = (NODE *)malloc(sizeof(NODE));
	pnode->data = data;//初始化数据域
	pnode->next = NULL;//初始化指针域为NULL

	return pnode;
}

//创建一个新节点(头插法)
NODE *createlink_byhead(NODE *phead, int data)
{
	NODE *pnode = (Node*)malloc(sizeof(Node));
	pnode = initnode(pnode, data);//初始化一个结点
	NODE *ptmp = phead;

	if (NULL == phead){    //链表为空，直接返回初始化的结点
		return pnode;
	}
	else{
		phead = pnode;    //将新申请的结点插在phead后面，也就是整个链表的最前面
		pnode->next = ptmp;
	}

	return phead;
}

//创建一个链表节点(尾插法)
NODE *createlink_bytail(NODE *phead, int data)
{
	NODE *pnode = (Node*)malloc(sizeof(Node));
	pnode = initnode(pnode, data);//初始化结点
	NODE *ptmp = phead;

	if (NULL == phead){//当链表为空，直接返回初始化的结点
		return pnode;
	}
	else{
		while (ptmp->next != NULL){//找到最后一个结点，插在尾部
			ptmp = ptmp->next;
		}
		ptmp->next = pnode;
	}

	return phead;
}

//求链表长度
int linklen(NODE *phead)
{
	int len = 0;//计数器
	NODE *ptmp = phead;

	while (ptmp != NULL){//遍历链表，计数器加1
		len++;
		ptmp = ptmp->next;
	}

	return len;
}

//按值查找
NODE *searchnode(NODE *phead, int key)
{
	NODE *ptmp = phead;

	if (NULL == phead){    //链表为空，直接返回
		return NULL;
	}
	while (ptmp->data != key && ptmp->next != NULL){//没找到key && 链表没找完
		ptmp = ptmp->next; //指针移动
	}
	if (ptmp->data == key) //找到key，返回该结点地址
		return ptmp;
	if (ptmp->next == NULL)//没找到，返回空
		return NULL;
}

//向前插入
NODE *insertnode_bypre(NODE *phead, int data, int key)
{
	NODE *pnode = (Node*)malloc(sizeof(Node));
	pnode = initnode(pnode, data);//初始化插入的结点
	NODE *ptmp = phead;

	if (NULL == phead){//链表为空，直接返回初始化的结点
		return pnode;
	}
	else if (phead->data == key){//处理第一个结点是否为目标结点
		phead = pnode;
		pnode->next = ptmp;
	}
	else{
		while (ptmp->next != NULL && ptmp->next->data != key){//循环遍历，没找完 && 没找到
			ptmp = ptmp->next;
		}
		if (NULL == ptmp->next){//没找到的情况
			printf("insert key NOT FOUND\n");
		}
		else{//把新节点插入目标结点的前面
			ptmp->next->data == key;
			pnode->next = ptmp->next;
			ptmp->next = pnode;
		}
	}
	return phead;
}

//向后插入
NODE *insertnode_byback(NODE *phead, int data, int key)
{
	NODE *pnode = (Node*)malloc(sizeof(Node));
	pnode = initnode(pnode, data);//初始化插入的结点
	NODE *ptmp = searchnode(phead, key);//查找到目标结点

	if (NULL == ptmp){//处理链表为空，或没找到
		printf("link is empty or not found key\n");
		return phead;
	}
	if (NULL == ptmp->next){//如果key为最后一个结点
		ptmp->next = pnode;
	}
	else{ //将新节点插入在目标结点的后面
		pnode->next = ptmp->next;
		ptmp->next = pnode;
	}

	return phead;
}

Node* addTail(Node* phead, int data){
	NODE *pnode = (Node*)malloc(sizeof(Node));
	pnode = initnode(pnode, data);//初始化插入的结点
	Node*ptail = phead;
	if (NULL == phead){
		phead = pnode;
	}
	else
	{
		while (ptail->next != NULL){
			ptail = ptail->next;

		}
		ptail->next = pnode;
		pnode->next = NULL;
		ptail = pnode;
		ptail->next = NULL;
		
	}
	return phead;
}
//删除节点
NODE *delnode(NODE *phead, int key)
{
	NODE *ptmp = phead;
	NODE *tmp = NULL;

	if (NULL == phead){//处理链表为空的情况
		printf("link is empty, del fail\n");
		return NULL;
	}
	else if (phead->data == key){//单独处理第一个结点
		phead = phead->next;
		free(ptmp);
		ptmp = NULL;
	}
	else{
		while (ptmp->next != NULL && ptmp->next->data != key){ //没找完 && 没找到
			ptmp = ptmp->next;
		}
		if (NULL == ptmp->next){ //没找到
			printf("del key NOT FOUND\n");
			return phead;
		}
		if (ptmp->next->data == key){ //找到目标结点删除之
			tmp = ptmp->next;
			ptmp->next = tmp->next;
			free(tmp);
			tmp = NULL;
		}
	}
	return phead;
}

//链表的倒置
NODE *reverselink(NODE *phead)
{
	NODE *ptmp = NULL;//创建一个新链
	NODE *tmp = NULL;

	if (NULL == phead){//处理链表为空
		printf("link is empty\n");
		return NULL;
	}
	else{
		while (phead != NULL){//将旧链上的结点链到新链上
			tmp = phead;
			phead = phead->next;
			tmp->next = ptmp;
			ptmp = tmp;
		}
	}
	return ptmp;
}

//链表排序(冒泡)
NODE *linksort(NODE *phead)
{
	NODE *p = NULL; //i
	NODE *q = NULL; //j
	NODE *r = NULL;
	NODE tmp;

	for (p = phead; p; p = p->next){
		for (q = p->next; q; q = q->next){
			if (p->data > q->data){
				tmp = *p;//整个结点交换
				*p = *q;
				*q = tmp;
				r = p->next;//将指针换回去
				p->next = q->next;
				q->next = r;
			}
		}
	}

	return phead;
}


int _tmain(int argc, _TCHAR* argv[])
{
	Node* tmp = (Node*)malloc(sizeof(Node));
	Node* node = createtnode(11);
	tmp = node;
	tmp = addTail(tmp, 22);
	tmp = addTail(tmp, 33);
	tmp = addTail(tmp, 44);
	printf("length=%d\n", linklen(node));
		while (node){//这里如果写成while(node->next!=NULL){...},那么最后一个节点的数据用不到
		printf("data=%d\n", node->data);
		node = node->next;
	}

	getchar();
	return 0;
}

 
