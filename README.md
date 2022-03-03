# Fb Prep

## Iterative O(1) inorder -> Morris Order



## 1) 1249. Minimum Remove to Make Valid Parentheses

```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        int n = s.size();
        
        int op = 0, cl = 0;
        
        for(int i = 0; i < n; i++)
        {
            if(s[i] == '(')
            {
                op++;
            }
            
            else if(s[i] == ')')
            {
                cl++;
            }
            
            if(cl > op)
            {
                cl--;
                s[i] = '#';
            }
        }
        
        op = 0;
        cl = 0;
        
        for(int i = n - 1; i >= 0; i--)
        {
            if(s[i] == '(')
            {
                op++;
            }
            
            else if(s[i] == ')')
            {
                cl++;
            }
            
            if(op > cl)
            {
                op--;
                s[i] = '#';
            }
        }
        
        string ans = "";
        
        for(char c : s)
        {
            if(c != '#')
            {
                ans += c;
            }
        }
        
        return ans;
    }
};
```

## 2) 680. Valid Palindrome II

```cpp
class Solution {
public:
    bool ispalindrome(string& s, int l, int r)
    {
        while(l < r)
        {
            if(s[l++] != s[r--])
            {
                return false;
            }
        }
        
        return true;
    }
    
    bool validPalindrome(string s) {
        int n = s.size();
        
        int l = 0, r = n - 1;
        
        while(l < r)
        {
            if(s[l] != s[r])
            {
                return ispalindrome(s, l + 1, r) || ispalindrome(s, l, r - 1);
            }
            
            else
            {
                l++;
                r--;
            }
        }
        
        return true;
    }
};
```

## 3) 314. Binary Tree Vertical Order Traversal

### DFS

```cpp
class Solution {
public:
    void dfs(TreeNode* root, map<int, vector<pair<int, int>>>& col_map, vector<vector<int>>& ans, int r, int c)
    {
        if(!root)
        {
            return;
        }
        
        col_map[c].push_back({r, root->val});
        
        dfs(root->left, col_map, ans, r + 1, c - 1);
        dfs(root->right, col_map, ans, r + 1, c + 1);
    }
    
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> ans;
        map<int, vector<pair<int, int>>> col_map;
        
        dfs(root, col_map, ans, 0, 0);
        
        for(auto it : col_map)
        {
            vector<pair<int, int>> col_values = it.second;
            
            sort(col_values.begin(), col_values.end(), [](const pair<int, int>& a, const pair<int, int>& b) -> bool {
                return a.first < b.first;
            });
            
            vector<int> col;
            
            for(pair<int, int>& p : col_values)
            {
                col.push_back(p.second);
            }
            
            ans.push_back(col);
        }
        
        return ans;
    }
};
```

### BFS

```cpp
class Solution {
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        if(!root)
        {
            return {};
        }
        
        vector<vector<int>> ans;
        
        unordered_map<int, vector<int>> col_to_row_map;
        
        int min_col = INT_MAX, max_col = INT_MIN;
        
        queue<pair<TreeNode*, vector<int>>> q;
        
        q.push({root, {0, 0}});
        
        while(!q.empty())
        {
            auto f = q.front();
            q.pop();
            
            TreeNode* node = f.first;
            int r = f.second[0];
            int c = f.second[1];
            
            col_to_row_map[c].push_back(node->val);
            
            min_col = min(min_col, c);
            max_col = max(max_col, c);
            
            if(node->left)
            {
                q.push({node->left, {r + 1, c - 1}});
            }
            
            if(node->right)
            {
                q.push({node->right, {r + 1, c + 1}});
            }
        }
        
        for(int col = min_col; col <= max_col; col++)
        {
            ans.push_back(col_to_row_map[col]);
        }
        
        return ans;
    }
};
```

## 4) 1762. Buildings With an Ocean View

```cpp
class Solution {
public:
    vector<int> findBuildings(vector<int>& heights) {
        int n = heights.size();
        
        vector<int> ans;
        
        for(int i = 0; i < n; i++)
        {
            while(!ans.empty() && heights[ans.back()] <= heights[i])
            {
                ans.pop_back();
            }
            
            ans.push_back(i);
        }
        
        return ans;
    }
};
```

