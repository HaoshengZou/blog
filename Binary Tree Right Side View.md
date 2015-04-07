在leetcode.com上刷到一题Binary Tree Right Side View，从整棵二叉树的右侧看这棵树，输出每一层最右边的元素。
https://leetcode.com/problems/binary-tree-right-side-view/

第一反应是要层序遍历，第一次使用队列、做BFS，excited！

深刻体会到STL的方便，list一用，实现队列其实非常简单。这道题也是leet上前所未有的首次submit即accepted，excited！

```C++
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

层序遍历（亦称广度优先搜索），使用了两个队列，分别记录当前层和下一层结点的指针。复杂度都是O(N)的，因为只遍历一次。

在leet上的discuss里，看到居然有人用深度优先搜索DFS，也能得到正确的答案。代码如下：

```C++
class Solution {
public:
    void recursion(TreeNode *root, int level, vector<int> &res)
    {
        if(root==NULL) return ;
        if(res.size()<level) res.push_back(root->val);
        recursion(root->right, level+1, res);
        recursion(root->left, level+1, res);
    }

    vector<int> rightSideView(TreeNode *root) {
        vector<int> res;
        recursion(root, 1, res);
        return res;
    }
};
```

这个编程技巧首先告诉了我在leet上如何在不改变答案接口的前提下改变形参个数，跟Weiss书里看到的一样，答案提供的接口只是一个驱动，再由它调用另一个函数即可。

DFS的方法仔细一想，确实是对的（当然也submit过惊觉accepted）。特别要注意的是两个递归调用的顺序，先向右`recursion(root->right, level+1, res);`，再向左`recursion(root->left, level+1, res);`。琢磨了一下，其中其实蕴含着贪心算法的思想，能向右找就尽量向右找。而判断是否找到某层的最右元素，用`if(res.size()<level) res.push_back(root->val);`。

可以简单地证明，上述if语句不会对某层的偏左元素先执行。文字证明如下：

> 对某层的两个结点A（左）、B（右），递归例程一定先访问B，因为A、B结点向上回溯，总会找到一个相同的父亲，A在其左孩子，B在其右孩子（这是基于“二叉树结构是不交叉的”这一事实）。而两个递归调用的顺序，决定了先访问B再访问A。

对这道题，BFS和DFS都能用，还是很神奇的。我想了想，跟树是二叉而非多叉有关，但多叉树的数据结构就不一样了，孩子结点整个用一串链表表示，方法自然也不同。
