###2023-6-12

## 01快速排序 

给定你一个长度为 n的整数数列。

请你使用快速排序对这个数列按照从小到大进行排序。

并将排好序的数列按顺序输出。

1≤n≤100000

```c++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e6+10;
int q[N];

int n;

void quick_sort(int q[],int l,int r)
{
    if(l>=r)return ;     //递归结束的条件
    int mid =q[l+r >> 1];//定义枢值
    int i=l-1,j=r+1;     //定义分治的两个起点
    while(i<j)
    {
        do i++;while(q[i]<mid); //从数组的开头开始判断处理
        do j--;while(q[j]>mid); //从数组的结尾开始判断处理
        if(i<j)swap(q[i],q[j]); //交换不满足条件的两个元素
    }
    quick_sort(q,l,j);          //递归处理j的左半边
    quick_sort(q,j+1,r);	   //递归处理j的右半边
    
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    
    for(int i=0;i<n;i++)cin>>q[i];
    quick_sort(q,0,n-1);
    for(int i=0;i<n;i++)cout<<q[i]<<' ';
    
    return 0;
}
```

## 02第k个数

给定一个长度为 n的整数数列，以及一个整数 k�，请用快速选择算法求出数列从小到大排序后的第 k个数

#### 输入格式 ：第一行包含两个整数 n 和 k。

第二行包含 n个整数（所有整数均在 1∼1 e 9范围内），表示整数数列

#### 输出格式

输出一个整数，表示数列的第 k 小数。

#### 数据范围

1≤n≤1000001,
1≤k≤n

```c++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e6;
int q[N];
int n,k;

int quick_sort(int l,int r,int k)
{
    if(l==r)return q[l];
    int i=l-1,j=r+1;
    int mid =q[l+r >> 1];
    while(i<j)
    {
        do i++;while(q[i]<mid);
        do j--;while(q[j]>mid);
        if(i<j)swap(q[i],q[j]);
    }
    int left =j-l+1;
    if(k<=left)quick_sort(l,j,k);//寻找下标为k的点
    else quick_sort(j+1,,r,k-left);
}

int main()
{
    ios::sync_with_stdio(false);
    cin>>n>>k;
    
    for(int i=0;i<n;i++)cin>>q[i];
    cout<<quick_sort(0,n-1,k)<<endl;
    
    return 0;
}
```



## 03归并排序

给定你一个长度为 n 的整数数列。

请你使用归并排序对这个数列按照从小到大进行排序。

并将排好序的数列按顺序输出。

#### 输入格式

输入共两行，第一行包含整数 n。

第二行包含 n 个整数（所有整数均在 1∼1091∼109 范围内），表示整个数列。

#### 输出格式

输出共一行，包含 n个整数，表示排好序的数列。

#### 数据范围

1≤n≤100000



```c++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e6+10;
int q[N],temp[N];
int n;
void merge_sort(int q[],int l,int r)
{
    if(l>=r)return ;
    int mid =l+r >> 1;
    merge_sort(q,l,mid);//先递归左半边
    merge_sort(q,mid+1,r);//递归右半边
    
    int k=0,i=l,j=mid+1;
    while(i<=mid&&j<=r)/两个起点循环条件
    {
        if(q[i]<q[j])temp[k++]=q[i++];//较小者放入临时数组
        else temp[k++]=q[j++];
    }
    while(i<=mid)temp[k++]=q[i++];//扫左半边尾巴
    while(j<=r)temp[k++]=q[j++];//扫右半边尾巴
    
    for(int i=l,j=0;i<=r;i++,j++)q[i]=temp[j];//对原数组进行处理
    
}

int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    
    for(int i=0;i<n;i++)cin>>q[i];
    
    merge_sort(q,0,n-1);
    
    for(int i=0;i<n;i++)cout<<q[i]<<' ';
    
    return 0; 
}
```



## 04逆序对的数量

给定一个长度为 n 的整数数列，请你计算数列中的逆序对的数量。

逆序对的定义如下：对于数列的第 i个和第 j 个元素，如果满足 i<j 且 a[i]>a[j]，则其为一个逆序对；否则不是。

#### 输入格式

第一行包含整数 n，表示数列的长度。

第二行包含 n个整数，表示整个数列。

#### 输出格式

输出一个整数，表示逆序对的个数。

#### 数据范围

1≤≤100000，
数列中的元素的取值范围 []1,10_9]

算法思想 ： 利用归并算法的过程 分别设置两个起点，通过比较当前是否出现逆序对，然后改变其中一个起点，达到记录数组中逆序对个数

<img src="C:\Users\HASEE\AppData\Roaming\Typora\typora-user-images\image-20230609183749332.png" alt="image-20230609183749332" style="zoom:50%;" />

