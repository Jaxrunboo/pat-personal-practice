### heap
#### 1155 想要判断给出的binary tree是否是一个heap
##### 自作分析： 根据root和第二个节点就可以假设tree是最大堆（最小堆），
##### 然后，采用递归的方式，父元素与子元素进行比较，父元素>=(<=)子元素，可继续比较,直到子元素为空。
##### 若不满足情况，则认为非最大堆（最小堆）
##### 柳婼版分析：
##### 1. 数组存储堆，使用vector，深搜回溯，直接显示paths
##### 2. 根据index，对数组数据循环判断堆的类型

> 自制版：
```c++
#include <iostream>
#include <stack>

using namespace std;

typedef int Index;
struct HeapNode
{
    int value;
    Index leftChildIndex;
    Index rightChildIndex;
};

//结果1:与increase顺序相同 -1:表示不同
int ChargeAndShowOnePath(Index index,HeapNode arr[],int increase)
{
    /*1.判断是否是叶子节点
    2.向上查找父节点，同时将index入栈
    3.出栈时比较大小，作为大小判断的结果*/
    stack<int> paths;
    int firstNum;
    int secondNum;
    int res =1;

    do
    {
        paths.push(index);
        index/=2;
    }
    while(index>0);

    if(!paths.empty())
    {
        firstNum = paths.top();
        paths.pop();
        cout<<arr[firstNum].value<<" ";
    }

    while(!paths.empty())
    {
        secondNum = paths.top();
        paths.pop();
        if(increase==1&& arr[secondNum].value>arr[firstNum].value)
        {
            res= -1;
        }
        else if(increase==-1&& arr[secondNum].value<arr[firstNum].value)
        {
            res= -1;
        }

        cout<<arr[secondNum].value;
        if(!paths.empty())
        {
            cout<<" ";
        }
        else
        {
            cout<<endl;
        }
        firstNum = secondNum;
    }
    return res;
}

void FindTreeChild(stack<int> &s,HeapNode arr[],int index)
{
    if(arr[index].leftChildIndex==-1&&arr[index].rightChildIndex==-1)
    {
        s.push(index);
    }
    else
    {
        if(arr[index].leftChildIndex!=-1)
        {
            FindTreeChild(s,arr,index*2);
        }
        if(arr[index].rightChildIndex!=-1)
        {
            FindTreeChild(s,arr,index*2+1);
        }
    }
}

int main()
{
    int heapEleNum;
    cin>>heapEleNum;
    HeapNode arr[heapEleNum+1];
    int i=1;
    int value;
    int increase;
    int obey =1 ;
    stack<int> indexS;
    //stack<int> &indexS1 = indexS;

    while(i<=heapEleNum)
    {
        cin>>value;
        arr[i].value = value;
        arr[i].leftChildIndex = 2*i>heapEleNum?-1:2*i;
        arr[i].rightChildIndex = (2*i+1)>heapEleNum?-1:(2*i+1);
        i++;
    }

    FindTreeChild(indexS,arr,1);

    increase = arr[1].value> arr[2].value? 1:(arr[1].value<arr[2].value?-1:1) ;

    while(!indexS.empty())
    {
        int needindex = indexS.top();
        indexS.pop();
        int result  = ChargeAndShowOnePath(needindex,arr,increase);
        if(result==-1)
        {
            obey = -1;
        }
    }
    if(obey==1)
    {
        if(increase==1)
        {
            cout<<"Max Heap"<<endl;
        }
        else
        {
            cout<<"Min Heap"<<endl;
        }
    }
    else
    {
        cout<<"Not Heap"<<endl;
    }

    return 0;
}

```
> 柳婼版:
```c++
#include <iostream>
#include <vector>
using namespace std;
vector<int> v;
int a[1009], n, isMin = 1, isMax = 1;
void dfs(int index)
{
    if (index * 2 > n && index * 2 + 1 > n)
    {
        if (index <= n)
        {
            for (int i = 0; i < v.size(); i++)
                printf("%d%s", v[i], i != v.size() - 1 ? " " : "\n");
        }
    }
    else
    {
        v.push_back(a[index * 2 + 1]);
        dfs(index * 2 + 1);
        v.pop_back();
        v.push_back(a[index * 2]);
        dfs(index * 2);
        v.pop_back();
    }
}
int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        scanf("%d", &a[i]);
    v.push_back(a[1]);
    dfs(1);
    for (int i = 2; i <= n; i++)
    {
        if (a[i/2] > a[i])
            isMin = 0;
        if (a[i/2] < a[i])
            isMax = 0;
    }
    if (isMin == 1)
        printf("Min Heap");
    else
        printf("%s", isMax == 1 ? "Max Heap" : "Not Heap");
    return 0;
}

```