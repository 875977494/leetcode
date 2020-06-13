
<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=true} -->
<!-- code_chunk_output -->
<dir align=center>LeetCode题解整合</dir>

[Toc]

## 序
开个坑做一些自己写的题解的整合，慢慢搞。


## 链表

 <a name="旋转链表"></a>

### 61.旋转链表
[题目链接](https://leetcode-cn.com/problems/rotate-list/)

**题目分析**
其实题干描述已经为此题降低了很多难度，即旋转链表可以等效为链表整体向右移动。

首先考虑向右移动的次数`k`，考虑`k`的值可能超过链表总长度，因此需要遍历一遍链表来获取链表长度`num`。
```c
int num = 0;
while (head != NULL){
    num++;
    head = head->next;
}
k = k % num;
```
借助示例，我们分析一下
示例`1->2->3->4->5->NULL, k = 2`，我们可以将整个链表变成环形链表，现在我们实际上只需要让原链表向右移动3位，移动到4，再让结点3指向空指针，使环形链表断开，就可以获得旋转后的链表`4->5->1->2->3->NULL`。

为了完成上述操作，我们需要使用***双指针法***；还需要知道右移位数。参考示例，不难知道，右移位数实际上就是`num - k`:
```c
k = num - (k % num);
```

实践中，成环的操作可以和计算链表长度的操作合并，降低运行时间：
```c
list temp = head;
int num = 1;
while (temp->next != NULL){
    num++;
    temp = temp->next;
}
temp_next = head;

```
**题目解答**
```C++
struct ListNode* rotateRight(struct ListNode* head, int k){
    if (head == NULL) return NULL;
    if (head->next == NULL) return head;

    // 成环，顺便计算元素个数
    int num = 1;
    list temp = head;
    while (temp->next != NULL){
        temp = temp->next;
        num++;
    }
    temp->next = head;
    k = num - (k % num);
    
    list p1, p2;
    p1 = head;
    p2 = head;

    for (int i = 0; i < k; i++){
        p1 = p1->next;
        p2 = p2->next;
    }
    for (int i = 1; i < num; i++){
        p2 = p2->next;
    }
    p2->next = NULL;
    
    return p1;
}
```

### 141.环形链表
[题目链接](https://leetcode-cn.com/problems/linked-list-cycle/)

**题目分析**
第一时间想到使用快慢指针，设置指针`p1`,每次迭代向前移动一位；指针`p2`,每次迭代向前移动两位。

两个指针若有一个为空，则表示该链表无环，终止迭代，输出`flase`；

两个指针若在迭代过程中相等，则表示有环，终止迭代，输出`true`。

**题目解答**
```c++
typedef struct ListNode* list;
bool hasCycle(struct ListNode *head) {
    if (head == NULL || head->next == NULL) return false;
    list p1, p2;
    p1 = head;
    p2 = head;
    // 设置标识，保证第一次迭代时不比较p1和p2
    int flag = 0;

    while (p1 != NULL && p2 != NULL){
        if (flag == 0) flag = 1;
        else {
            if (p1 == p2) return true;
        }
        // 若p1或p2的下一位出现空指针，则说明无环
        if (p1->next == NULL || p2->next == NULL) return false;
        else{
            p1 = p1->next;
            p2 = p2->next->next;
        }
    }
    return false;
}
```

### 237.删除链表中的结点
[题目链接](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

**题目分析**
这道题的难点其实是读题。

其实我们的初始结点，就是需要被删除的结点。因此我们只需要将下一个结点的值赋给当前结点，删除下一个结点即可。（因为要被删除的结点不是必然不是末尾结点）
**题目解答**
```c++
void deleteNode(struct ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```

## 数组

## 双指针

## 哈希表

## 回溯算法

## 树与二叉树


