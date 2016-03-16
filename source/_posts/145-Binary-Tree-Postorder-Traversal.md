title: 145. Binary Tree Postorder Traversal
date: 2016-03-16 21:35:21
tags:
- leetcode
---
Given a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree {1,#,2,3},
   1
    \
     2
    /
   3
return [3,2,1].

解题思路：
方法一：递归方法。。。

```java
class Solution {
public:
	// recursive
    vector<int> postorderTraversal(TreeNode* root) {
         vector<int>res;
         postorder(root,res);
         return res;
    }
    void postorder(TreeNode* root,vector<int>&res){
        if(root==NULL)return ;
        if(root->left!=NULL)postorder(root->left,res);
        if(root->right!=NULL)postorder(root->right,res);
        res.push_back(root->val);
    }
};
```
方法二：非递归方法
参考：
http://zhidao.baidu.com/link?url=hi29Y_FNWekIU9L4YJV0hqzuzB6YXtQdH-7BJvfUUvzB731gxdH1_zMKfAZkBUo4JlhupBobBuq_usGqy-nrm_J9Fc79KE6i0WJ7tvb6sk3
每次将节点的左结点放入栈中，对于最左节点，若存在右节点，则将其放入栈中，修改最左节点的右节点为空，同时将该右节点的左节点依次放入栈中。若找到一个最左节点，没有右节点，则该节点被访问。。。
```java
// iterator
// reference:http://zhidao.baidu.com/link?url=hi29Y_FNWekIU9L4YJV0hqzuzB6YXtQdH-7BJvfUUvzB731gxdH1_zMKfAZkBUo4JlhupBobBuq_usGqy-nrm_J9Fc79KE6i0WJ7tvb6sk3
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
         vector<int>res;
         if(root==NULL)return res;
         stack<TreeNode*>s;
         TreeNode*p=root;
         while(p){
            s.push(p);
            p=p->left;
			}
         while(!s.empty()){
             p=s.top();
             //cout<<p->val<<endl;
             if(p->right){
                 //cout<<p->right->val<<endl;
                 s.push(p->right);
                 TreeNode*q=p->right;
                 p->right=NULL;
                 p=q;
                 while(p->left){
					//cout<<p->left->val<<"  fei"<<endl;
					s.push(p->left);
					p=p->left;
				}
             }else{
                 s.pop();
                 res.push_back(p->val);
				}
             }
         return res;
    }

};
```

方法三：类似前序遍历，不过将访问顺序改为根结点，右节点，左节点的顺序，栈可以实现。该过程是后序遍历的逆序。
```java
// another iterator
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
         vector<int>res;
         if(root==NULL)return res;
         stack<TreeNode*>s;
         s.push(root);
         while(!s.empty()){
             TreeNode*p=s.top();
             s.pop();
             res.push_back(p->val);
             if(p->left)s.push(p->left);
             if(p->right)s.push(p->right);
         }
         std::reverse(res.begin(),res.end());
         return res;
    }

};
```
