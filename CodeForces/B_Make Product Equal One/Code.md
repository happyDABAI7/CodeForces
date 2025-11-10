# 题解

## 1. 我的题解
```
#include<bits/stdc++.h>
using namespace std;

int main(){
	int n;
	long long count=0;
	cin>>n;
	int i=n;
	vector<int> arr(n);
	vector<int> arrF; 
	vector<int> arrZ; 
	vector<int> arrD; 
	while(i--){
        cin>>arr[i]; 
        if(arr[i]<0) arrF.push_back(i);
        if(arr[i]==0) arrZ.push_back(i);
        if(arr[i]>1) arrD.push_back(i);
    }
    int c1=arrF.size();
    int c2=arrZ.size();
    int c3=arrD.size();
    if(c1!=0&&c2!=0){
    	count+=c2;
		while(c1--){
			count+=(-1-arr[arrF[c1]]);
		}
	}
	else if(c1!=0&&c2==0){
		int j=c1;
		while(c1--){
			count+=(-1-arr[arrF[c1]]);
		}
		if(j%2!=0){
			count+=2;
		}
	}
	else if(c1==0&&c2!=0){
		count+=c2;
	}
	while(c3--){
		count+=(arr[arrD[c3]]-1);
	}	
    
	cout<<count<<'\n';
}
```

### 思路
要使 a_1·a_2·...·a_n = 1，则对于大于1的数字要减至1；小于0的数字要增至-1；0根据需求变成1或-1；若没有0，则负数要根据需求判断是否有一个数要变成1。
因此，思路就是分类讨论：
首先，无论是否有正数，对分类讨论无影响，正数的处理方法都是 count 加上正数大于 1 的部分。
若存在0和负数，则count在加上正数大于1的部分和负数小于-1的部分的绝对值后，再加上0的数量；
若没有0，count在加上正数大于1的部分后，再加上负数小于-1的部分的绝对值，若负数的数量为奇数，则最终count需再 +2；
若没有负数，则count在加上正数大于1的部分后，再加上0的数量。

时间复杂度是 O(n)。
<br>
<br>

## 2. 别人的题解
### （1）相当于我的思路的优化版，将分组讨论中共同点提前统一处理，再根据不同情况if-else处理。
```
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int n;
	cin>>n;
	long long a[n+5],sum=0,ans=0,o=0;
	for(int i=1;i<=n;i++)
	{
		cin>>a[i];
		if(a[i]<=-1) //这里要写小于等于避免类似第一组样例的情况
		{
			sum+=abs(a[i]+1);
			ans++;
		}
		else if(a[i]==0)
		sum++,o++;
		else if(a[i]>1)
		sum+=a[i]-1;
	}
	if(ans%2==1&&o==0)
	cout<<sum+2<<endl;
	else
	cout<<sum<<endl;
	return 0;
}
```

### 思路：
直接负数改 −1，0 改 1，正数改成 1。
接着根据负数数量奇偶和是否存在0，进行特殊讨论，只有负数数量为奇数且没有0时，结果需要 +2 。

时间复杂度是 O(n)。
