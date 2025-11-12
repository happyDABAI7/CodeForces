# 题解

## 1. 我的题解
```cpp
#include <bits/stdc++.h>
using namespace std;

void bj(vector<int>& bb) {  //处理二进制片段的 “进位” 操作
	auto w = find(bb.begin(), bb.end(), 0);
	if (w != bb.end()) {
		*w = 1;
		for (auto m = bb.begin(); m != w; ++m) {
			*m = 0;
		}
	}
	else {
		fill(bb.begin(), bb.end(), 0);
		bb.push_back(1);
	}
}

bool two(int x) {  //判断是不是2的幂
	return (x > 0) && ((x & (x - 1)) == 0);
}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(NULL);

	int t;
	cin >> t;
	while (t--) {
		int x;
		cin >> x;
		int w = x, count = 0;
		vector<int> arr;

		if (two(w)) {
			while (w != 1) {
				w /= 2;
				count++;
			}
			count++;
			int i = count;
			while (i--) {
				arr.push_back(0);
			}
			arr[count - 1] = 1;
			cout << count << '\n';
			for (int j : arr) cout << j << ' ';
			cout << '\n';
		}
		else {
			while (w != 0) {
				int r = w % 2;
				arr.push_back(r);
				w = w / 2;
			}
			int i = arr.size();
			for (int j = 0; j < i - 1; j++) {
				if (arr[j] == 1 && arr[j + 1] == 1) {
					arr[j] = -1;
					vector<int> tt;
					int q = j + 1;
					for (int m = q; m < i; m++) {
						tt.push_back(arr[m]);
					}
					arr.resize(q);
					bj(tt);
					for (int v : tt) arr.push_back(v);
					i = arr.size();
				}
			}
			cout << arr.size() << '\n';
			for (int j : arr) cout << j << ' ';
			cout << '\n';
		}

	}
}
```


### 思路
这段代码的核心是将输入整数转换为一种特殊二进制形式：系数仅为-1、0、1，且无连续的1。  

若整数是2的幂，直接构造对应长度的序列，仅最高位为1（符合无连续1的特性）；若不是2的幂，先转为普通二进制（存储0和1），再遍历消除连续的1——将低位连续1中的低位改为-1，对高位片段做进位处理（通过`bj`函数实现类似二进制进位的调整），最终得到符合要求的序列并输出。


<br>
<br>

## 2. 别人的题解
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=32+10;
int t,n,a[N];
void solve(int x){
	int cnt=0,ans=0;
	//如果将其反转，那么a数组就是x的二进制。所以不反转刚好起到了转成二进制并反转的作用
	while(x){
		a[++cnt]=(x&1);
		x>>=1;
	}
	a[cnt+1]=0;
	for(int i=1;i<=cnt;++i){
		if(a[i]==2)//进位
			a[i]=0,++a[i+1];
		if(a[i]==1&&a[i+1]==1)//判断当有连续两个1的情况
			a[i]=-1,a[i+1]=0,++a[i+2];
	}
	for(int i=1;i<=cnt+1;++i)
		if(a[i]) ans=i;//找到最终的位数
	printf("%d\n",ans);
	for(int i=1;i<=ans;++i)
		printf("%d ",a[i]);
	printf("\n");
	return;
}
int main(){
	cin>>t;
	while(t--){
		cin>>n;
		solve(n);
	}
	return 0;
}
```


### 思路
对于任意一个整数，我们可以将其转为二进制并逆转。
可以发现，如果存在 $A_i = A_{i+1} = 1$（即两个1相邻），那么这两个1的和会累加到 $2$，此时需要考虑进位处理。

>示例：29的处理过程
29的二进制逆转后为：`1,0,1,1,1`  
观察到 $A_3 = A_4 = 1$，满足相邻1的条件，处理方式如下：  
将 $A_3$ 变为 $-1$，$A_4$ 变为 $0$，并给 $A_5$ 加 $1$。  
此时 $A_5 = 2$，仍需继续进位，最终序列变为：`1,0,−1,0,0,1`。