## 5) 1570. Dot Product of Two Sparse Vectors

```cpp
class SparseVector {
public:
    vector<pair<int, int>> spvec;
    SparseVector(vector<int> &nums) {
        int n = nums.size();
        
        for(int i = 0; i < n; i++)
        {
            if(nums[i] != 0)
            {
                spvec.push_back({i, nums[i]});
            }
        }
    }
    
    // Return the dotProduct of two sparse vectors
    int dotProduct(SparseVector& vec) {
        int ans = 0;
        
        int l1 = 0, l2 = 0;
        int n1 = spvec.size(), n2 = vec.spvec.size();
        
        while(l1 < n1 && l2 < n2)
        {
            if(spvec[l1].first == vec.spvec[l2].first)
            {
                ans += spvec[l1].second * vec.spvec[l2].second;
                l1++;
                l2++;
            }
            else if(spvec[l1].first < vec.spvec[l2].first)
            {
                l1++;
            }
            else
            {
                l2++;
            }
        }
        
        return ans;
    }
};
```

## 6) 938. Range Sum of BST

```cpp
class Solution {
public:
    void dfs(TreeNode* root, int low, int high, int& ans)
    {
        if(!root)
        {
            return;
        }
        
        if(root->val >= low && root-> val <= high)
        {
            ans += root->val;
        }
        
        if(root->val > low)
        {
            dfs(root->left, low, high, ans);
        }
        
        if(root->val < high)
        {
            dfs(root->right, low, high, ans);
        }
    }
    
    int rangeSumBST(TreeNode* root, int low, int high) {
        int ans = 0;
        dfs(root, low, high, ans);
        return ans;
    }
};
```

## 7) 528. Random Pick with Weight

```cpp
class Solution {
private:
    vector<int> psum;
public:
    Solution(vector<int>& w) {
        for(int i : w)
        {
            if(psum.empty())
            {
                psum.push_back(i);
            }
            
            else
            {
                psum.push_back(psum.back() + i);
            }
        }
    }
    
    int pickIndex() {
        float r = (float)(rand()) / (float)(RAND_MAX);
        float pos = r * psum.back();
        return upper_bound(psum.begin(), psum.end(), pos) - psum.begin();
    }
};
```

## 8) 921. Minimum Add to Make Parentheses Valid

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int n = s.size();
        
        int op = 0, cl = 0, ans = 0;
        
        for(int i = 0; i < n; i++)
        {
            if(s[i] == '(')
            {
                op++;
            }
            
            else if(s[i] == ')')
            {
                cl++;
            }
            
            if(cl > op)
            {
                op++;
                ans++;
            }
        }
        
        op = 0, cl = 0;
        
        for(int i = n - 1; i >= 0; i--)
        {
            if(s[i] == '(')
            {
                op++;
            }
            
            else if(s[i] == ')')
            {
                cl++;
            }
            
            if(op > cl)
            {
                cl++;
                ans++;
            }
        }
        
        return ans;
    }
};
```

## 9) 408. Valid Word Abbreviation

```cpp
class Solution {
public:
    int get_length(string& A, int& pos)
    {
        int n = A.size();
        
        if(pos >= n || A[pos] == '0')
        {
            return -1;
        }
        
        int ans = 0;
        
        while(pos < n && isdigit(A[pos]))
        {
            ans *= 10;
            ans += A[pos] - '0';
            pos++;
        }
        
        return ans;
    }
    
    bool validWordAbbreviation(string W, string A) {
        int wn = W.size();
        int an = A.size();
        
        int i = 0, j = 0;
        
        while(i < wn && j < an)
        {
            if(isdigit(A[j]))
            {
                int length = get_length(A, j);
                
                if(length == -1)
                {
                    return false;
                }
                
                i += length;
            }
            
            else
            {
                if(W[i] != A[j])
                {
                    return false;
                }
                
                i++;
                j++;
            }
        }
        
        return i == wn && j == an;
    }
};
```

## 10) 339. Nested List Weight Sum

### DFS

```cpp
class Solution {
public:
    void dfs(NestedInteger& u, int& ans, int level)
    {
        if(u.isInteger())
        {
            ans += u.getInteger() * level;
        }
        
        else
        {
            for(NestedInteger v : u.getList())
            {
                dfs(v, ans, level + 1);
            }
        }
    }
    
