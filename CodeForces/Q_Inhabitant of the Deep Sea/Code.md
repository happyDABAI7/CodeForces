# 题解

## 1. 我的题解
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(NULL);

    int t;
    cin>>t;
    while (t--) {
        int n;
        long long k;
        cin >> n >> k;
        vector<long long> a;
        int i=n;
    	while(i--){
			int m; 
			cin>>m; 
			a.push_back(m); 
		}

        int x = 0, y = n - 1;
        int mm = 0;

        while (x <= y && k > 0) {
            long long L = a[x];
            long long R = a[y];
            
            if (x == y) {
			    if (k >= a[x]) { 
			        k -= a[x];
			        a[x] = 0;
			        x++;
			    } else { 
			        a[x] -= k;
			        k = 0;
			    }
			    break;
			}

            int l = (mm == 0 ? 1 : 0);
            int r = (mm == 1 ? 1 : 0);
            long long sl = 2 * L - l;
            long long sr = 2 * R - r;
            long long s = min(sl, sr);

            if (s > k) {
                k = 0;
                break;
            }
            long long ll = (s + l) / 2;
            long long rr = (s + r) / 2;

            a[x] -= ll;
            a[y] -= rr;
            k -= s;
            
            if (x <= y && a[x] <= 0) x++;
            if (x <= y && a[y] <= 0) y--;
            if (s % 2 == 1) mm ^= 1;
        }

        int sum = n - max(0, y - x + 1);
        cout << sum << '\n';
    }
}
```

### 思路
海妖左边一击，右边一击，这显然，左边会比右边提前一击；如果从右边开始打，右边会比左边提前一击，所以可以设置状态码mm：当mm是0时，左边提前一击，在很多情况下，就相当于比右边多一击；mm=1，右边同理。
这样，船要被打倒，就需要2*船耐受度-mm（mm=0，左边的船需要 $2*耐受度-1$ 下才会被打倒，[-1是因为此时左船提前打一下，也就是苦于少打一下]，此时，右边的船需要 $2*耐受度-0$ 下才会被打倒；mm=1，同理。

在这个原理下，l、r视为左右船的状态(是否多打一下)，通过 $sl = 2 * L - l$和$sr = 2 * R - r$，可以得到左右船要被打多少下才被击倒，此时sl、sr中较小的对应的船先倒下。
一条船被击倒后，就需要更新击打次数k和船的耐受度，$ll = (s + l) / 2$ 和 $rr = (s + r) / 2$ 分别表示左右船被击打的次数，l和r仍然是状态码，l=1，表示左船被多打一下。
这样更新好了各参数，就继续重新判断击打情况。

但要注意，若船耐受度为0，则要换一条船；若极大总次数k小于此次需要击打次数s，则直接k置零，船更新耐受度，但都不会倒下。

此外，还需注意状态码mm的更新逻辑，但此次击打次数s为奇数时，表示下次击打从另一边的船开始，所以需要更新状态码，mm^=1 。

时间复杂度：O(t * n)。
<br>
<br>

## 2. 别人的题解
#### (1)、直接将k分为K1，K2
```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
int t,n,k,k1,k2,h,a[200005],ans;
signed main() {
	cin>>t;
	while(t--){
		cin>>n>>k;
		k2=k/2;//由于除法是向下取整，海怪又是从前面开始，所以除法用在 k2 上。
		k1=k-k2;
		h=0,ans=0;//记得清零！
		for(int i=1;i<=n;i++) {
			cin>>a[i];
			if(k1>=a[i]){//为了节省一点时间就在读入的同时做了第一遍循环。
				k1-=a[i];
				h=i;
			}else{
				a[i]-=k1;
				k1=0;
			}
		}
		ans=h;
		for(int i=n;i>h;i--){
			if(a[i]<=k2) k2-=a[i],ans++;
			else break;
		}
		cout<<ans<<"\n";
	}
	return 0;
}

```

### 思路
我的算法思路是将销毁一只船作为一步，然后一步一步地判断是否销毁一只船或k结束。
而这个算法是一开始就将销毁左船和销毁右船作为两步，分开来讨论。

先攻击左船，所以左船比右船多0或1次攻击，所以先将总攻击次数k分为k1和k2，可能多一击的k1就是左船的攻击总次数，k2是右船的攻击总次数。

接着就直接用k1和k2攻击左右船，直至攻击次数用完。

攻击左船时，击倒的船的数量就是最后被击倒的船的索引(数组从1开始),右船每次被击倒一艘，总击倒数量++。
最终输出总计倒数量。

时间复杂度为 O(t * n)。

---

#### (2)、前缀和后缀和法(也进行了K的分配)
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=2e5+10;
long long n,k,a[N],pre[N],suf[N];
int main(){
	int T;
    cin>>T;
	while(T--){
		cin>>n>>k;
		long long ans=0,sum=0;
		for(int i=1;i<=n;++i){
			cin>>a[i];
			sum+=a[i];
		}
		if(k>=sum){  //特判，所有船都被摧毁
			cout<<n<<"\n";
			continue;
		}

		for(int i=1;i<=n;++i)   pre[i]=pre[i-1]+a[i]; //前缀和！

		for(int i=n;i>=1;--i)   suf[i]=suf[i+1]+a[i]; //后缀和！

		long long head=k/2,tail=k/2;
		if(k%2==1)++head;  //分配左右船的击打次数

		for(int i=1;i<=n;++i){  //利用前缀和判断左半部分船被击倒几艘
			if(head<pre[i]){
				ans=i-1;
				break;
			}
		}

		for(int i=n;i>=1;--i){  //利用后缀和判断右半部分船被击倒几艘
			if(tail<suf[i]){
				ans+=n-i;
				break;
			}
		}

		for(int i=1;i<=n;++i)pre[i]=suf[i]=0;  //将前缀和后缀和数组清零，防止对下一轮计算产生影响
		cout<<ans<<"\n";
	}
	return 0;
}
```

### 思路
和(1)逻辑相同，但在判断击倒几艘船时，使用了前缀和和后缀和直接进行遍历查表，而(1)是一点一点减，直至不够减。


