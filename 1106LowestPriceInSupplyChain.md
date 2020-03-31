##### 1106 Lowest Price in Supply Chain 
##### N<10^10，可以使用vector二维数组存储数据
##### 万一末尾节点没有零售商？没有在考虑范围内，按照结果来看，零售商应该是叶子节点
##### mindep表示层数  num表示叶节点个数
##### 需要注意结果精确位数


```c#
#include <cstdio>
#include <vector>
#include <cmath>

using namespace std;

int n;//节点数
vector<int> arr[100000];
int mindep = 99999999;
int num=1;

void DFS(int root,int depth)
{
    if(mindep<depth)
        return;
    if(arr[root].size() == 0)
    {
        if(mindep>depth)
        {
            mindep = depth;
            num =1;
        }
        else if(mindep== depth)
        {
            num++;
        }
    }

    for(int i=0; i<arr[root].size(); i++)
    {
        DFS(arr[root][i],depth+1);
    }
}

int main()
{
    int n;
    double p,r;
    scanf("%d %lf %lf",&n,&p,&r);
    for(int i=0; i<n; i++)
    {
        int ln;
        scanf("%d",&ln);

        for(int j=0; j<ln; j++)
        {
            int index;
            scanf("%d",&index);
            arr[i].push_back(index);
        }

    }

    DFS(0,0);//获取树

    printf("%.4f %d",p*pow((1+r/100),mindep),num);

    return 0;
}

```