    int depthSum(vector<NestedInteger>& nestedList) {
        int ans = 0;
        
        for(NestedInteger n : nestedList)
        {
            dfs(n, ans, 1);
        }
        
        return ans;
    }
};
```

### BFS

```cpp
class Solution {
public:
    int depthSum(vector<NestedInteger>& nestedList) {
        int ans = 0;
        
        queue<pair<NestedInteger&, int>> q;
        
        for(NestedInteger& n : nestedList)
        {
            q.push({n, 1});
        }
        
        while(!q.empty())
        {
            NestedInteger& u = q.front().first;
            int level = q.front().second;
            q.pop();
            
            if(u.isInteger())
            {
                ans += u.getInteger() * level;
            }
            
            else
            {
                for(NestedInteger& v : u.getList())
                {
                    q.push({v, level + 1});
                }
            }
        }
        
        return ans;
    }
};
```

## 11) 71. Simplify Path

```cpp
class Solution {
public:
    string simplifyPath(string path) {
        vector<string> levels;
        
        string tmp = "";
        
        for(char c: path)
        {
            if(c == '/')
            {
                if(tmp != "")
                {
                    levels.push_back(tmp);
                }
                
                tmp = "";
            }
            
            else
            {
                tmp += c;
            }
        }
        
        if(tmp != "")
        {
            levels.push_back(tmp);
        }
        
        vector<string> st;
        
        for(string s : levels)
        {
            if(s == "..")
            {
                if(!st.empty())
                {
                    st.pop_back();
                }
            }
            
            else if(s != ".")
            {
                st.push_back(s);
            }
        }
        
        tmp = "";
        
        for(string s : st)
        {
            tmp += "/" + s;
        }
        
        return tmp == "" ? "/" : tmp;
    }
};
```

## 12) 426. Convert Binary Search Tree to Sorted Doubly Linked List

### Iterative inorder using stack

```cpp
class Solution {
public:
    void inorder(stack<Node*>& st, Node* root)
    {
        if(!root)
        {
            return;
        }
        
        Node* tmp = root;
        
        while(tmp)
        {
            st.push(tmp);
            tmp = tmp->left;
        }
    }
    
    Node* treeToDoublyList(Node* root) {
        if(!root)
        {
            return NULL;
        }
        
        stack<Node*> st;
        inorder(st, root);
        
        Node* dummy = new Node();
        Node* curr = dummy;
        
        Node* t;
        
        while(!st.empty())
        {
            t = st.top();
            st.pop();
            
            curr->right = t;
            t->left = curr;
            
            if(t->right)
            {
                inorder(st, t->right);
            }
            
            curr = curr->right;
        }
        
        dummy->right->left = t;
        t->right = dummy->right;
        
        return dummy->right;
    }
};
```

## 13) 973. K Closest Points to Origin

### Max Heap

```cpp
class Solution {
public:
    int dis(vector<int>& p)
    {
        return p[0] * p[0] + p[1] * p[1];
    }
    
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        int n = points.size();
        
        vector<vector<int>> ans;
        
        priority_queue<pair<int, int>> pq;
        
        for(int i = 0; i < n; i++)
        {
            pq.push({dis(points[i]), i});
            
            if(pq.size() > k)
            {
                pq.pop();
            }
        }
        
        while(!pq.empty())
        {
            ans.push_back(points[pq.top().second]);
            pq.pop();
        }
        
        return ans;
    }
};
```

## 14) 227. Basic Calculator II

```cpp

```

## 15) 215. Kth Largest Element in an Array

```cpp
#define rand(a,b) a + rand() % (b - a + 1)
class Solution {
public:
    int randomized_select(vector<int>& arr, int l, int r, int k)
    {    
        if(l == r)
        {
            return arr[l];
        }
        
        int rpind = randomized_partition(arr, l, r);
        int rprank = rpind - l + 1;
        
        if(k == rprank)
        {
            return arr[rpind];
            
        }
        
        else if(k < rprank)
        {
            return randomized_select(arr, l, rpind - 1, k);
        }
        
        return randomized_select(arr, rpind + 1, r, k - rprank);
    }
    
