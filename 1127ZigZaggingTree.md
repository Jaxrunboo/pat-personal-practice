##### 1127 ZigZagging on a Tree
##### 分析：已知中序和后序，可得一个唯一的二叉树,对其按照层次，之字形输出结果
##### 1.根据中序和后序建树，保存在二维数组中
##### 2.层序遍历要使用队列来实现
##### 3.还需要一个二维数组来存储每层的结果，用来实现zigzag

```c++

#include <iostream>
#include <vector>
#include <queue>

using namespace std;
vector<int> in,post,result[35];
int tree[35][2],n,rootIndex;

struct Node
{
    int index;
    int depth;
};

void dfs(int &index,int inLeftIndex,int inRightIndex,int postLeftIndex,int postRightIndex)
{
    if(inLeftIndex>inRightIndex)
        return;
    index = postRightIndex;
    //inIndex
    int i=1;
    while(in[i]!=post[postRightIndex])
        i++;
    dfs(tree[index][0],inLeftIndex,i-1,postLeftIndex,index-(inRightIndex - i)-1);
    dfs(tree[index][1],i+1,inRightIndex,index-(inRightIndex - i),postRightIndex-1);
}

void bfs()
{
    //使用队列，进行层序遍历
    //当前数组还是一个post的顺序，将其变为层序，保存到result中
    queue<Node> q ;
    q.push(Node{rootIndex,0});
    while(!q.empty())
    {
        Node node =  q.front();
        q.pop();
        result[node.depth].push_back(post[node.index]);
        if(tree[node.index][0]!=0)
            q.push(Node{tree[node.index][0],node.depth+1});
        if(tree[node.index][1]!=0)
            q.push(Node{tree[node.index][1],node.depth+1});
    }
}

int main()
{
    cin>>n;
    in.resize(n+1);
    post.resize(n+1);
    for(int i=1; i<=n; i++)
    {
        cin>>in[i];
    }
    for(int i=1; i<=n; i++)
    {
        cin>>post[i];
    }
    //深搜获取唯一二叉树
    dfs(rootIndex,1,n,1 ,n);
    //广搜得到层次结构
    bfs();
    //按要求输出
    printf("%d",result[0][0]);
    for(int i=1;i<35;i++)
    {
        if(i%2 ==1)
        {
            for(int j=0;j<result[i].size();j++)
            {
                printf(" %d",result[i][j]);
            }
        }
        else
        {
            for(int j=result[i].size()-1;j>=0;j--)
            {
                printf(" %d",result[i][j]);
            }
        }
    }
    return 0;
}

```