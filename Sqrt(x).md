https://leetcode.com/problems/sqrtx/
，实现开平方函数，返回一个整数的平方根的整数部分。可以采用二分查找，在C++实现中我的算法已经是很快的了。

这道题很长士气，完全是自己努力想出来的。自己想算法的时候也要用一些特殊值去算，去检验，去尝试，通过尝试完善算法的一些细节。为了做这道题，我先是自己写了一遍二分查找，才发现自己其实从来没有写过二分查找T_T。比如说可以这么实现：

```C++
int biSearch(vector<int> &x, int t, int l, int r) //return the index of target t
{
	int mid=(l+r)/2;
	if (x[mid] == t) return mid;
	if (x[mid] > t)
		return biSearch(x, t, l, mid-1);
	else return biSearch(x, t, mid+1, l);
}
```

注意普通查找里，若要继续递归查找，常常mid-1和mid+1。但在开平方函数里，经过自己的尝试（从5到15），发现不加减一，直接用mid递归调用是对的（加减一或许也是对的）。因为返回的是整数部分，所以x/mid和mid相差1就是递归终点，返回两者中较小的，如下：

```C++
    int biSqrt(int x, int l, int r)
    {
        int mid = (l+r)/2;
        if (x/mid == mid || x/mid - mid == 1) return mid;
        if (x/mid - mid == -1) return x/mid;
        if (x/mid > mid) return biSqrt(x, mid, r);
        if (x/mid < mid) return biSqrt(x, l, mid);
    }
```

这道题好像也能用牛顿法求解，参见https://leetcode.com/discuss/24965/newtons-iterative-method-in-c
，复习了，牛顿法可用来求函数的零点，在这里注意，要解f(y)=y-sqrt(x)=0，写成f(y)=y*y-sqrt(x)=0才好求导解，否则对y求导就是trivial的1了。
