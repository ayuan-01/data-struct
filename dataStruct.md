# 链表

链表的一些基础操作

1. 链表定义

```c
struct Node {
    int data;
    struct Node* next;
}
```

2. 创建节点

```c
struct Node* createNode(int value) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if(newNode == NULL)
        printf("malloc failed!");
    newNode->data = value;
    newNode->next = NULL;
    return newNode;
}
```

3. 头插法插入节点

```c
struct Node* insertAtHead(struct Node* head, int value) {
    struct Node* newNode = createNode(value);
    newNode->next = head;
    return newNode;  // 新的头结点
}
```

4. 尾插法插入节点

```c
struct Node* insertAtTail(struct Node* head, int value) {
    struct Node* newNode = createNode(value);
    if(head == NULL) 
        return newNode;
    struct Node* temp = head;
    while(temp->next != NULL) {
        temp = temp->text;
    }
    temp->next = newNode;
    return head;
}
```

5. 遍历链表

```c
void printList(struct Node* head) {
    struct Node* temp = head;
    while (temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}
```

### 1.  实现链表的逆置

```c
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL || head->next == NULL) return NULL;
    struct LiseNode* former;
    struct ListNode* latter;
    struct ListNode* mid = head;
    while (mid != NULL) {
        latter = mid->next;
        mid->next = former;
        former = mid;
        mid = latter;                        
    }
}
```



### 2. 判断单链表中是否存在环

```c
struct ListNode* detectCycle(struct ListNode* head) {
    struct ListNode* fast = head;
    struct ListNode* slow = head;
    
    while(fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast) {
            fast = head;
            while(fast != slow) {
                fast = fast->next;
                slow = slow->next;
        	}
            return slow;
        }
    }
    return NULL;
}
```



### 3. 单链表相交，如何求交点

```c
struct ListNode* getIntersectionNode(struct ListNode* headA, struct ListNode* headB) {
    struct ListNode* p = headA;
    struct ListNode* q = headB;
    
    while(q != p) {
        if (p == NULL)
            p = headB;
        else
            p = p->next;
        if (q == NULL)
            q = headA;
        else 
            q = q->next
    }
    return q;
}
```



### 4. 求有环链表第一个入环点

```c
struct ListNode* detectCycle(struct ListNode* head) {
    struct ListNode* fast = head;
    struct ListNode* slow = head;
    
    while(fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast) {
            fast = head;
            while(fast != slow) {
                fast = fast->next;
                slow = slow->next;
        	}
            return slow;
        }
    }
    return MULL;
}
```



### 5. 写出链表的删除一个节点的程序

```c
void deleteNode(Node** headRef, int key) {
    Node* temp = *headRef;
    Node* prev = NULL;
	if (temp == NULL) return;
    if (temp->data == key) {
        *headRef = temp->next; // 头指针后移
        free(temp);            // 释放原头节点
        return;
    }

    while (temp != NULL && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }
    if (temp == NULL) return;
    prev->next = temp->next;
    free(temp); 
}
```



### *6. 用递归算法实现两个有序链表的合并

```c
struct ListNode* Merge(struct ListNode* pHead1, struct ListNode* pHead2 ) {
    // write code here
    if (pHead1 == NULL) return pHead2;
    if (pHead2 == NULL) return pHead1;
    
    if (pHead1->val > pHead2->val) {
        pHead2->next = Merge(pHead1, pHead2->next);
        return pHead2;
    } else {
        pHead1->next = Merge(pHead1->next, pHead2);
        return pHead1;
    }
}
```

# 二叉树

满二叉树：除了最后一层，所有的节点都是满的，并且所有节点都有两个子节点

完全二叉树：最后一层可以是单节点

二叉搜索数：BST，左子树中的**所有节点值**都**小于该节点值**。右子树中的**所有节点值**都**大于该节点值**。

平衡二叉树：AVL，左右子节点的高度不超过1的BST。

```c
// 定义二叉树节点结构
typedef struct Node {
    int data;               // 存储节点的数据
    struct Node* left;      // 指向左子节点
    struct Node* right;     // 指向右子节点
} Node;

// 创建新节点
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("内存分配失败！\n");
        exit(1);
    }
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// 销毁二叉树
void destroyTree(Node* root) {
    if (root == NULL) return;
    destroyTree(root->left);   // 递归释放左子树
    destroyTree(root->right);  // 递归释放右子树
    free(root);                // 释放当前节点
}

// 二叉树深度
int maxDepth(struct TreeNode* root){
    if (root == NULL) return 0;
    int lenLeft = maxDepth(root->left);
    int lenRight = maxDepth(root->right);
    return lenLeft > lenRight ? lenLeft + 1 : lenRight + 1;
}
```

