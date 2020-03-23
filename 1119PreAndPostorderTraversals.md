##### 1119 Pre and Post-order Traversals 
##### 分析：已知先序和后序，求中序
##### 需要知道，如何判断这个中序是否不唯一.先序的首位和后序的末位相等，为根元素。之后先序后推一位，后序前推一位。当两值相等,此时便无法区分到底是左子树还是右子树
##### 即不能够唯一断定，某子节点是左节点还是右节点
##### vector存储pre，post，in
##### 算法中自定义出现不唯一时认定其为左子树

```c++
#include <iostream>
#include <vector>

using namespace std;

int n;
vector<int> pre,post,in;
bool uniqueT = true;

void Charge(int preL,int preR,int postL,int postR)
{
    if(preL==preR)
    {
        in.push_back(pre[preL]);
        return;
    }

    //两端相等也认定为左子树
    if(pre[preL]==post[postR])
    {
        //根节点确定，考虑子树情况
        if(pre[preL+1]== post[postR-1])
        {
            uniqueT = false;
            //意味着剩下的全是左子树的内容
            Charge(preL+1,preR,postL,postR-1);//左
            in.push_back(pre[preL]);//根
        }
        else
        {
            int i=0;
            while(pre[preL+i]!=post[postR-1])
            {
                i++;
            }
            i=i-1;
            Charge(preL+1,preL+i,postL,postL+i-1);//左子树
            in.push_back(pre[preL]);//根节点
            Charge(preL+i+1,preR,postL+i,postR-1);//右子树
        }
    }
}

int main()
{
    cin>>n;
    pre.resize(n+ 1);
    post.resize(n+1);
    for(int i=1; i<=n; i++)
    {
        cin>>pre[i];
    }
    for(int i=1; i<=n; i++)
    {
        cin>>post[i];
    }
    //判断并存储
    Charge(1,n,1,n);
    //输出
    if(uniqueT)
    {
        cout<<"Yes"<<endl;
    }
    else
    {
        cout<<"No"<<endl;
    }
    cout<<in[0];
    for(int i=1; i<in.size(); i++)
    {
        cout<<" "<<in[i];
    }
    cout<<endl;
    return 0;
}
```