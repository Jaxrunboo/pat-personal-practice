##### 1115 Counting Nodes in a BST
##### 分析：左子树小于父节点，右子树大于父节点
##### 链表存储自定义节点，存储bst，递归构建
##### 由根节点开始，dfs节点，并累计不同深度的系欸但数量

```c#
#include <iostream>
#include <vector>

using namespace std;
int n;//<=1000
struct Node
{
    int value;
    struct Node *left,*right;
};

Node* Build(struct Node *root,int value)
{
    if(root==NULL)
    {
        root = new Node();
        root->value = value;
        root->left = NULL;
        root->right = NULL;
    }
    else if(value <= root->value)
    {
        root->left=  Build(root->left,value);
    }
    else
    {
        root->right = Build(root->right,value);
    }
    return root;
}

vector<int> arr(1001);
int maxDepth = 0;

void DFS(struct Node *root,int depth)
{
    if(root==NULL)
    {
        maxDepth = max(maxDepth,depth);
        return ;
    }
    arr[depth]++;
    DFS(root->left,depth+1);
    DFS(root->right,depth+1);
}

int main()
{
    cin>>n;
    int t;
    Node *root = NULL;
    for(int i=1; i<=n; i++)
    {
        cin>>t;
        root = Build(root,t);
    }
    DFS(root,1);
    int b = arr[maxDepth-1];
    int a = arr[maxDepth-2];

    for(int i=0;i<arr.size();i++)
    {
        cout<<arr[i]<<endl;
    }
    printf("%d + %d = %d",b,a,a+b);

    return 0;
}

```