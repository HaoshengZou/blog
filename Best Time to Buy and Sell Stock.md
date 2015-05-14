原始版本：https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
，给定每日定价，只要知道买卖一次最大收益是多少。

注意简单求最大最小值是不行的，极小值必须在极大值之前出现。我自己想到的动归是遍历两遍的联机算法，先从后往前遍历，记录该时刻