### 遍历

```c
// 中序遍历：创建一个数组，数组作为传入参数保存遍历的结果
/**
 * returnNum: 保存遍历出的元素
 * returnSize: 保存二叉树的大小
 */
void inTra(struct TreeNode* root, int *returnNum, int* returnSize)
{
    if(root==NULL) return;
    inTra(root->left, returnNum, returnSize);
    returnNum[(*returnSize)++] = root->val;
    inTra(root->right, returnNum, returnSize);
}

int* inorderTraversal(struct TreeNode* root, int* returnSize) {
    int *returnNum = (int *)malloc(sizeof(int)*101);
    *returnSize = 0;
    if(root==NULL) return NULL;
    inTra(root,returnNum,returnSize);
    return returnNum;
}
```



```c
// 前序遍历：根 -> 左 -> 右
void preorderTraversal(Node* root) {
    if (root == NULL) return;
    printf("%d ", root->data);  // 访问根节点
    preorderTraversal(root->left);  // 遍历左子树
    preorderTraversal(root->right); // 遍历右子树
}

// 中序遍历：左 -> 根 -> 右
void inorderTraversal(Node* root) {
    if (root == NULL) return;
    inorderTraversal(root->left);   // 遍历左子树
    printf("%d ", root->data);     // 访问根节点
    inorderTraversal(root->right);  // 遍历右子树
}


// 后序遍历：左 -> 右 -> 根
void postorderTraversal(Node* root) {
    if (root == NULL) return;
    postorderTraversal(root->left);  // 遍历左子树
    postorderTraversal(root->right); // 遍历右子树
    printf("%d ", root->data);      // 访问根节点
}
```

### 深度

```c
int maxDepth(struct TreeNode* root) {
    // 递归结束条件
    if (root == NULL) return 0;

    int l_depth = maxDepth(root->left);
    int r_depth = maxDepth(root->right);

    return (l_depth > r_depth ? l_depth : r_depth) + 1;
}
```

### 是否平衡二叉树

```c
bool isAVL(struct TreeNode* root)
{
	struct TreeNode* left  = root->left;
    struct TreeNode* right = root->right;
    
    int l_dep = maxDepth(left);
    int r_dep = maxDepth(right);
    
    if (l_dep == r_dep) return true;
    else if (l_dep > r_dep) return ((l_dep-r_dep) < 1 ? true : false);
    else return ((r_dep-l_dep) < 1 ? true : false);
}
```

# 排序查找算法及其改进

我想对于每一个经历过秋招的小伙伴们来说，十大排序基本都被问过（**快速排序**、**归并排序**、堆排序、冒泡排序、插入排序、选择排序、希尔排序、桶排序、基数排序）。

| 排序名称 | 最好时间复杂度 | 平均时间复杂度 | 最坏时间复杂度 | 空间复杂度 |
| -------- | -------------- | -------------- | -------------- | ---------- |
| 快排     | $O(nlogn)$     | $O(nlogn)$     | $O(n^2)$       | $O(logn)$  |
| 归并     | $O(nlogn)$     | $O(nlogn)$     | $O(n)$         | $O(n)$     |
| 插入     | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$     |
| 冒泡     | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$     |

### *快速排序

快速排序

```c
void swapval(int* a, int* b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}

void QuickSort (struct ListNode* pBegin, struct ListNode* pEnd) {
    if (pBegin == NULL || pEnd == NULL || pBegin == pEnd)
        return;
    
    struct ListNode* pL = pBegin;
    struct ListNode* pR = pBegin->next;
    int pivot = pL->val;
    // pL是小于pivot的最后一个元素
    while (pR != pEnd->next) {
        if (pR->val <= pivot) {
            pL = pL->next;	// 有元素小于pivot，pL右移后放到pL的位置
            if (pL != pR)
            	swapval(&pL->val, &pR->val);            
        }
        pR = pR->next;
    }
    // 把pivot放到中间
    swapval(&pBegin->val, &pL->val);
 	QuickSort(pBegin, pL);
 	QuickSort(pL->next, pEnd);
}

struct ListNode* sortList(struct ListNode* head) {
    if (head == NULL || head->next == NULL) {  
        return head;
    }
    struct ListNode* pTemp = head;
    struct ListNode* pStart = head;

    while (pTemp->next != NULL) {
        pTemp = pTemp->next;
    }
    pEnd = pTemp;
    
    QuickSort(pStart, pEnd);
    return head;
}
```

