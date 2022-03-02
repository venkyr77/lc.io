# Fb Prep

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

```

### BFS

```cpp

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
