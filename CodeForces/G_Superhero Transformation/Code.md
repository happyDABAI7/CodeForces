# 题解

## 1. 我的题解
```
#include<bits/stdc++.h>
using namespace std;

int main(){
	string t,s;
	cin>>s>>t;
	
	int d1=s.length(),d2=t.length();
	if(d1!=d2){
		cout<<"No\n";
		return 0;
	}
	bool ok = true;
	while(d1--){
		if((s[d1]=='a' || s[d1]=='e' || s[d1]=='i' || s[d1]=='o' || s[d1]=='u')
		&& (t[d1]=='a' || t[d1]=='e' || t[d1]=='i' || t[d1]=='o' || t[d1]=='u')){
			continue;
		}
		else if((s[d1]=='a' || s[d1]=='e' || s[d1]=='i' || s[d1]=='o' || s[d1]=='u')
		|| (t[d1]=='a' || t[d1]=='e' || t[d1]=='i' || t[d1]=='o' || t[d1]=='u')){
			ok = false;	
			break;
		}
	}
	
	if(ok) cout<<"Yes\n";
	else cout<<"No\n";
}
```

### 思路
利用 ok 标记，加上遍历
首先，判断s和t长度是否相等，不相等直接No；
然后，同时遍历s和t字符串，若两者都是元音或都不是元音，则继续遍历；反之，ok标记变为false，并不再遍历。
最后，根据ok标记，输出对应结果。

这段代码的时间复杂度是 O(n)，其中 n 是字符串 s（或 t）的长度。
<br>
<br>

## 2. 别人的题解
相比我的题解，就是将是否同为元音或非元音封装成函数或宏定义
```
#include<stdio.h>
#include<string.h>
#define judge(x) (x=='a'||x=='e'||x=='i'||x=='o'||x=='u')
//宏定义：判断x是否为元音。是返回1，否返回0. 
char s[1010],t[1010];int l;
int main(int argc,char*argv[]){
	gets(s);
	gets(t);
	//使用gets获取输入，但并不提倡各位使用gets. 
	if(strlen(s)!=strlen(t)){
		//判断s,t位数是否相同 
		printf("No");
		return 0;
	}
	l=strlen(s);
	for(int i=0;i<l;i++){
		//遍历字符串 
		if(judge(s[i])!=judge(t[i])){
			//如果不同为元音或辅音，返回"No"
			printf("No");
			return 0;
		}
	}
	printf("Yes");  //没有任何问题 
	return 0;
}
```

### 思路
将是否为元音封装成宏定义，并返回true or false，然后通过判断 judge(s[i])!=judge(t[i]) 来判断是否同为元音或非元音，最后得到结果。

这段代码的时间复杂度是 O(n)，其中 n 是字符串 s（或 t）的长度。
