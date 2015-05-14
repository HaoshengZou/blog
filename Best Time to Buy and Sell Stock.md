原始版本：https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
，给定每日定价，想要知道，只买卖一次的话最大收益是多少。

注意简单求最大最小值是不行的，极小值必须在极大值之前出现。我自己想到的动归是遍历两遍的联机算法，先从后往前遍历，记录该时刻及以后的最高卖价。再从前往后遍历，若当前时刻买，用之前得到的当前时刻之后的最高卖价减去当前价格，找到最大的这样的差即可。
进一步地，参见http://www.tuicool.com/articles/rMJZj2
，一次正向遍历可以记录当前及以前的最小值，用当前值减去即可。

版本2：https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/
，较简单，手贱看了tag，提示贪心算法就超简单了==简单想一想，因为我能知道每一天的价格，那么，只要知道后一天会跌，那我今天就卖，就行。

版本3：https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
，至多卖两次，求最大收益。

没想出来，上网查的http://www.tuicool.com/articles/rMJZj2
，这里才真正需要正反两次遍历，且稍加思考就知道，实际只买卖一次（比如一直涨，中途某天卖了又买）的情况也考虑进去了。第一次正向遍历，记录到当前日期的最大收益。第二次反向遍历，计算当前日期往后的最大收益，加上第一次遍历记录的当前日期往前的最大收益值，就是总的收益。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size(), maxP=0;
        if (n<=1) return 0;
        vector<int> fw(n, 0), bw(n, 0); //OK?
        int fw_low=prices[0], bw_high=prices[n-1];
        
        //forward traversal
        for (int i=1; i<n; i++)
        {
            if (prices[i]<fw_low)
            {
                fw_low = prices[i];
                fw[i] = fw[i-1];
            }
            else fw[i] = max( fw[i-1], prices[i]-fw_low );
        }
        
        //backward traversal and find maxP
        for (int i=n-2; i>=0; i--)
        {
            if (prices[i]>bw_high)
            {
                bw_high = prices[i];
                bw[i] = bw[i+1];
            }
            else
            {
                bw[i] = max( bw[i+1], bw_high-prices[i] );
                if (fw[i] + bw[i] > maxP)
                    maxP = fw[i] + bw[i];
            }
        }
        return maxP;
    }
};
```

版本4：https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/

直接参考http://blog.csdn.net/linhuanmars/article/details/23236995
