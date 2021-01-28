

## 链表

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

### 面试题 02.01. 移除重复节点
[题目链接](https://leetcode-cn.com/problems/remove-duplicate-node-lcci/)
**题目分析**
采用哈希表，遍历链表，储存第一出现的元素，如果遇到重复元素，则删除该结点。这样做可以保留第一个结点。

```c++
unordered_set<int> s;
s.insert(head->val);
ListNode* p = head;
while (p->next) {
    ListNode* temp = p->next;
    if (s.count(temp->val)) {
        p->next = p->next->next;
    }else {
        s.insert(temp->val);
        p = p->next;
    }
}
p->next = NULL;
```
最后返回`head`即可。

**题目解答**
```c
class Solution {
public:
    ListNode* removeDuplicateNodes(ListNode* head) {
        if (!head || !head->next) return head;

        unordered_set<int> s;
        s.insert(head->val);
        ListNode* temp = head;

        while (temp->next) {
            ListNode* cur = temp->next;
            if (s.count(cur->val)) {
                temp->next = temp->next->next;
            }else {
                s.insert(cur->val);
                temp = temp->next;
            }
        }
        temp->next = NULL;
        return head;
    }
};
```
## 数组
### 1014. 最佳观光组合
[题目链接](https://leetcode-cn.com/problems/best-sightseeing-pair/)

**题目分析**
题目给定的观光景点评分公式是`dp = A[i] + A[j] + i - j`，变化一下可以得到`A[i] + i`和`A[j] - j`。

我们可以发现，在遍历的过程中，只要固定了`i`和`j`，前后两个式子就不会变。所以我们可以遍历原数组，在遍历的过程中不断更新`A[i] + i`的最大值以及`dp`。

遍历过程如下：
```c++
for (int i = 1; i < A.size(); i++) {
    res = res > temp + A[j] - j ? res : temp + A[j] - j;
    temp = temp > A[j] + j ? temp : A[j] + j;
}
```
最后返回`res`即可。

**题目解答**
```c++
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& A) {
        int res = INT_MIN;
        int temp = A[0] + 0;
        for (int j = 1; j < A.size(); j++) {
            res = res > temp + A[j] - j ? res : temp + A[j] - j;
            temp = temp > A[j] + j ? temp : A[j] + j;
        }
        return res;
    }
};
```

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
### 16.最接近的三数之和
[题目链接](https://leetcode-cn.com/problems/3sum-closest/)
**题目分析**
此题的思路与[15.三数之和](https://leetcode-cn.com/problems/3sum/)有些相似。都是固定一个点，剩余两个点使用双指针查找，为了使用双指针，首先要对原数组进行排序。

同时，为了方便运算，如果原数字有且仅有3个数字，直接返回其和。
```c++
if (nums.size() == 3) return nums[0] + nums[1] + nums[2];
sort(nums.begin(), nums.end());
```

然后就是教科书式的双指针遍历了，遍历起始值`i`，左指针`left = i + 1`，右指针`right = nums.size() - 1`，计算`temp = nums[i] + nums[left] + nums[right]`：
- 若`temp == target`，直接返回`target`；
- 若`temp < target`：
    - 计算`temp`与`target`差值，如果小于当前差值，则更新差值与`res`，左指针加一`left++`;
- 若`temp > target`：
    - 计算`temp`与`target`差值，如果小于当前差值，则更新差值与`res`，右指针减一`right--`;

最后返回`res`。

遍历过程：
```c++
int del = INT_MAX;
int res;
for (int i = 0; i < nums.size() - 2; i++) {
    int l = i + 1;
    int r = nums.size() - 1;
    int temp = nums[i] + nums[l] + nums[r];
    if (temp == target) return target;
    else if (temp < target) {
        if (abs(temp - target) < del) {
            del = abs(temp - target);
            res = temp;
        }
        l++;
    }else {
        if (abs(temp - target) < del) {
            del = abs(temp - target);
            res = temp;
        }
        r--;
    }
}
```
**题目解答**
```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() == 3) return nums[0] + nums[1] + nums[2];
        sort(nums.begin(), nums.end());
        
        int del = INT_MAX;
        int res;
        for (int i = 0; i < nums.size() - 2; i++) {
            int l = i + 1;
            int r = nums.size() - 1;
            while (l < r) {
                int temp = nums[i] + nums[l] + nums[r];
                if (temp == target) {
                    return target;
                }else if (temp < target) {
                    if (abs(temp - target) < del) {
                        del = abs(temp - target);
                        res = temp;
                    }
                    l++;
                }else if (temp > target) {
                    if (abs(temp - target) < del) {
                        del = abs(temp - target);
                        res = temp;
                    }
                    r--;
                }
            }
        }
        return res;
    }
};
```

### 209.长度最小的子数组(未完成)
[题目链接](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

**题目分析**

**题目解答**
```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        if (nums.size() == 0) return 0;
        int start = 0;
        int end = 0;

        int res = INT_MAX;
        int sum = 0;
        while (end < nums.size()) {
            sum += nums[end];
            while (sum >= s) {
                res = min(end - start + 1, res);
                sum -= nums[start];
                start++;
            }
            end++;
        }

        return res == INT_MAX ? 0 : res;
    }
};
```

## 二分法
### 374.猜数字大小
[题目链接](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

**题目分析**
这是一到典型的二分查找题目。题目里有个坑，题目说的我的数字比较大，是指题目给定的数字。

令左指针`l = 1`，令右指针`r = n`。遍历中点`m = l + (r - l) / 2`：
- 如果`guess(m) == 0`，返回`m`；
- 如果`guess(m) == -1`,则令`r = m - 1`;
- 如果`guess(m) == 1`,则令`l = m + 1`。

**题目解答**
```c++
int guessNumber(int n){
    int l = 1;
    int r = n;
    while (l < r) {
        int m = l + (r - l) / 2;
        if (guess(m) == 0) return m;
        else if (guess(m) == 1) l = m + 1;
        else r = m - 1;
    }
    return l;
}
```

### 1300. 转变数组后最接近目标值的数组和（未完成）
[题目链接](https://leetcode-cn.com/problems/sum-of-mutated-array-closest-to-target/)

**题目分析**



**题目解答**
```c++
int findBestValue(vector<int>& arr, int target) {
    sort(arr.begin(), arr.end());

    int max = *max_element(arr.begin(), arr.end());
    int sum = 0;
    vector<int> prefix(arr.size() + 1);
    for (int i = 1; i < arr.size() + 1; i++) {
        prefix[i] = prefix[i - 1] + arr[i - 1];
    }

    int sub = target;
    int res = 0;
    for (int i = 1; i <= max; i++) {
        auto iter = lower_bound(arr.begin(), arr.end(), i);

        int temp = prefix[iter - arr.begin()] + (arr.end() - iter) * i;
        // printf("%d %d %d\n", temp, i, prefix[idx]);
        if (abs(temp - target) < sub) {
            sub = abs(temp - target);
            res = i;
        }
    }
    return res;
}
```
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
### 124.二叉树中的最大路径和

[题目链接](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/submissions/)
**题目分析**
使用递归解决这个问题，分别递归当前结点的左右两个子树的最大路径和，在递归的过程中不断更新最大路径和。

递归的过程：
```c++
// 递归出口
if (!root) return 0;
// 左子树最大路径和
int left = max(maxGain(root->left), 0);
// 右子树最大路径和
int right = max(maxGain(root->right), 0);
// 当前路径和
int temp = root->val + left + right;
// 更新路径和
res = temp > res ? temp : res;

return root->val + max(left + right);
```

**题目解答**
```c++
int max(int x, int y) {
    return x > y ? x : y;
}

int maxGain(struct TreeNode* root, int* res) {
    if (!root) return 0;

    int left = max(maxGain(root->left, res), 0);
    int right = max(maxGain(root->right, res), 0);
    int temp = left + right + root->val;

    if (temp > *res) *res = temp;

    return root->val + max(left, right);
}

int maxPathSum(struct TreeNode* root){
    int res = INT_MIN;
    maxGain(root, &res);

    return res;
}
```
## 字符串

### 28.实现 strStr() (未完成)
**题目分析**
实际上就是实现一下kmp算法。

**题目解答**
```c++

```

### 14. 最长公共前缀
[题目链接](https://leetcode-cn.com/problems/longest-common-prefix/)

**题目分析**
可以采用纵向扫描的方式，所谓纵向扫描，就是同时比对所有字符串的第i个字符，如果全部都相同，则最长公共前缀+1；否则，跳出循环，返回当前遍历到的最长公共前缀。

遍历过程如下：
```c++
int flag = 0;
string res;
for (int i = 0; i < len; i++) {
    char temp = strs[0][i];
    for (int j = 1; j < num; j++) {
        if (i == strs[j].size() || temp != ) {
            flag = 1;
            break;
        }
    }
    if (flag == 1) break;
    res += temp;
}
```
最后返回`res`即可。

**题目解答**
```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        // 字符串个数
        int num = strs.size();
        if (num == 0) return "";
        // 第一个字符串长度，方便设定循环出口
        int len = strs[0].size();

        string res;
        int flag = 0;
        for (int i = 0; i < len; i++) {
            // 当前循环要比较的字符
            char temp = strs[0][i];
            // printf("%c ", temp);
            for (int j = 1; j < num; j++) {
                if (i == strs[j].size() || strs[j][i] != temp) {
                    flag = 1;
                    break;
                }
            }
            if (flag == 1) break;
            res += temp;
        }
        return res;
    }
};
```

### 67.二进制求和
[题目链接](https://leetcode-cn.com/problems/add-binary/)

**题目分析**
本题采用模拟法解决，首先确定二进制加法的基本法则：
```
1 + 1 = 10
1 + 0 = 01
0 + 0 = 00
```
所以我们可以从两个字符串的最后一位开始遍历，按照上面的法则运算，为了表示进位，我们需要定义一个进位符`flag`，有进位时，令`flag = 1`；没有进位时，令`flag = 0`。

同时，为了方便运算，我们可以令两个字符串等长，其实操作也很简单，只要在短的字符串前面加`0`字符即可。
```c++
int len_a = a.length();
int len_b = b.length();
if (len_a < len_b) swap(a, b);
while (b.length() < a.length()) {
    b = '0' + b;
}
```
然后定位进位符`flag = 0`, 从`i = a.length() - 1`开始遍历，遍历的情况：

- 当`a[i] = 1 && b[i] = 1`时
    - 如果`flag = 1`，令`temp = 1`;
    - 如果`flag = 0`，令`temp = 0, flag = 1`;
- 当`a[i] = 0 && b[i] = 0`时
    - 如果`flag = 1`，令`temp = 1, flag = 0`;
    - 如果`flag = 0`，令`temp = 0`;
- 当`a[i] = 1 || b[i] = 1`时
    - 如果`flag = 1`，令`temp = 0`;
    - 如果`flag = 0`，令`temp = 1`;
- 一轮遍历结束，`res = temp + res`;

遍历结束后，如果`flag = 1`，在结果前加字符`1`。

**题目解答**
```c++
class Solution {
public:
    string addBinary(string a, string b) {
        int len_a = a.length();
        int len_b = b.length();
        if (len_a < len_b) swap(a, b);
        while (b.length() < a.length()) {
            b = '0' + b;
        }
        
        string res;
        int it = a.length();
        int flag = 0;
        for (int i = it - 1; i >= 0; i--) {
            char temp;
            if (a[i] == '1' && b[i] == '1') {
                if (flag == 1) temp = '1';
                else {
                    temp = '0';
                    flag = 1;
                }
            } else if (a[i] == '0' && b[i] == '0') {
                if (flag == 1) {
                    temp = '1';
                    flag = 0;
                }else temp = '0';
            } else if (a[i] == '1' || b[i] == '1') {
                if (flag == 1) {
                    temp = '0';
                }else temp = '1';
            }
            // printf("%c %c %d %c\n", a[i], b[i], flag, temp);
            res = temp + res;
        }
        if (flag == 1) res = '1' + res;

        return res;
    }
};
```
