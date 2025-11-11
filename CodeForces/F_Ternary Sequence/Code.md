# 题解

## 1. 我的题解
```
#include<bits/stdc++.h>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--){
		int x1,y1,z1,x2,y2,z2;
		cin>>x1>>y1>>z1;
		cin>>x2>>y2>>z2;
		int sum=0;
		
		if(z1>=y2) {
			sum+=2*y2;
			z1-=y2;
			if(y1>=x2){
				y1-=x2;
				sum-=2*y1;
			} 
		}
		else {
			sum+=2*z1;
			y2-=z1;
			if(x1>=z2){
				sum=sum;
			}
			else{
				z2-=x1;
				sum-=2*z2;
			}
		}
		
		cout<<sum<<'\n';
	}	
}
```

### 思路
根据题目，我们希望最后的和最大，那么就需要尽量让 $a_i > b_i$ 尽可能多， $a_i < b_i$ 尽可能少。
此外，和0相乘时，三个式子结果都是0，所以，对于$a_i > b_i$，我们就先看$a_i=2$，$b_i=1$，这样可以得到有效的正数；对比后，无论$a_i=2$还有剩，都要先把$b_i=2$先尽可能多地去掉，只有$a_i=0$可以有效去除，所以，对比$a_i=0$和$b_i=2$的数量。
这样结果会有四种可能：
$a_i=0$，$a_i=1$，$a_i=2$，$b_i=0$ (1)；
$a_i=1$，$a_i=2$，$b_i=0$，$b_i=2$ (2)；
$a_i=0$，$a_i=1$，$b_i=0$，$b_i=1$ (3)；
$a_i=1$，$b_i=0$，$b_i=1$，$b_i=2$ (4)。
对于情况(1)和(3)，sum结果不变，对于情况(2)和(4)，需要减去$a_i=1 < b_i=2$而产生的负数。
最终得到结果。

时间复杂度是 O(t)。
<br>
<br>

## 2. 别人的题解
### 贪心+构造
```
#include<iostream>
using namespace std;
int T;
int min(int a,int b){
	if(a<b) return a;
	else return b;
}
int main(){
    cin>>T;
    while(T--){
    	int x1,y1,z1,x2,y2,z2;
        cin>>x1>>y1>>z1>>x2>>y2>>z2;
        int min1=min(x1,z2);
        x1-=min1;z2-=min1;
        int min2=min(z1,z2);
        z1-=min2;z2-=min2;
        int ans=(min(y2,z1)-min(y1,z2))*2;
        cout<<ans<<endl;
    }
}
```


### 思路
只有 (2,1) 会产生正贡献，只有 (1,2) 会产生负贡献。
所以贪心，最大化正贡献，最小化负贡献。
所以就用 A 的 0 抵掉 B 的 2，再用A 的 2 抵掉 B 的 2，简单来说，就是尽可能使用A的0和2有效抵消掉B的2，这样剩下的就是正贡献和必存在的负贡献。

时间复杂度是 O(t)。
