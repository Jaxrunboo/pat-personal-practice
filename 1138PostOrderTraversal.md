##### 分析：已知前序和中序，求后序第一个应输出节点
##### 1.无需先得到整体的二叉树，可递归求得最左侧&&最底层的节点
##### 2.使用vector<int>存储先序、中序，使用map<int,int>存储中序index和value
##### 3.对先序的值逐个操作，与中序比对，对应处理
##### 柳神版分析：
##### 思路大致相同，关键点判断不同，其只采用两个vector存储先序中序，添加一个flag标签判断结束时机
##### 自制版的思路来源是柳神的 1151LcaInBinaryTree,柳神版不使用map速度更快，空间占用小
> 自制版
```c++
#include <iostream>
#include <vector>
#include <map>

using namespace std;

int n;//n<=50000
vector<int> pre,in;
map<int,int> pos;

void deal(int preRootIndex,int leftIndex,int rightIndex )
{
    int inIndex = pos[pre[preRootIndex]] ;
    if(inIndex-leftIndex>0)
    {
        preRootIndex++;
        rightIndex = inIndex-1;
        deal(preRootIndex,leftIndex,rightIndex);
    }
    else if(inIndex-rightIndex<0)
    {
        preRootIndex++;
        leftIndex = inIndex +1;
        deal(preRootIndex,leftIndex,rightIndex);
    }
    else
    {
        cout<<pre[preRootIndex];
    }
}

int main()
{
    cin>>n;
    if(n==0) return 0;
    pre.resize(n);
    in.resize(n);
    for(int i=0; i<n; i++)
    {
        cin>>pre[i];
    }
    for(int i=0; i<n; i++)
    {
        cin>>in[i];
        pos[in[i]] = i;
    }

    deal(0,0,n-1);
    return 0;
}
```
> 柳神版：
```c++
#include <iostream>
#include <vector>
using namespace std;
vector<int> pre, in;
bool flag = false;
void postOrder(int prel, int inl, int inr)
{
    if (inl > inr || flag == true)
        return;
    int i = inl;
    while (in[i] != pre[prel])
        i++;
    postOrder(prel+1, inl, i-1);
    postOrder(prel+i-inl+1, i+1, inr);
    if (flag == false)
    {
        printf("%d", in[i]);
        flag = true;
    }
}
int main()
{
    int n;
    scanf("%d", &n);
    pre.resize(n);
    in.resize(n);
    for (int i = 0; i < n; i++)
        scanf("%d", &pre[i]);
    for (int i = 0; i < n; i++)
        scanf("%d", &in[i]);
    postOrder(0, 0, n-1);
    return 0;
}

```