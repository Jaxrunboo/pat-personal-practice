##### 1110 Complete Binary Tree 
##### 分析：每次可以获得一个 坐标 和左子树坐标和右子树坐标
##### 判断是否是完全二叉树 是:返回树的最后一个节点坐标
##### 否:返回root坐标
##### => 如何根据给定的数据，把数据构建成树
##### 一个数组hasvalue[n]，根据输入结果判断是否是1，若1-n某一点不为1，即为根节点
##### 一个二维数组存储输入值
##### 若为完全二叉树，dfs后得的最大元素index会比节点总数大
##### 

```c# 
#include <iostream>

using namespace std;
int n;
int arr[100][2];
int maxn = 0;//最大index
int ans;//最大节点值在数组的index

void DFS(int root,int index)
{
    if(index>maxn)
    {
        maxn = index;
        ans = root;
    }
    if(arr[root][0]!= -1)
        DFS(arr[root][0],index *2);
    if(arr[root][1]!= -1)
        DFS(arr[root][1],index *2+1);
}

int main()
{
    cin>>n;
    int root = 0;
    int hasvalue[100] = {0};

    string left,right;
    for(int i=0; i<n; i++)
    {
        cin>>left>>right;
        if(left=="-")
        {
            arr[i][0]=-1;
        }
        else
        {
            arr[i][0] = stoi(left);
            hasvalue[arr[i][0]] = 1;
        }
        if(right=="-")
        {
            arr[i][1]=-1;
        }
        else
        {
            arr[i][1] = stoi(right);
            hasvalue[arr[i][1]] = 1;
        }
    }

    while(hasvalue[root]!=0)
        root++;
    DFS(root,1);

    if(maxn==n)
    {
        printf("YES %d",ans);
    }
    else
    {
         printf("NO %d",root);
    }

    return 0;
}

```