    int partition(vector<int>& arr, int l, int r)
    {
        for(int i = l; i < r; i++)
        {
            if(arr[i] >= arr[r])
            {
                swap(arr[l++], arr[i]);
            }
        }
        swap(arr[l], arr[r]);
        return l;
    }
    
    int randomized_partition(vector<int>& arr, int l, int r)
    {
        int pivot = rand(l, r);
        swap(arr[pivot], arr[r]);
        return partition(arr, l, r);
    }
    
    int findKthLargest(vector<int>& nums, int k) {
        return randomized_select(nums, 0, nums.size() - 1, k);
    }
};
```

## 16) 50. Pow(x, n)

### Recursive

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if(n == 0)
        {
            return 1.0;
        }
        
        if(n == 1)
        {
            return x;
        }
        
        if(n == -1)
        {
            return 1.0 / x;
        }

        if(n % 2)
        {
            return x * myPow(x, n - 1);
        }
        
        double tmp = myPow(x, n / 2);
        return tmp * tmp;
    }
};
```

### Iterative

```cpp
class Solution {
public:
    double myPow(double x, int N) {
        long long n = N;
        
        if(n < 0)
        {
            x = 1.0 / x;
            n = -n;
        }
        
        double ans = 1;
        
        while(n > 0)
        {
            if(n & 1)
            {
                ans = ans * x;
            }
            
            x = x * x;
            n = n >> 1;
        }
        
        return ans;
    }
};
```

## 17) 987. Vertical Order Traversal of a Binary Tree

```cpp

```

## 18) 199. Binary Tree Right Side View

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(!root)
        {
            return {};
        }
        
        queue<TreeNode*> q;
        q.push(root);
        vector<int> rv;
        
        while(!q.empty())
        {
            int n = q.size();
            while(n--)
            {
                TreeNode* front = q.front();
                q.pop();
                
                if(n == 0)
                {
                    rv.push_back(front->val);
                }
                
                if(front->left)
                {
                    q.push(front->left);
                }
                
                if(front->right)
                {
                    q.push(front->right);
                }
                
            }
        }
        
        return rv;
    }
};
```

## 19) 31. Next Permutation

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        
        int r = n - 1, ind = -1;
        
        for(int i = n - 1; i >= 1; i--)
        {
            if(nums[i] > nums[i - 1])
            {
                ind = i;
                break;
            }
        }
        
        if(ind == -1)
        {
            reverse(nums.begin(), nums.end());
            return;
        }
        
        for(int i = n - 1; i >= ind; i--)
        {
            if(nums[i] > nums[ind - 1])
            {
                swap(nums[i], nums[ind - 1]);
                break;
            }
        }
        
        reverse(nums.begin() + ind, nums.end());
    }
};
```

## 20) 827. Making A Large Island

```cpp
class Solution {
public:
    void make_set(int v, vector<int>& parent, vector<int>& size)
    {
        parent[v] = v;
        size[v] = 1;
    }
    
    int find_set(int v, vector<int>& parent)
    {
        if(parent[v] == v)    
        {
            return v;
        }
        int rep = find_set(parent[v], parent);
        parent[v] = rep;
        return rep;
    }
    
    void union_sets(int a, int b, vector<int>& parent, vector<int>& size)
    {
        a = find_set(a, parent);
        b = find_set(b, parent);
        if(a != b)
        {
            if(size[a] < size[b])
            {
                swap(a, b);
            }
            parent[b] = a;
            size[a] += size[b];
        }
    }
    
    bool isinside(int m, int n, int i, int j)
    {
        return (i >= 0 && i < m && j >= 0 && j < n);
    }
    
    int largestIsland(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        vector<int> parent(m * n);
        vector<int> size(m * n);
        
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                int set_num = i * m + j;
                make_set(set_num, parent, size);
            }
        }
        
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j])
                {
                    int dx[4] = {-1, 1, 0, 0};
                    int dy[4] = {0, 0, -1, 1};
                    
                    for(int k = 0; k < 4; k++)
                    {
                        int x = i + dx[k];
                        int y = j + dy[k];
                        int set_num1 = i * m + j;
                        int set_num2 = x * m + y;
                        if(isinside(m, n, x, y) && grid[x][y] && find_set(set_num1, parent) != find_set(set_num2, parent))
                        {
                            union_sets(set_num1, set_num2, parent, size);
                        }
                    }
                }
            }
        }
        
        int maxx = INT_MIN;
        
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(!grid[i][j])
                {
                    int dx[4] = {-1, 1, 0, 0};
                    int dy[4] = {0, 0, -1, 1};
                    
                    unordered_set<int> s;
                    
                    int test = 1;
                    
                    for(int k = 0; k < 4; k++)
                    {
                        int x = i + dx[k];
                        int y = j + dy[k];
                        int set_num1 = i * m + j;
                        int set_num2 = x * m + y;
                        if(isinside(m, n, x, y) && grid[x][y])
                        {
                            int rep = find_set(set_num2, parent);
                        
                            if(s.count(rep) == 0)
                            {
                                s.insert(rep);
                                test += size[rep];
                            }
                        }
                    }
                    
                    maxx = max(maxx, test);
                }
            }
        }
        
        return maxx == INT_MIN ? m * n : maxx;
    }
};
```