```c
void quickSort(int *a, int left, int right)
{
    if (left >= right) return;
    int i = left - 1;
    int pivot = a[right];
    
    for (int j = left; j < right; j++) {
        // 如果不大于标准，换到左边来
        if (a[j] <= pivot) {
            i++;
            swap(&a[j], &a[i]);
        }
    }
    
    // i的位置时小于pivot的最后一个数，所以pivot的索引应该是i+1
    int pivotIndex = i + 1;
    swap(&a[pivotIndex], &a[right]);
    quickSort(a, left, pivotIndex-1);
    quickSort(a, pivotIndex+1, right);
}
```



### *冒泡排序

```c
// 双重循环，每次循环次数减1.长度为5的数组，第一次循环对比4次
void bubble_sort(int *arr, int n) {
    int flag;
    for (int i = 0; i < n-1; i++) {
        flag = 0;
        for (int j = 0; j < n-1-i; j++) {
            if (arr[j] > arr[j+1]) {
                swap(&arr[j], &arr[j+1]);
                flag = 1;
            }
        }
        if (flag == 0) return;
    }
}
```



### 归并排序

```c
/*
 * 归并排序的思想：
 * 1. merge函数，传入一个以m为界限的左右分别排序好的数组，然后把这个数组合并
 * 1.1 首先malloc两个数组L，R然后填充，然后使用? :填充arr[k++]
 * 2. mergeSort函数递归执行，停止条件l>=r;
 */
void merge(int arr[], int l, int m, int r)
{
    int n1 = m - l + 1;
    int n2 = r - m;
    int *L;
    int *R;
    
    L = (int *)malloc(n1 * sizeof(int));
    R = (int *)malloc(n2 * sizeof(int));
    
    for(int i = 0; i < n1; i++) L[i] = arr[l + i];
    for(int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];
    
    int i, j, k; 
    i = j = 0;
    k = l;
    
    while(i < n1 && j < n2)
        arr[k++] = L[i] < R[j] ? L[i++] : R[j++];
    
    while(i < n1) arr[k++] = L[i++];
    while(j < n2) arr[k++] = R[j++];
    
    free(L);
    free(R);
}

void mergeSort(int arr[], int l, int r)
{
    if (l > r) return;
    int m = (l + r)/2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```



### 堆排序

### *插入排序

```c
// 从第二个数开始往前插入数，前面部分是排序好的，假如有n=5个数据，第一次插入a[1]，第4次插入a[4]。
// 所以外循环n-1次，从index=1开始。从j = i - 1依次向左比较，直到比较到最低位或者小于key的数。
// 两个关键点，i是需要插入的索引key=arr[i]，i左边也就是j=i-1是排序好的节点，然后向左比对，当arr[j]<key时，arr[j+1]=key。
void insert_sort(int *arr, int n) {
    for (int i = 1; i < n, i++) {
        int key = arr[i];
        int j = i - 1;
        while(j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}
```

### 选择排序

# 数组

### 二分查找法

```c
/**
 * nums     : 输入的数组
 * numsSize : 数组的大小
 * target   : 目标值
 * return   : 返回索引
 */
int searchInsert(int* nums, int numsSize, int target) {
    int left = 0;
    int right = numsSize - 1;
    int mid;

    while (left <= right) {
        mid = left + (right - left) / 2;;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return left;
}
```

# 字符串

### 字符串复制

```c
void mucpy(char *s1, char *s2) {
    while(*s1++ = *s2++);
}
```

### 字符串翻转

```c
void swap(char* str, int start, int end) {
    char temp = 0;
    while (start <= end) {
        temp = str[start];
        str[start] = str[end];
        str[end] = temp;
        end--;
        start++;
    }
}

void reverseStr(char* str, int n) {
    int start = 0;
    int end   = n - 1;

    swap(str, start, end);
}
```

### 单词翻转

```c
#include <stdio.h>
#include <string.h>
 
// 反转字符串的某一部分 [start, end]
void reverse(char* s, int start, int end) {
    while (start < end) {
        char temp = s[start];
        s[start] = s[end];
        s[end] = temp;
        start++;
        end--;
    }
}
 
// 反转字符串中的单词顺序
void reverseWords(char* s) {
    int len = strlen(s);
    if (len <= 1) return;
 
    // 1. 反转整个字符串
    reverse(s, 0, len - 1);
 
    // 2. 逐个反转每个单词
    int wordStart = 0;
    for (int i = 0; i <= len; i++) {
        if (s[i] == ' ' || s[i] == '\0') {
            reverse(s, wordStart, i - 1);
            wordStart = i + 1;
        }
    }
}
 
int main() {
    char str[] = "Hello world";
    printf("Original: \"%s\"\n", str);
 
    reverseWords(str);
    printf("Reversed: \"%s\"\n", str);
 
    return 0;
}
```

# main

