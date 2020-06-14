
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

### 1470. 重新排列数组
[题目链接](https://leetcode-cn.com/problems/shuffle-the-array/)

**题目分析**
思路还是比较清晰的，设置两个指针`i`和`j`分别指向数组第`1`个元素和第`n+1`个元素。然后设置一个空数组，将两个元素加入空数组中。

遍历过程：
```c++
while (j < nums.size()) {
    res[count++] = nums[i++];
    res[count++] = nums[j++];
}
```
**复杂度分析**
时间复杂度`O(N)`,空间复杂度`O(N)`。

**题目解答**
```c++
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        vector<int> res(nums.size(), 0);
        int i = 0;
        int j = n;
        int count = 0;
        while (j < nums.size()) {
            res[count++] = nums[i++];
            res[count++] = nums[j++];
        }
        return res;
    }
};
```


## 双指针

### 15.三数之和(未完成)
[题目链接](https://leetcode-cn.com/problems/3sum/)

**题目分析**


**题目解答**
```c++
class Solution {
private:
    vector<vector<int>> res;
    vector<int> temp;
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int left = 0, right = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) return res;
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            left = i + 1;
            right = nums.size() - 1;
            while (left < right) {
                int t = nums[i] + nums[left] + nums[right];
                if (t == 0) {
                    temp.push_back(nums[i]);
                    temp.push_back(nums[left]);
                    temp.push_back(nums[right]);
                    res.push_back(temp);
                    temp.clear();
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                }else if (t < 0) {
                    left++;
                }else right--;
            }
        }
        return res;
    }
};
```

## 二分法
### 1300. 转变数组后最接近目标值的数组和（未完成）
[题目链接](https://leetcode-cn.com/problems/sum-of-mutated-array-closest-to-target/)

**题目分析**
显然需要先将原数组排序，排序后计算数组和`sum`，如果`sum <= target`，那么直接返回`arr.back()`；同样地，计算`arr[0] * arr.size()`，如果`arr[0] * arr.size() >= target`，那么直接返回`arr[0]`。
```c++
sort(arr.begin(), arr.end());
int sum = 0;
for (int a : arr) sum += a;
if (sum <= target) return arr.back();
if (arr[0] * arr.size() >= target) return arr[0];
```



**题目解答**

## 哈希表
### 1.两数之和(未完成)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        unordered_map<int, int> m;

        for (int i = 0; i < nums.size(); i++) {
            if (m.count(target - nums[i])) {
                res.push_back(i);
                res.push_back(m[target - nums[i]]);
                return res;
            }
            m[nums[i]] = i;
        }
        return res;
    }
};
```
## 回溯算法

## 树与二叉树

### 108.将有序数组转换为二叉搜索树

**题目分析**
使用双指针法，确定数组的中值，递归的插入树中。
```c++
TreeNode*  creTre(vector<int> nums, int left, int right){
    if (left > right) return NULL;
    
    int mid = (left + right) / 2;
    TreeNode* root = new TreeNode(nums[mid]);
    root->left = creTre(nums, left, mid - 1);
    root->right = creTre(nums, mid + 1, right);
    
    return root;
}
```
**题目解答**
```c++
class Solution {
public:
    TreeNode* creTre(vector<int> nums, int left, int right){
        if (left > right) return NULL;
        int mid = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = creTre(nums, left, mid - 1); 
        root->right = creTre(nums, mid + 1, right);

        return root;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return creTre(nums, 0, nums.size() - 1);
    }
};
```

## 字符串

### 28.实现 strStr() (未完成)
**题目分析**
实际上就是实现一下kmp算法。

**题目解答**
```c++

```

