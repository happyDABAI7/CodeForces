# 题解

## 1. 我的题解
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {

	int t;
	cin >> t;
	while (t--) {
		int n, m;
		cin >> n >> m;
		vector<long long> a(n);
		for (int i = 0; i < n; ++i) {
			cin >> a[i];
		}
		long long oo = *max_element(a.begin(), a.end());
		while (m--) {
			char c;
			long long l, r;
			cin >> c >> l >> r;

			if (oo >= l && oo <= r) {
				if (c == '+') oo++;
				else oo--;
			}
			cout << oo << ' ';
		}
		cout << '\n';
	}
}
```


### 思路
永远只考虑最开始的那个最大值即可，无论是+还是-，最大值永远是那个。

输出最大值，那么考虑最大值有什么性质，发现对于相等的数，无论是操作 1 还是操作 2，这些相等的数还是相等，因为它们都同样地 +1 或 −1，推而广之，当一个不是最大值的数，无论他怎样地 +1，他肯定在某次操作后，变为最大值，而根据上文的结论，他之后会一直与最大值相等，对于其他数亦是如此，那么得：所有数不会超过最大值。

时间复杂度为 O(t × (n + m))。
<br>
<br>

## 2. 别人的题解



### 思路
