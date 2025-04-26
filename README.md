1. 数据结构定义
typedef struct Node {
    int data;
    struct Node* next;
} Node;
定义了一个链表节点结构 Node，包含：

data：存储整型数据。

next：指向下一个节点的指针。

2. 核心功能函数
(1) 创建新节点 (createNode)
c
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        fprintf(stderr, "内存分配失败\n");
        exit(EXIT_FAILURE);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}
功能：动态分配内存创建一个新节点，并初始化数据和指针。

注意：如果内存分配失败，会打印错误信息并终止程序（exit(EXIT_FAILURE)）。

(2) 在链表头部插入节点 (insertAtHead)
c
void insertAtHead(Node** head, int data) {
    Node* newNode = createNode(data);
    newNode->next = *head;
    *head = newNode;
}
功能：将新节点插入链表头部，成为新的头节点。

步骤：

创建新节点。

新节点的 next 指向原头节点。

更新头指针 head 指向新节点。

(3) 在链表尾部插入节点 (insertAtTail)
c
void insertAtTail(Node** head, int data) {
    Node* newNode = createNode(data);
    if (*head == NULL) {
        *head = newNode;
        return;
    }
    Node* current = *head;
    while (current->next != NULL) {
        current = current->next;
    }
    current->next = newNode;
}
功能：将新节点插入链表尾部。

步骤：

如果链表为空，直接作为头节点。

否则遍历到链表末尾，将最后一个节点的 next 指向新节点。

(4) 删除节点 (deleteNode)
c
void deleteNode(Node** head, int data) {
    if (*head == NULL) return;
    
    // 删除头节点
    if ((*head)->data == data) {
        Node* temp = *head;
        *head = (*head)->next;
        free(temp);
        return;
    }
    
    // 查找待删除节点的前驱
    Node* current = *head;
    while (current->next != NULL && current->next->data != data) {
        current = current->next;
    }
    
    // 找到后删除
    if (current->next != NULL) {
        Node* temp = current->next;
        current->next = current->next->next;
        free(temp);
    }
}
功能：删除链表中第一个匹配 data 的节点。

步骤：

如果头节点匹配，直接删除并更新头指针。

否则遍历链表，找到目标节点的前驱，调整指针关系并释放内存。

未找到时不作操作。

(5) 打印链表 (printList)
c
void printList(Node* head) {
    Node* current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}
功能：按顺序打印链表内容，格式为 数据 -> ... -> NULL。

(6) 释放链表内存 (freeList)
c
void freeList(Node* head) {
    Node* current = head;
    while (current != NULL) {
        Node* temp = current;
        current = current->next;
        free(temp);
    }
}
功能：释放链表中所有节点的内存，避免内存泄漏。

3. 主函数测试 (main)
c
int main() {
    Node* head = NULL;
    
    // 测试插入
    insertAtHead(&head, 10);  // 链表: 10 -> NULL
    insertAtTail(&head, 20);  // 链表: 10 -> 20 -> NULL
    insertAtHead(&head, 5);   // 链表: 5 -> 10 -> 20 -> NULL
    insertAtTail(&head, 30);  // 链表: 5 -> 10 -> 20 -> 30 -> NULL
    
    printf("初始链表: ");
    printList(head);  // 输出: 5 -> 10 -> 20 -> 30 -> NULL
    
    // 测试删除
    deleteNode(&head, 20);    // 删除中间节点，链表: 5 -> 10 -> 30 -> NULL
    printf("删除20后的链表: ");
    printList(head);
    
    deleteNode(&head, 5);     // 删除头节点，链表: 10 -> 30 -> NULL
    printf("删除5后的链表: ");
    printList(head);
    
    deleteNode(&head, 99);    // 删除不存在的节点，链表不变
    printf("尝试删除99后的链表: ");
    printList(head);
    
    freeList(head);  // 释放内存
    
    return 0;
}
功能：演示链表的插入、删除、打印和内存释放操作。
