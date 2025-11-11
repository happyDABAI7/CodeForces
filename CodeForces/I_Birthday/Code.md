# 题解

## 1. 我的题解
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
	int n,i;
	cin >> n;
	i=n;
	vector<int> h(n);
	
	while (i--) {
		cin >> h[i];
	}
	sort(h.begin(), h.end());
	deque<int> dq;
	dq.push_front(h[0]);
	for (int j = 1; j < n; j++) {  //重要实现代码块
		if (j % 2 == 0) dq.push_back(h[j]);
		else dq.push_front(h[j]);
	}
	for (int hh : dq) {
		cout << hh << " ";
	}
	cout << endl;

	return 0;
}
```

### 思路
最矮的站中间，第二矮站左边，第三矮站右边，循环往复。

时间复杂度为 O(n log n)。
<br>
<br>

## 2. 别人的题解
### 直接输出
```cpp
#include<bits/stdc++.h>
using namespace std;
int a[114];
int main(){
	int n;cin>>n;
	for(int i=0;i<n;i++){
		cin>>a[i];
	}
	sort(a,a+n);// 排序
	// 从小到大输出奇数位上的数
	for(int i=0;i<n;i++){
		if(i%2)cout<<a[i]<<" ";
	}
	// 从大到小输出偶数位上的数
	for(int i=n-1;i>=0;i--){
		if(i%2==0)cout<<a[i]<<" ";
	}
	return 0;
}
```

### 思路
形成山峰状站队，山峰为最高的人。
思路与我的相同，但这里直接输出，且因为左边的人编号都为奇数，右边的人都为偶数，所以直接根据这个规律排序后输出。




