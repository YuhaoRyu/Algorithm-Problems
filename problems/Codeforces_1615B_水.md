## Codeforces_1615B And It's Non-Zero

#### 链接

[https://codeforces.com/contest/1615/problem/B]()

#### 题干

删掉最少数量的数字使得[l,r]区间内的数进行按位与后不为0

#### 思路

统计[l,r]内的数字按照二进制拆分后，每一位的前缀和，可以得知区间内只要有一位按位与不为0即可，所以位为0的前缀和最少的位数即为所求

#### Input

The first line contains one integer t (1≤t≤10^4) — the number of test cases. Then t cases follow.

The first line of each test case contains two integers l and r (1≤l≤r≤2⋅10^5) — the description of the array.

#### Output

For each test case, output a single integer — the answer to the problem.

#### **Example**

input

```
5
1 2
2 8
4 5
1 5
100000 200000
```

output

```
1
3
0
2
31072
```

#### 题解代码

```c++
#include<bits/stdc++.h>
using namespace std;
int bin2[200001][30];
int main()
{
    for(int a=1;a<=200000;a++)
    {
        for(int b=1;b<30;b++)bin2[a][b] += bin2[a-1][b];
        int tmp = a;
        int b_ = 1;
        while(tmp)
        {
            if(tmp%2)bin2[a][b_] += 1;
            b_++;
            tmp >>= 1;
        }
    }
    int T;
    cin>>T;
    while(T--)
    {
        int l,r;
        cin>>l>>r;
        int res = 0x3f3f3f3f;
        for(int a=1;a<30;a++)
        {
            res = min(r - l + 1 - bin2[r][a] + bin2[l - 1][a], res);
        }
        cout<<res<<endl;
    }
}
```

#### 运行结果

| [B - And It's Non-Zero](https://codeforces.com/contest/1615/problem/B) | GNU C++17 | **Happy New Year!** | 78 ms | 23500 KB |
| ------------------------------------------------------------ | --------- | ------------------- | ----- | -------- |
