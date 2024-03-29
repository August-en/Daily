# 二叉树的四种遍历方式
[一组动画彻底理解二叉树遍历](https://mp.weixin.qq.com/s?__biz=MzUyNjQxNjYyMg==&mid=100000221&idx=1&sn=628d039883b2cc0a243d83fb0ad6e4c8&scene=19#wechat_redirect)  

**二叉树节点定义**
```cpp
struct TreeNode{
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) : {}
};
```

## 前序遍历  
[Leetcode 144 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)  

**递归**
```cpp
vector<int> preoderTraversal(TreeNode* root){
    vector<int> res;
    preorder(root, res);
    return res;
}
void preorder(TreeNode* root, vector<int>& res){
    if(root){
        res.push_back(root->val);
        preorder(root->left, res);
        preorder(root->right, res);
    }
}
```
**迭代**
```cpp
vector<int> preorderTraversal(TreeNode* root){
    vector<int> res;
    stack<TreeNode*> s;
    TreeNode* p = root;
    while(p || !s.empty()){
        if(p){
            s.push(p);
            res.push_back(p->val);
            p = p->left;
        }
        else{
            p = s.top(); s.pop();
            p = p->right;
        }
    }
    return res;
}
```

## 中序遍历  
[Leetcode 94 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)  

**递归**
```cpp
vector<int> inorderTraversal(TreeNode* root){
    vector<int> res;
    inorder(root, res);
    return res;
}
void inorder(TreeNode* root, vector<int>& res){
    if(root){
        inorder(root->left, res);
        res.push_back(root->val);
        inorder(root->right, res);
    }
}
```
**迭代**
```cpp
vector<int> inorderTraversal(TreeNode* root){
    vector<int> res;
    stack<int> s;
    TreeNode* p = root;
    while(p || !s.empty()){
        if(p){
            s.push(p);
            p = p->left;
        }
        else{
            p = s.top(); s.pop();
            res.push_back(p->val);
            p = p->right;
        }
    }
    return res;
}
```

## 后序遍历
[Leetcode 145 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)  

**递归**
```cpp
vector<int> postorderTraversal(TreeNode* root){
    vector<int> res;
    postorder(root, res);
    return res;
}
void post(TreeNode* root, vector<int>& res){
    if(root){
        postorder(root->left, res);
        postorder(root->right, res);
        res.push_back(root->val);
    }
}
```
**迭代**
```cpp
vector<int> postorderTraversal(TreeNode* root){
    vector<int> res;
    stack<TreeNode*> s;
    TreeNode* p = root;
    while(p || !s.empty()){
        if(p){
            s.push(p);
            res.insert(res.begin(), p->val);
            p = p->right;
        }
        else{
            p = s.top(); s.pop();
            p = p->left;
        }
    }
    return res;
}
```

## 层序遍历  
[Leetcode 102 二叉树的层次遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/submissions/)  

**解法1**
```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int> > res;
    if(root == nullptr) return res;
        
    queue<TreeNode*> que;
    que.push(root);
    while(!que.empty()){
        vector<int> vec;
        int len = que.size();
        for(int i = 0; i < len; i++){  // que.size()在循环时一直变化，需要提前保存一下该层节点的数目！
            TreeNode* cur = que.front();
            que.pop();
            vec.push_back(cur->val);
            if(cur->left) que.push(cur->left);
            if(cur->right) que.push(cur->right);
        }
        res.push_back(vec);
    }
    return res;
}
```
**解法2**  
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int> > res;
        if(root == nullptr) return res;
        
        vector<int> vec;
        queue<TreeNode*> que;
        que.push(root);
        int toBeVisit = 1, nextLevel = 0;
        while(!que.empty()){
            TreeNode* cur = que.front();
            que.pop();
            vec.push_back(cur->val);
            toBeVisit--;
            if(cur->left){
                que.push(cur->left);
                nextLevel++;
            }
            if(cur->right){
                que.push(cur->right);
                nextLevel++;
            }
            if(toBeVisit == 0){  // 换层
                toBeVisit = nextLevel;
                nextLevel = 0;
                res.push_back(vec);
                vec.clear();
            }
        }
        return res;
    }
};
```
