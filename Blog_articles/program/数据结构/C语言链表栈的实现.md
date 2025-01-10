# C语言链表栈的实现

## 背景

栈是一种基础且重要的数据结构,广泛应用于编程语言的运行时栈、表达式求值、函数调用等场景。本文将介绍如何用C语言实现一个基于链表的栈。

## 链式栈的定义

链式栈使用链表作为底层存储结构,每个链表节点包含两个字段:

```c
cCopy code// 链表节点结构定义
typedef struct LinkNode {
  ElemType data; // 数据域 
  struct LinkNode* next; // 指针域
} Node;

typedef Node* Stack; // 栈类型
```

其中`ElemType`是元素类型,可以根据需要定义为`int`、`float`等。`next`指针指向下一个节点。`Stack`类型定义了栈,表示链表头指针。

## 基本操作实现

链式栈常见的基本操作有:

- 初始化栈
- 判断栈空
- 入栈
- 出栈
- 读取栈顶元素
- 销毁栈

这些操作可以按如下方式实现:

```c
cCopy code// 初始化栈
void initStack(Stack* s) {
  *s = NULL; 
}

// 判断栈空
bool isEmpty(Stack s) {
  return s == NULL;
}

// 入栈
void push(Stack* s, ElemType x) {
  Node* newNode = malloc(sizeof(Node));
  newNode->data = x;
  newNode->next = *s;
  *s = newNode;
}

// 出栈  
ElemType pop(Stack* s) {
  Node* top = *s;
  ElemType x = top->data;
  *s = top->next;
  free(top);
  return x;
}

// 读取栈顶元素
ElemType peek(Stack s) {
  return s->data; 
}

// 销毁栈
void destroyStack(Stack* s) {
  Node* cur = *s;
  while (cur) {
    Node* next = cur->next;
    free(cur);
    cur = next;
  }
  *s = NULL;
}
```

## 总结

链式栈利用链表提供了一种灵活动态的栈结构,只需要理解其链表的存储方式,就可以轻松实现各种栈操作。上述C语言实现给出了链式栈的基本框架,可以根据需要进行扩展和优化。栈这种基础数据结构在许多场景下都大显身手,比如递归编程、表达式求值等,非常值得我们掌握。