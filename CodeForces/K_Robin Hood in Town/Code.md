# 题解

## 1. 我的题解
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        vector<int> arr(n);
        long long sum = 0;
        for (int i = 0; i < n; ++i) {
            cin >> arr[i];
            sum += arr[i];
        }
        sort(arr.begin(), arr.end());

        if(n<=2) {
			cout<<-1<<'\n';
			continue;
		}

        long long l = 0, r = 1e18;
        long long  f = -1;
        while (l <= r) {
            long long mid = l + (r - l) / 2;  //二分查找法找mid
            double all = sum + mid;
            double avg = all / n;
            double aa = avg / 2.0;
            int c = lower_bound(arr.begin(), arr.end(), aa) - arr.begin(); //找到现在这个x在财富值数组中的位置

            if (c > n / 2) {
                f = mid;
                r = mid - 1;
            }
            else {
                l = mid + 1;
            }
        }

        cout << f << '\n';
    }
    return 0;
}
```

### 思路
本质是**二分查找法**。
给x设置一个超级大的初始范围->0~1e18
接着利用二分查找法，查找x的值，借这个x求平均财富的一半，用这个值去看有多少人少于这个财富值，人数是否满足多于一半的人不开心。
若超过一般的人不开心，则将右边界置为mid-1；反之，左边界置为mid+1。(这也就是二分查找法的关键)
直至找到这个x，循环停止。
注意，人们的财富值数组需要先排序，才能使用二分查找法。
此外，注意n<=2时，无解。

时间复杂度：O(t × n log n)。
<br>
<br>

## 2. 别人的题解
```cpp
#include <iostream>
#include <algorithm>
using namespace std;

using LL = long long;

const int N = 2e5 + 5;

LL a[N];

int main()
{
	int t;
	cin >> t;
	while(t--)
	{
		int n;
		cin >> n;
		LL sum = 0;
		for(int i = 1; i <= n; i++) //注意数组从1开始存元素
		{
			cin >> a[i];
			sum += a[i];
		}
		if(n <= 2)
		{
			cout << -1 << endl;
			continue;
		}
		sort(a+1, a+n+1);
		LL p = a[n/2+1];
		LL m = p * 2 * n + 1;
		cout << max(0LL, m-sum) << endl;
	}
	return 0;
}
```

### 思路
需要让一半以上的人不开心，那就找到那个中间点，这个点及其左边的数量和刚好超过一半人，所以这个点上的值要严格小于加上x后的平均值的一半。
因为 $p<(sum+x)/(2n)$
变形得 $(2np-sum)<x$
所以利用这个值p反向计算，先*2再*n，再-sum，此时得到值t，此时的结果应当 $<x$ 。
由于x是非负整数，所以此时t+1后一定严格大于t，所以 $x=t+1$,
此外，这时的x可能会算出负数，但负数显然不合理，所以，设置当x<0时，x置为0。

正向推断+反向计算

时间复杂度为 O(t * n log n)。
