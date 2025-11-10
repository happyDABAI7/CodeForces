# 题解

## 1. 我的源代码：
```
#include<bits/stdc++.h>   //万能头
using namespace std;

int main(){
	int t,n,s,e,m1,m2;
	string str;
	cin>>t;
	while(t--){
		s=0;
		cin>>n;
		if(n==0) {
			cout<<"No\n";
			continue;
		}
		if(n==1){
			cout<<"Yes\n";
			continue;
		}
		e=n-1;
		cin>>str;
		if(n%2==0){
			m1=n/2-1;
			m2=m1+1;
		}
		else{
			m1=n/2;
			m2=m1;
		}
		while(s<e&&str[s]==str[e]){
			s++;
			e--;
		}
		if(s>=e) {
			cout<<"Yes\n";
			continue;
		}
		while(m1>=s&&m2<=e&&str[m1]==str[m2]){
			m1--;
			m2++;
		}
		if(m1==s){
			cout<<"Yes\n";
			continue;
		}
		while(s<=m1){
			if((str[s]-'0')!=(1-(str[e]-'0'))) break;
			s++;
			e--;
		}
		if(s==(m1+1)) cout<<"Yes\n";
		else cout<<"No\n";
	}
}
```
### 思路：
回文，就是将字符串对折可以重叠。由此得到----四指针法。
头尾，中间两位，共四个指针。偶数(<ins>1</ins>00<ins>11</ins>00<ins>1</ins>) 奇数(<ins>1</ins>01<ins>0</ins>1<ins>1</ins>01<ins>0</ins>)
首尾指针同步向中间移动，直至首尾指针所指向数字不同；中间两指针同步向两边移动，直至两指针所指向数字不同。
四个指针都停下后，首尾指针同步向中间两指针移动，并对比首尾指针所指向的数字是否不同，若不同则继续往下比；若相同[(str[s]-'0')!=(1-(str[e]-'0'))]，则停止，并输出No。
若首尾指针与中间两指针相遇，且此时的数字也不相同，则输出Yes；反之，No。

时间复杂度 O(t*n)。
<br>
<br>

## 2. 他人的题解：
```
#include<bits/stdc++.h>
using namespace std;
#define int long long  // 仍需保留，因为代码中int变量实际被替换为long long

void work(){
    int n;
    string a;
    cin >> n >> a;
    bool flag = false, flag1 = false;  // 用bool更贴合逻辑（原int本质是标记）
    for(int i = 0; i < (n >> 1); i++){
        if(a[i] != a[n - i - 1]){
            if(flag && flag1){
                cout << "No\n";
                return;
            }
            flag = true;
        } else if(flag){
            flag1 = true;
        }
    }
    cout << "Yes\n";
}

main(){
    int _;
    cin >> _;
    while(_--) work();
    return 0;
}
```

### 思路：
用两个指针从两端向中间扫，如果两端不同，说明其中一边肯定要改，因为只能改一次，所以我们就一直改左边的就好了。
如果发现有两个要改的地方，并且这两个地方中间有不需要改的，说明区间断开了，至少要改两次，就说明不能变成回文串。
如果区间没有断开，说明它可以。

时间复杂度 O(t*n)。
