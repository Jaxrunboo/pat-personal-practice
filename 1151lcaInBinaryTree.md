##### 1151 LCA in Binray Tree
##### 分析：
##### 1.给定了前序和中序，得到唯一的二叉树结构
##### 2.获得的请求lca的数据，逐条执行
##### 柳诺版本分析：
##### lca方法： 无需得出二叉树，根据前序和中序可以得到二叉树的root节点，相对复杂的点是root节点位置的计算
##### 1.若u和v在root的左侧，获取左侧子二叉树的root节点，重新使用u、v进行比较
##### 2.若u和v在root的两侧，则u、v的lca为root
##### 3.若u和v在root的右侧，获取右侧子二叉树的root节点，重新使用u、v进行比较
##### 4.若u或者v不存在，输出u、v不存在的结论
##### 使用两个vector分别存储前序和中序，使用一个map<value,index>来存储前序的值在中序中对应的位置
> 柳神版代码：
```c++
#include <iostream>
#include <vector>
#include <map>

using namespace std;

vector<int> pre,in;
map<int,int> pos;
int m,n,a,b;

void lca(int inl,int inr,int preRoot,int a,int b)
{
    if(inl>inr)
        return;
    int rootIndex = pos[pre[preRoot]];
    int aIndex = pos[a];
    int bIndex =pos[b];
    if(aIndex<rootIndex&&bIndex< rootIndex)
        lca(inl,rootIndex-1,preRoot+1,a,b);
    else if((aIndex<rootIndex&&bIndex>rootIndex)||(aIndex>rootIndex&&bIndex<rootIndex))
        printf("LCA of %d and %d is %d.\n",a,b,pre[preRoot]);
    else if(aIndex>rootIndex&&bIndex> rootIndex)
        lca(rootIndex+1,inr,preRoot+1+(rootIndex-inl),a,b);
    else if(aIndex==rootIndex)
        printf("%d is an ancestor of %d.\n",a,b);
    else if(bIndex == rootIndex)
        printf("%d is an ancestor of %d.\n",b,a);
    else
        return;
}

int main()
{
    cin>>m>>n;
    pre.resize(n+1);
    in.resize(n+1);
    int arr[m][2];

    for(int i=1; i<=n; i++)
    {
        cin>>in[i];
        pos[in[i]]=i;
    }
    for(int i=1; i<=n; i++)
    {
        cin>>pre[i];
    }
    for(int i=0; i<m; i++)
    {
        cin>>arr[i][0]>>arr[i][1];
    }
    for(int i=0; i<m; i++)
    {
        a=arr[i][0];
        b=arr[i][1];
        if(pos[a]==0&&pos[b]==0)
            printf("ERROR: %d and %d are not found.\n",a,b);
        else if(pos[a]==0||pos[b]==0)
            printf("ERROR: %d is not found.\n",pos[a]==0?a:b);
        else
            lca(1,n,1,a,b);
    }
    return 0;
}

```