## ) 2. Add Two Numbers

1. We will build the answer node by node.
2. We start from the head of the two lists. If both of them are empty, we check the carry.
3. If carry is non empty then we return a node with carry's value.
4. If either list1 is non empty or list2 is non empty we proceed. If list1 is non empty we choose the node to be list1's current node.
5. We will add the sum of the two nodes to this node to prevent extra space allocation.
6. We add the current nodes of the list and appropirately call the recursive function for the next nodes of the two lists.

```cpp
class Solution {
public:
    ListNode* add(ListNode* l1, ListNode* l2, int carry)
    {
        if(!l1 && !l2)
        {
            return carry ? new ListNode(carry) : NULL;
        }
        
        ListNode* node = l1 ? l1 : l2;
        
        int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry;
        
        node->val = sum % 10;
        carry = sum / 10;
        
        node->next = add(l1 ? l1->next : NULL, l2 ? l2->next : NULL, carry);
        
        return node;
    }
    
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        return add(l1, l2, 0);
    }
};
```

## ) 56. Merge Intervals

1. Sort the intervals by start time.

2. We will start from the first interval, that is the interval with the minimum start time and start adding intervals to our final set of answer.

3. If the current interval's start time is lesser than the last added interval's end time, we update the last added interval's end time in the answer array. Else we add the interval to the answer.

``` cpp
class Solution {
public:
    
    static bool comp(vector<int>& a, vector<int>& b)
    {
        return a[0] < b[0];
    }
    
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int n = intervals.size();
        
        vector<vector<int>> ans;
        
        sort(intervals.begin(), intervals.end(), comp);
        
        for(int i = 0; i < n; i++)
        {
            if(ans.empty() || ans.back()[1] < intervals[i][0])
            {
                ans.push_back(intervals[i]);
            }
            
            else
            {
                ans.back()[1] = max(ans.back()[1], intervals[i][1]);
            }
        }
        
        return ans;
    }
};
```

## 2) 146. LRU Cache

```cpp
class LRUCache {
private:
    int cap;
    list<int> recent_keys;
    unordered_map<int, int> cache;
    unordered_map<int, list<int>::iterator> key_index_map;
    
public:
    void update(int key)
    {
        if(key_index_map.count(key))
        {
            auto pos_to_delete = key_index_map[key];
            recent_keys.erase(pos_to_delete);
        }
        
        else if(recent_keys.size() >= cap)
        {
            int key = recent_keys.back();
            recent_keys.pop_back();
            cache.erase(key);
            key_index_map.erase(key);
        }
        
        recent_keys.push_front(key);
        key_index_map[key] = recent_keys.begin();
    }
    
    LRUCache(int capacity) {
        cap = capacity;
    }
    
    int get(int key) {
        if(cache.count(key))
        {
            update(key);
            return cache[key];
        }
        
        else
        {
            return -1;
        }
    }
    
    void put(int key, int value) {
        update(key);
        cache[key] = value;
    }
};
```
