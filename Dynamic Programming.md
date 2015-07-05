leet上这道动规的题：https://leetcode.com/problems/word-break/
，给定字典，判断一个字符串是否能根据字典分词，只需要返回是或否。

虽然在DP的tag下，但我先按自己的想法，写了一个我以为是DP，但其实是贪心的算法：
一个个读入字符，每次读入都在字典中查找，一旦找到就清空，重新继续读入，读到最后，若能找到则返回true。

倒也通过了大部分测试样例，但卡在了字符串是`"aaaaa"`，而字典是`"aa", "aaa"`的情况，读到两个a就找到，丢掉，最后剩一个空荡荡的a。
经思考，贪心是没法解的，只要我一找到就扔掉，贪心就不能处理上述情况。

还是老老实实做动规。这道题，按照传统的动规思路，想要得到的状态转移方程之前都没碰到过类似的形式，因此没能自己想出来。

先上代码再分析，来自http://blog.csdn.net/feliciafay/article/details/18999903

```c++
class Solution {
public:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
    vector<bool> wordB(s.length() + 1, false);  
    wordB[0] = true;  
    for (int i = 1; i < s.length() + 1; i++) {  
        for (int j = i - 1; j >= 0; j--) {  
            if (wordB[j] && wordDict.count(s.substr(j, i - j))) {   //set的count函数就是判断元素有无的
                wordB[i] = true;  
                break; //只要找到一种切分方式就说明长度为i的单词可以成功切分，因此可以跳出内层循环。  
            }  
        }  
    }  
    return wordB[s.length()];  
    }
};
```

这里，真正的状态转移方程是：

> wordB[i] = true iff(if and only if) there exists j s.t. wordB[j]==true && the substring from j+1 to i can be found in the dictionary

是一个“存在性”的状态转移方程，和之前遇到的简单的取max之类的状态转移方程不同。要多琢磨琢磨！
这个状态方程并不难找，虽然找到任一个字串都可能被分成好多个单词，但wordB[j]正是对该字串的完整总结，只需要去字典里找j之后的字串即可。j将字串分为了两部分，但两部分地位并不完全相同，不是简单的分治。

需要注意一些细节，比如下标i和j的意义。i指的是前i个字符的字串能否分词，但string的下标又从零起，因此s.substr(j, i - j)实际正取出了第j+1到i个字符。此外，substr()函数的两个形参是substr(npos, count)，而非起止下标。以及wordB[0]=true的初始化也很重要，空的字串自然认为可以分词。

---

Took a deep look into Backpack Problem again, and found that how beautiful DP is! In BackPack Problem, it not only gives the maximum value, but can also easily gives the policy with no increase in time complexity!

想一句话说明白动态规划：
动态规划可以给出一类问题的最优解（满足最优化原理的问题）。
动态规划的核心思想就是顾名思义，动态规划。
形象地看，有一个N维状态数组记录状态，有一个状态转移方程对该数组操作。
本质地看，感觉是：查看所有相关的最优子策略（而不是遍历），找到当前阶段的最优策略。