```c++
#include<iostream>
#include<algorithm>

typedef long long ll;

using namespace std;

const int N=1e6+10;

int q[N],temp[N];
int n;

ll merge_sort(int q[],int l,int r)//在归并算法中计算逆序对
{
    if(l>=r)return 0;
    int mid =l+r >> 1;
    ll res=merge_sort(q,l,mid)+merge_sort(q,mid+1,r);//答案等于左半边递归+右半边递归的结果
    
    int i=l,j=mid+1,k=0;
    while(i<=mid&&j<=r)
    {
        if(q[i]<=q[j])temp[k++]=q[i++];
        else 
        {
            temp[k++]=q[j++];
            res+=mid-i+1;//根据上图显示的逆序对个数
        }
    }
    while(i<=mid)temp[k++]=q[i++];
    while(j<=r)temp[k++]=q[j++];
    for(int i=l,j=0;i<=r;i++,j++)q[i]=temp[j];
    
    return res;
    
}

int main()
{
    ios::sync_with_stdio(false);
    cin>>n;
    
    for(int i=0;i<n;i++)cin>>q[i];
    
    cout<<merge_sort(q,0,n-1)<<endl;
    
    return 0;
}
```



## 05数的范围

给定一个按照升序排列的长度为 n 的整数数组，以及 q 个查询。

对于每个查询，返回一个元素 k的起始位置和终止位置（位置从 00 开始计数）。

如果数组中不存在该元素，则返回 `-1 -1`。

#### 输入格式

第一行包含整数 n和 q，表示数组长度和询问个数。

第二行包含 n个整数（均在 1∼100001∼10000 范围内），表示完整数组。

接下来 q 行，每行包含一个整数 k，表示一个询问元素。

#### 输出格式

共 q 行，每行包含两个整数，表示所求元素的起始位置和终止位置。

如果数组中不存在该元素，则返回 `-1 -1`。

#### 数据范围

1≤n<=100000
1≤q<=10000
1≤k<=10000

```C++
#include<iostream>
#include<algorithm>

using namespace std;

const int N=1e6+10;

int n,m;


int main()
{
    ios::sync_with_stdio(false);
    
    cin>>n>>m;
    for(int i=0;i<n;i++)cin>>q[i];
    
    while(m--0)
    {
        int x;
        cin>>x;
        
        int l=0,r=n-1;//经典的整数二分写法
        while(l<r)
        {
            int mid=l+r >> 1;
            if(q[mid]>=x)r=mid;
            else l=mid+1;
        }
        
        if(q[l]!=x)cout<<"-1 -1"<<endl;
        else 
        {
            cout<<l<<" ";
            int l=0,r=n-1;
            while(l<r)
            {
                int mid =l+r+1 >> 1;
                if(q[mid]<=x)l=mid;
                else r=mid-1;
            }
            
            cout<<r<<endl;
        }
        
        return 0;
    }


```

##  数的三次方根

给定一个浮点数 n，求它的三次方根。

#### 输入格式

共一行，包含一个浮点数 n。

#### 输出格式

共一行，包含一个浮点数，表示问题的解。

注意，结果保留 66 位小数。

#### 数据范围

−10000≤n≤10000

```C++
#include<iostream>
#include<algorithm>

using namespace std;

double n;

int main()
{
    cin>>n;
    double l=-10000,r=10000;
    while(r-l>1e-8)
    {
        double mid=(l+r)/2;
        if(mid*mid*mid<n)l=mid;
        else r=mid;
    }
    printf("%.6lf\n",l);
    return 0;
}
```

## 高精度加法

给定两个正整数（不含前导 00），计算它们的和。

#### 输入格式

共两行，每行包含一个整数。

#### 输出格式

共一行，包含所求的和。

#### 数据范围

1≤整数长度≤100000



```c++
#include<iostream>
#include<algorithm>
#include<vector>
#include<string>
using namespace std;

vector<int> A,B,C;
string a,b;

vector<int> add(vector<int> &A,vector<int> &B)
{
    vector<int> C;
    int t=0;
    for(int i=0;i<A.size()||i<B.size();i++)
    {
        if(i<A.size())t+=A[i];
        if(i<B.size())t+=B[i];
        C.push_back(t%10);
        t/=10;
    }
    
    if(t)C.push_back(1);
    return C;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>a>>b;
    
    for(int i=a.size()-1;i>=0;i--)A.push_back(a[i]-'0');
    for(int i=b.size()-1;i>=0;i--)B.push_back(b[i]-'0');
    
    auto C=add(A,B);
    for(int i=C.size()-1;i>=0;i--)cout<<C[i];
    return 0;
}
```

## 高精度减法

给定两个正整数（不含前导 0），计算它们的差，计算结果可能为负数。

#### 输入格式

共两行，每行包含一个整数。

#### 输出格式

共一行，包含所求的差。

#### 数据范围

1≤整数长度≤105



