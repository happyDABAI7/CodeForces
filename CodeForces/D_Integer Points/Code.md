# 题解

## 1. 我的题解
```
#include<bits/stdc++.h>
using namespace std;

int main() {
	int t, n, m;
	cin >> t;

	while (t--) {
		long long count = 0;
		cin >> n;
		int i = n;
		vector<int> arr1;
		while (i--) {
			int p;
			cin >> p;
			arr1.push_back(p);
		}
		cin >> m;
		int j = m;
		vector<int> arr2;
		while (j--) {
			int q;
			cin >> q;
			arr2.push_back(q);
		}

		long long p1 = 0, p2 = 0, q1 = 0, q2 = 0;
		for (int x : arr1) {
			if (x % 2 == 0) p1++; else p2++;
		}
		for (int x : arr2) {
			if (x % 2 == 0) q1++; else q2++;
		}
		count = p1 * q1 + p2 * q2;
		cout << count << '\n';
	}
}
```

### 思路
这是一道数学算术题
要求 $y = x + p_i$ 和 $y = -x + q_i$ 的交点，且交点横纵坐标是整数。
那么就联立两个式子，得到x=(b-a)/2，要使x为整数，则(b-a)%2应当等于0，即(b-a)是偶数。
根据 <span style="color: #c40e0eff;">奇数相加减，为偶数；偶数相加减，为偶数</span> 这一规律，可得最终结果应为 "a为偶数个数\*b为偶数个数 + a为奇数个数\*b为奇数个数" 。

时间复杂度是 O(t * (n + m))，其中 t 是测试用例的数量，n 是第一个数组的长度，m 是第二个数组的长度。
<br>
<br>

## 2. 别人的题解


