##### 1147 Heaps
##### 分析：
##### 给定了固定个数，固定元素数的完全二叉树，且按照层序遍历给出
##### 判断其是什么堆？且按照后续遍历给出
##### 数目固定，使用二维数组（初始化为0），不存在空间浪费，后续遍历输出即可
##### 柳神版分析：思路大体相同，但相对而言，空间和时间利用率更佳
##### 1.判断堆类型时，采用n/2个循环计算
##### 2.重复利用vector<int>，不需要二维数组
> 自制版：
```c++
#include <iostream>

using namespace std;

int m =0;
int n=0;

int arr[100][1001];

void PrintfArr(int arrNew[],int index)
{
    if(index>n)
        return;
    PrintfArr(arrNew,index*2);
    PrintfArr(arrNew,index*2+1);
    cout<<arrNew[index];
    if(index>1)
    {
        cout<<" ";
    }
    else
    {
        cout<<endl;
    }
}

//-1 min 1 max 0 not
int Charge(int arrNew[][1001],int findex)
{
    bool isMin = true;
    bool isMax =  true;

    for(int i=2; i<n+1; i++)
    {
        if(arrNew[findex][i/2]>arrNew[findex][i])
            isMin = false;
        if(arrNew[findex][i/2]<arrNew[findex][i])
            isMax = false;
    }
    if(isMin&&!isMax)
        return -1;
    else if(!isMin&&isMax)
        return 1;
    else
        return 0;
}
int main()
{
    cin>>m>>n;
    for(int i=0; i<m; i++)
    {
        for(int j=1; j<n+1; j++)
        {
            cin>>arr[i][j];
        }
    }

    for(int i=0; i<m; i++)
    {
        int res = Charge(arr,i);
        if(res==1)
        {
            cout<<"Max Heap"<<endl;
        }
        else if(res ==-1)
        {
            cout<<"Min Heap"<<endl;
        }
        else
        {
            cout<<"Not Heap"<<endl;
        }
        PrintfArr(arr[i],1);
    }
    return 0;
}
```
> 柳神版:
```c++
#include <iostream>
#include <vector>
using namespace std;
int m, n;
vector<int> v;
void postOrder(int index)
{
    if (index >= n)
        return;
    postOrder(index * 2 + 1);
    postOrder(index * 2 + 2);
    printf("%d%s", v[index], index == 0 ? "\n" : " ");
}
int main()
{
    scanf("%d%d", &m, &n);
    v.resize(n);
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
            scanf("%d", &v[j]);
        int flag = v[0] > v[1] ? 1 : -1;
        for (int j = 0; j <= (n-1) / 2; j++)
        {
            int left = j * 2 + 1, right = j * 2 + 2;
            if (flag == 1 && (v[j] < v[left] || (right < n && v[j] <
                                                 v[right])))
                flag = 0;
            if (flag == -1 && (v[j] > v[left] || (right < n && v[j] >
                                                  v[right])))
                flag = 0;
        }
        if (flag == 0)
            printf("Not Heap\n");
        else
            printf("%s Heap\n", flag == 1 ? "Max" : "Min");
        postOrder(0);
    }
    return 0;
}

```