```c++
#include<iostream>
#include<algorithm>
#include<vector>
#include<string>

using namespace std;

vector<int> A,B;
string a,b;
bool cmp(vector<int>&A,vector<int>&B)
{
    if(A.size()!=B.size())return A.size()>B.size();
    for(int i=A.size()-1;i>=0;i--)
    {
        if(A[i]!=B[i])
        {
            return A[i]>B[i];
        }
    }
    return true;
}

vector<int> sub(vector<int> &A,vector<int> &B)
{
    vector<int> C;
    int t=0;
    for(int i=0;i<A.size()||i<B.size();i++)
    {
        t=A[i]-t;
        if(i<B.size())t-=B[i];
        C.push_back((t+10)%10);
        
        if(t<0)t=1;
        else t=0;
    }
    
    while(C.size()>1&&C.back()==0)C.pop_back();
    
    return C;
}
int main()
{
    ios::sync_with_stdio(false);
    
    cin>>a>>b;
    
    for(int i=a.size()-1;i>=0;i--)A.push_back(a[i]-'0');
    for(int i=b.size()-1;i>=0;i--)B.push_back(b[i]-'0');
    
    if(cmp(A,B))
    {
        auto C=sub(A,B);
        for(int i=C.size()-1;i>=0;i--)cout<<C[i];
    }else 
    {
        auto C=sub(B,A);
        cout<<"-";
        for(int i=C.size()-1;i>=0;i--)cout<<C[i];
    } 
    return 0;
}
```

## 高精度乘法

给定两个非负整数（不含前导 0） A 和 B，请你计算 A×B 的值。

#### 输入格式

共两行，第一行包含整数 A，第二行包含整数 B。

#### 输出格式

共一行，包含 A×B的值。

#### 数据范围

1≤A的长度≤100000
0≤B≤10000

```c++
#include<iostream>
#include<algorithm>
#include<vector>
#include<string>

using namespace std;


vector<int> A,B;
string a,b;


vector<int> mul(vector<int> &A,vector<int> &B)
{
    vector<int> C(A.size()+B.size()+10,0);
    for(int i=0;i<A.size();i++)
    {
        for(int j=0;j<B.size();j++)
        {
            C[i+j]+=A[i]*B[j];
        }
    }
    
    int t=0;
    for(int i=0;i<C.size();i++)
    {
        t+=C[i];
        C[i]=t%10;
        t/=10;
    }
    while(C.size()>1&&C.back()==0)C.pop_back();
    return C;
}
               
int main()
{
    ios::sync_with_stdio(false);
    cin>>a>>b;
    
   	for(int i=a.size()-1;i>=0;i--)A.push_back(a[i]-'0');
    for(int i=b.size()-1;i>=0;i--)B.push_back(b[i]-'0');
    
    auto C=mul(A,B);
    for(int i=C.size()-1;i>=0;i--)cout<<C[i];
    
    return 0;
}
```

## 高精度除法

给定两个非负整数(不包含前导0)A,B，请你计算A/B的商和余数

### 输入格式

共两行,第一行包含整数A,第二行包含整数B

### 输入格式

共两行,第一行输出所求的商,第二行输出所求的余数

### 数据范围

1<=A的长度<=100000

1<=B<=10000

B一定不为0

```c++
#include<iostream>
#include<algorithm>
#include<vector>
#include<string>

using namespace std;

vector<int> A;
int B;
string a;
vector<int> div(vector<int>&A,int b,int &r)
{
    vector<int>C;
    r=0;
    for(int i=A.size()-1;i>=0;i--)
    {
        r=r*10+A[i];
        C.push_back(r/b);
        r%=b;
    }
    reverse(C.begin(),C.end());
    while(C.size()>1&&C.back()==0)C.pop_back();
    return C;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>a>>B;
    
    for(int i=a.size()-1;i>=0;i--)A.push_back(a[i]-'0');
    
    int r;
    auto C=div(A,B,r);
    for(int i=C.size()-1;i>=0;i++)cout<<c[i];
    cout<<r<<endl;
    return 0;
}


```

## 前缀和

输入一个长度为n的整数序列

接下来再输入m个询问，每个寻味输出一堆l,r.

对于每个询问,输出原序列中从第l个数到第r个数的和

### 输出格式

第一行包含两个整数n和m

第二行包含n个整数，表示整数数列

接下里m行，每行包含两个整数l和r表示一个询问的区间范围

```c++
#include<iostream>
#include<algorithm>

using namespace std;
const int N=1e5+10;
int sum[N],a[N];
int n,m,x;

int main()
{
    ios::sync_with_stdio(false);
    for(int i=1;i<=n;i++)
	{
   	 	sum[i]=sum[i-1]+a[i];
	
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        cin>>x;
        sum[i]=x+sum[i-1];
    }
    while(m--)
    {
        int l,r;
        cin>>l>>r;
        cout<<sum[r]-sum[l-1]<<endl;
        
    }
    
    return 0;
}
```

