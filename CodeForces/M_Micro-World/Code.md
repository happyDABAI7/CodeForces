# 题解

## 1. 我的题解
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	ios::sync_with_stdio(false);
    cin.tie(NULL);
	
    int n, k;
    cin >> n >> k;
	int i=n; 
	vector<int> arr; 
	while(i--){
		int t; 
		cin>>t; 
		arr.push_back(t); 
	}
    sort(arr.begin(), arr.end());
    
    int count = 0;
    int j = 0;
    for (int m = 1; m < n; m++) {
    	int cc=1;
		while(arr[m]==arr[j] && m < n){
			cc++;
			m++;
		} 
		if (m == n) break; 
        if (arr[m] > arr[j] && arr[m] <= arr[j] + k) {
        	count+=cc;
        } 
        j = m;
    }
    cout << n-count << endl;
    return 0;
}
```

### 思路
先排序，这样可以通过后面的数字判断前面的数字是否可被覆盖。
如12345，k=1，则从2开始遍历，2可覆盖1，count++；下一个，3可覆盖2，count++......直至数组遍历完。
但这样忽略了一个问题，若数组是11234，k=1，那最终会剩下14，这不对，所以对重复数字需要特殊计数，先计算重复数字的个数，后续若可被覆盖，则统一计入count。

最终，count计入的是可被删除的数字数量，所以，最终结果就是n-count。

```
开始  
│  
├─ 读取 n、k，读取数组 arr  
│  
├─ 对 arr 排序（使相同元素相邻）  
│  
├─ 初始化 count=0，j=0（当前组起点）  
│  
├─ m 从 1 遍历到 n-1：  
│  │  
│  ├─ 统计当前组（j 开始）的元素个数 cc（相同元素连续计数）  
│  │  （通过 m 后移，直到 arr[m] != arr[j]）  
│  │  
│  ├─ 若下一组元素 arr[m] 在 [arr[j]+1, arr[j]+k] 范围内：  
│  │  └─ count += cc（当前组元素可被覆盖）  
│  │  
│  └─ 更新 j = m（移至下一组起点）  
│  
├─ 输出 n - count（未被覆盖的元素数）  
│  
结束  
```

时间复杂度：O(n log n)。
<br>
<br>

## 2. 别人的题解
### (1)、利用栈

```cpp
#include <bits/stdc++.h>
using namespace std ;
int n,a[200100],k;
vector<int>q;
int main() {
	cin>>n>>k;
	for(int i=1; i<=n; i++) cin>>a[i];
	sort(a+1,a+1+n); //排序 
	for(int i=1; i<=n; i++) {
		while(!q.empty()&&a[i]-q.back()<=k&&a[i]>q.back()) 
			q.pop_back(); //判断是否满足要求，满足则将他踢出 
		q.push_back(a[i]); //加入a[i] 
	}
	cout<<q.size()<<endl; //输出个数 
	return false;
}
```

### 思路
元素入栈，后续元素判断能否吞噬栈尾元素，若能则栈尾元素出栈，并循环，直至新元素不能吞噬栈尾元素或栈空，此时新元素入栈。

时间复杂度：O(n log n)。

#### 上述代码用数组模拟栈，也可用Stack栈结构
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 2e5 + 25;
int n, k;
int a[N];
stack<int> st;

int main() {
    // 关闭同步流，加速cin/cout（处理大数据时必要）
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    
    sort(a + 1, a + n + 1);  // 排序数组
    
    for (int i = 1; i <= n; i++) {
        // 若栈顶元素可被当前元素a[i]覆盖（满足a[i] > 栈顶且a[i] <= 栈顶 + k），则弹出栈顶
        while (!st.empty() && st.top() < a[i] && a[i] <= st.top() + k) {
            st.pop();
        }
        st.push(a[i]);  // 将当前元素入栈
    }
    
    cout << st.size() << endl;  // 栈的大小即为结果
    return 0;
}
```

