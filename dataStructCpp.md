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

完全二叉树：最后一层可以是单节点，所有节点连续几种在左边。

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
| 归并     | $O(nlogn)$     | $O(nlogn)$     | $O(nlogn)$     | $O(n)$     |
| 插入     | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$     |
| 冒泡     | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$     |
| 堆排     | $O(nlogn)$     | $O(nlogn)$     | $O(nlogn)$     | $O(1)$     |
| 选择     | $O(n^2)$       | $O(n^2)$       | $O(n^2)$       | $O(1)$     |

### *快速排序

```c
class solution {
public:
    void quickSort(vector<int>& arr, int left, int right) {
        if (left >= right) return;

        int pivot = arr[right];
        int l = left - 1;
        int r = left;

        for (; r < right; ++r) {
            if (arr[r] < pivot) {
                l++;
                swap(arr[l], arr[r]);
            }
        }

        swap(arr[l+1], arr[right]);

        int povitIndex = l + 1;
        quickSort(arr, left, povitIndex - 1);
        quickSort(arr, povitIndex + 1, right);
    }
};
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
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    vector<int> L(arr.begin() + left, arr.begin() + left + n1); // 或 mid + 1
    vector<int> R(arr.begin() + mid + 1, arr.begin() + mid + 1 + n2); // 或 right + 1

    int i = 0, j = 0;
    int k = left;

    while (i < n1 && j < n2) arr[k++] = L[i] < R[j] ? L[i++] : R[j++];
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left)/2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid+1, right);
    merge(arr, left, mid, right);
}
```

### 堆排序

堆的数组表示：

对于数组中一个位置为i的节点（下标从0开始），

1. 父节点：`(i-1)/2`
2. 左子节点`2i+1`
3. 右子节点`2i+2`

最后一个非叶子节点的下标是`n/2 - 1`。n是数组长度。

最大堆：每个父节点大于子节点。

最小堆：每个子节点大于父节点。

#### 构建最大堆，排序

