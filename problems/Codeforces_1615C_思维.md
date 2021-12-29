## Codeforces_1615C Menorah

#### 链接

https://codeforces.com/contest/1615/problem/C

#### 题干

两个相同长度的只有0和1的字符串q和w，将“选定字符串中的某个1以外的所有字符翻转，其中翻转定义为0→1.1→0”作为一次操作，最少对q做几次操作能让q变为w

#### 思路

从题干可得，字符串中字符的顺序是无意义的，只要input与target字符串对应的字符匹配即可，两个字符串可以写作n个二元组，并且所有二元组都属于[(1,1),(1,0),(0,0),(0,1)]，可以将二元组的个数以此形式统计为P=[A,B,C,D]。从题目可以知道只要让P变化为[X,0,Y,0]就可以达到题目要求。而题目中的"操作"，对于抽象出来的问题可以等价于两个变换

func1:[A,B,C,D]→[D+1,C,B,A-1]

func2:[A,B,C,D]→[D,C+1,B-1,A]

从题干里可以得到该题目字符串长度为10^5，不能直接使用bfs对两种变换进行搜索，但是该题目存在一个天然剪枝，既func1与func2只能交替使用，否则在搜索树上会成环，因此实际上迭代下去变换结果只有两个，分别为先进行func1的结果和先进行func2的结果，可以设定存在元素小于0为终止条件。

#### Input

The first line contains an integer t (1≤t≤10^4) — the number of test cases. Then t cases follow.

The first line of each test case contains a single integer n 1≤n≤10^5) — the number of candles.

The second line contains a string a of length n consisting of symbols 0 and 1 — the initial pattern of lights.

The third line contains a string b of length n consisting of symbols 0 and 1 — the desired pattern of lights.

It is guaranteed that the sum of n does not exceed 10^5.

#### Output

For each test case, output the minimum number of operations required to transform aa to bb, or −1−1 if it's impossible.

#### **Example**

input

```
5
5
11010
11010
2
01
11
3
000
101
9
100010111
101101100
9
001011011
011010101
```

output

```
0
1
-1
3
4
```

#### 题解代码

```c++
#include<bits/stdc++.h>
using namespace std;
vector<int> func1(vector<int> input)
{
    vector<int> res = {input[3]+1, input[2], input[1], input[0]-1};
    return res;
}
vector<int> func2(vector<int> input)
{
    vector<int> res = {input[3], input[2]+1, input[1]-1, input[0]};
    return res;
}
int main()
{
    int T;
    cin>>T;
    while(T--)
    {
        int n;
        cin>>n;
        string q,w;
        cin>>q;
        cin>>w;
        vector<int>tup;
        for(int a=1;a<=4;a++)tup.push_back(0);
        for(int a=0;a<n;a++)
        {
            if(q[a]=='0')
            {
                if(w[a]=='0')tup[2]++;
                else tup[3]++;
            }
            else
            {
                if(w[a]=='0')tup[1]++;
                else tup[0]++;
            }
        }
        int fg = 0;
        int res = 0x3f3f3f3f;
        int count = 0;
        vector<int>tmp = tup;
        if(tmp[1]==0 && tmp[3]==0)
        {
            cout<<0<<endl;
            continue;
        }
        while(tmp[0] >= 0 && tmp[1] >= 0 &&tmp[2] >= 0 &&tmp[3] >= 0)
        {
            if(count%2 ==0)tmp = func1(tmp);
            else tmp = func2(tmp);
            count++ ;
            if(tmp[1]==0 && tmp[3]==0)
            {
                fg = 1;
                res= min(res, count);
                break;
            }
        }
        count = 0;
        tmp = tup;
        while(tmp[0] >= 0 && tmp[1] >= 0 &&tmp[2] >= 0 &&tmp[3] >= 0)
        {
            if(count%2 ==1)tmp = func1(tmp);
            else tmp = func2(tmp);
            count++ ;
            if(tmp[1]==0 && tmp[3]==0)
            {
                fg = 1;
                res= min(res, count);
                break;
            }
        }
        if(fg)cout<<res<<endl;
        else cout<<-1<<endl;
    }
}
```

#### 运行结果

| [C - Menorah](https://codeforces.com/contest/1615/problem/C) | GNU C++17 | **Happy New Year!** | 77 ms | 400 KB |
| ------------------------------------------------------------ | --------- | ------------------- | ----- | ------ |

