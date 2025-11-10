# 题解

## 1. 我的题解
```
#include<bits/stdc++.h>
using namespace std;

int main() {
	int t,n;
	cin>>t;
	while(t--){
		cin>>n;
		int m;
		vector<int> arr; 
		arr.push_back(n);
		arr.push_back(n-1);
		m=ceil((2*n+1)/2);
		n=n-2;
		while(m!=2){
			arr.push_back(m);
			arr.push_back(n);
			m=ceil((m+n+1)/2);
			n--;
		}
		cout<<m<<'\n';
		int i=0;
		while(i<arr.size()){
			cout<<arr[i]<<'\t'<<arr[i+1]<<'\n';
			i+=2;
		}
	} 	
}
```

### 思路
通过自行举例，得到，最终剩余的数字一定是2。
> 为什么 2 是最小答案呢？我们假设存在更小的答案，则该答案只能为 1，因为上取整不可能取到 0。然后我们发现，1 只能由 (1+1)/2 得到，一旦出现一个大于 1 的数，合并后的结果一定也会大于 1，因此不存在答案为 1 的情况，所以 2 就是最小答案。

那么，我们从右往左，依次取两个数，计算并向上取整得到m，m不为2时，说明还没得到最终结果，则继续用m和下一个n计算，得到新m；循环往复，直至m==0。
> 为什么不需要在得到新m的时候排序？因为右边的数是较大的数，两个较大的数相加除以2，并向上取整，一定大于其他较小的数。

时间复杂度是 O(t * n)。
<br>
<br>

## 2. 别人的题解
#### (1)、对我的题解进行改进，因为从右往左取数字，得到的中间值m有一些个规律：
第一组 n和n-1
后续，令i=n-1，并每输出一组，就i--
后续组 i-1和i+1
```
#include <bits/stdc++.h>
using namespace std;
int main()
{
	int t,n;
	cin>>t;
	while(t--)
	{
		cin>>n;
		cout<<2<<endl;//最后剩下的数
		for(int i=n;i>=2;i--)//从大往小取
		{
			if(i==n)//取n和n-1
			{
				cout<<i-1<<' '<<i<<endl;
			}
			else cout<<i-1<<' '<<i+1<<endl;//取n和n-2
		}
	}
    return 0;
}
```
### 思路
也是从右往左取数字，但没有算中间值m部分。

时间复杂度是 O(t * n)。
<br>

#### (2)、也是从右往左取数字，但前两组数字的取法不同
```
#include<iostream>
#include<cstdio>
using namespace std;
int T,n;
int main()
{
	scanf("%d",&T);
	while(T--)
	{
		scanf("%d",&n);
		printf("2\n");
		if(n==2) {printf("1 2\n");continue;}
		printf("%d %d\n",n,n-2);
		printf("%d %d\n",n-1,n-1);
		for(int i=n-3;i>=1;i--)
		printf("%d %d\n",i+2,i);
	}
	return 0;
}
```

### 思路
第一组 n和n-2
第二组 n-1和n-1
后续，令i=n-3，并每输出一组，就i--
后续组 i和i+2