```cpp
/**
 * @brief 堆化函数
 * 
 * 这是堆排序的核心。它将以 i 为根的子树调整为大顶堆。
 * 假设 i 的左右子树已经是大顶堆。
 * 
 * @param arr  待排序的数组
 * @param n    数组的大小，也是堆中元素的总数
 * @param i    当前需要堆化的子树的根节点索引
 */
void heapify(std::vector<int>& arr, int n, int i) {
    int largest = i;    
    int left = 2 * i + 1; 
    int right = 2 * i + 2; 

    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;
    if (largest != i) {
        std::swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void maxHeap(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
}

void heapSort(std::vector<int>& arr) {
    int n = arr.size();
    maxHeap(arr);
    for (int i = n - 1; i > 0; i--) {
        std::swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

#### 使用priority_heap容器适配器实现的堆排列

```cpp
void heapSortUsingPriorityQueue(std::vector<int>& arr) {
    if (arr.empty()) {
        return;
    }

    // 1. 创建一个最大堆（默认行为）
    // 将数组中的所有元素放入优先队列
    std::priority_queue<int> max_heap;
    for (int val : arr) {
        max_heap.push(val);
    }

    // 2. 依次取出最大元素，并存回原数组
    // 此时元素是按从大到小的顺序被取出的
    int index = 0;
    while (!max_heap.empty()) {
        arr[index] = max_heap.top(); // 获取最大值
        max_heap.pop();              // 从堆中移除
        index++;
    }

    // 3. 因为得到的是降序序列，需要反转数组才能得到升序结果
    std::reverse(arr.begin(), arr.end());
}
```

### *插入排序

```c
// 从第二个数开始往前插入数，前面部分是排序好的，假如有n=5个数据，第一次插入a[1]，第4次插入a[4]。
// 所以外循环n-1次，从index=1开始。从j = i - 1依次向左比较，直到比较到最低位或者小于key的数。
// 两个关键点，i是需要插入的索引key=arr[i]，i左边也就是j=i-1是排序好的节点，然后向左比对，当arr[j]<key时，arr[j+1]=key。
void insertSort(vector<int>& arr) {
    for (int i = 0; i < arr.size(); ++i){
        // i=2插入到前面排序好的数组，跟前面的一一比较
        int key = arr[i];
        int j = i-1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

### 选择排序

```cpp
// 选择排序函数（升序）
// 遍历数组，第一遍找出最小的放到第一个位置，第二遍从[1]开始，找出最小的，以此类推
void selectionSort(std::vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n - 1; ++i) {
        int minIndex = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIndex]) minIndex = j;
        }

        if (minIndex != i) swap(arr[i], arr[minIndex]);
    }
}
```

# 数组

### 二分查找法

```cpp
#include <iostream>
#include <string>
#include <vector>

class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;

        while (left <= right) {
            int mid = left + (right - left)/2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) left = mid + 1;
            else right = mid - 1;
        }
        return left;
    }
};

int main(void)
{
	Solution sol;
    std::vector<int> v_nums = {1, 3, 5, 6};
    
    int target = 5;
    
    std::cout << "返回索引：" << sol.searchInsert(v_nums, target) << std::endl;
    return 0;
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
//C++
std::reverse(s.begin(), s.end());

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

```cpp
// 字符串反转
// 1. 使用算法实现 #include <algorithm>
std::string s = "hello";
std::reverse(s.begin(), s.end());
// 2. 手动实现
while (left < right) {
    std::swap(s[left++], s[right--]);
}

// 单词反转
std::string reverseWords(std::string s) {
    // 1. 分裂单词，存入字符串数组
    std::stringstream ss; ss.str(s);
    std::vector<std::string> words;
    std::string word;
    while (ss >> word) {
        words.push_back(word);
    }
    
    // 2. 反转数组
    std::reverse(words.begin(), words.end());
    
    // 2. 合并单词
    std::string result;
    for (int i = 0; i < words.size(); ++i) {
        if (i != 0) result += " ";
        result += words[i];
    }
    
    return result;
}

```



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

# 图

图的几个概念：

节点（顶点，vertex），边（edge），有向图/无向图（单向边，双向边），带权图（边带有数值（权重），比如两个城市之间的举例，两个任务之间的时间成本）

图在算法中的经典应用：最短路径问题，搜索问题（BFS，DFS）。

想象你在一个迷宫里，要从起点走到终点，迷宫有很多岔路口，每个岔路口又有多个分支。你可以用两种策略来探索迷宫：深度优先搜索（DFS）：一条路走到黑，选一条路一直走下去，直到走不通了，再返回上一个岔路口，换另一条路继续，使用栈或者递归。广度优先搜索（BFS）：层层推进，先走一步看看所有可能，再走第二步……像水波一样一圈一圈扩散，用队列实现，适合找最短路径。

假设一个无向图

```
0 -- 1 -- 3
|    |
2 -- 4
```

用邻接表表示为：

```
0 -> 1, 2
1 -> 0, 3, 4
2 -> 0, 4
3 -> 1
4 -> 1, 2
```

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
using namespace std;

// 图的邻接表表示
vector<vector<int>> graph = {
    {1, 2},     // 0
    {0, 3, 4},  // 1
    {0, 4},     // 2
    {1},        // 3
    {1, 2}      // 4
};

vector<bool> visited(5, false); // 记录节点是否被访问过

// 深度优先搜索（DFS）- 递归实现
void dfs(int node) {
    visited[node] = true;
    cout << node << " ";

    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor);
        }
    }
}

// 深度优先搜索（DFS）- 非递归（栈）实现
void dfs_stack(int start) {
    fill(visited.begin(), visited.end(), false); // 重置访问状态
    stack<int> s;
    s.push(start);
    visited[start] = true;

    while (!s.empty()) {
        int node = s.top();
        s.pop();
        cout << node << " ";

        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                s.push(neighbor);
            }
        }
    }
}

// 广度优先搜索（BFS）- 队列实现
void bfs(int start) {
    fill(visited.begin(), visited.end(), false); // 重置访问状态
    queue<int> q;
    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << node << " ";

        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}

int main() {
    cout << "DFS (递归): ";
    dfs(0);
    cout << endl;

    cout << "DFS (栈): ";
    dfs_stack(0);
    cout << endl;

    cout << "BFS (队列): ";
    bfs(0);
    cout << endl;

    return 0;
}
```

# 嵌入式常考的函数实现

## memcpy

```c
void* my_memcpy(const void* dest, const void* src, int n) {
    char* d = (char*)dest;
    char* s = (char*)src;
    
    // 判断重叠，s在前从后往前拷贝，s在后从前往后拷贝
    if (s < d && s + n < d) {
        for (int i = n - 1; i >= 0; ++i) {
            d[i] = s[i];
        }
    } else {
        for (int i = 0; i < n; ++i) {
            d[i] = s[i];
        }
    }
    
    return dest;
}
```

## strcpy

```c
char *my_strcpy(char *dest, char *src) {
    if (dest == NULL || src == NULL) return NULL;
    char *ret = dest;
    while ((*dest++ = *src++) != '\0');
    return ret;
}
```

## strncpy

```c
#include <stddef.h>
char *my_strncpy(char *dest, char *src, size_t n) {
    if (dest == NULL || src == NULL) return NULL;
    
    char *ret = dest;
    while (n && (*dest++ = *src++) != '\0') n--;
    
    // n有剩余
    if (n != 0) {
        while (n--) *dest++ = '\0';
    }
    
    return dest;
}
```

## strlen

```c
size_t my_strlen(const char *str) {
    if (str == NULL) return 0;
    size_t len = 0;    
    while (*str++ != '\0') len++;
    return len;
}
```

## strcmp

```c
int my_strcmp(const char *s1, const char *s2) {
    if (s1 == NULL || s2 == NULL) return -1;
    while (*s1 && (*s1 == *s2)) {
        s1++;
        s2++;
    }
    return *(unsigned char *)s1 - *(unsigned char *)s2
}
```

