在leetcode.com上刷到一题Binary Tree Right Side View，从整棵树的右侧看这棵树，输出每一层最右边的元素。

第一反应是要层序遍历，第一次使用队列、做BFS，excited！
深刻体会到STL的方便，list一用，队列其实非常简单。也是leet上前所未有的首次submit即accepted，excited！

```
class Solution {
public:
    vector<int> rightSideView(TreeNode *root) {
        vector<int> view;
        if (root==NULL) return view; //empty tree
        else
        {
            list<TreeNode*> parent, child;
            TreeNode* tmp;
            parent.push_back(root);
            while (!parent.empty()) //still has layers to go
            {
                view.push_back(parent.back()->val);
                while (!parent.empty()) //visit next layer
                {
                    tmp = parent.front();
                    if (tmp->left != NULL) child.push_back(tmp->left);
                    if (tmp->right != NULL) child.push_back(tmp->right);
                    parent.pop_front();

                }
                parent = child;
                child.clear();
            }
            return view;
        }
    }
};
```
