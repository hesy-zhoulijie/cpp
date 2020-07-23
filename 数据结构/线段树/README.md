# 线段树是什么？？线段树怎么写？？

如果你在考提高组前一天还在问这个问题，那么你会与一等奖失之交臂；如果你还在冲击普及组一等奖，那么这篇博客会浪费你人生中宝贵的 5~20 分钟。

上面两句话显而易见，线段树这个数据结构是一个从萌新到正式 OI 选手的过渡，是一个非常重要的算法，也是一个对于萌新来说较难的算法。不得不说，我学习了这个算法 5 遍左右才有勇气写的这篇博客。

但是，对于 OI 正式选手来说，线段树不是算法，应该是一种工具。她能把一些对于区间（或者线段）的修改、维护，从 O(N) 的时间复杂度变成 O（logN）。

废话不说，这篇博客会分为四部：

**第一部：线段树概念引入**

**第二部：简单 (无 pushdown）的线段树**

**第三部：区间 +/- 修改与查询**

**第四部：区间乘除修改与查询**

**总结**

## 第一部 概念引入

线段树是一种二叉树，也就是对于一个线段，我们会用一个二叉树来表示。比如说一个长度为 4 的线段，我们可以表示成这样：

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180919200225625-1602154608.png)

这是什么意思呢？ 如果你要表示线段的和，那么最上面的根节点的权值表示的是这个线段 1~4 的和。根的两个儿子分别表示这个线段中 1~2 的和，与 3~4 的和。以此类推。

然后我们还可以的到一个性质：**节点 i 的权值 = 她的左儿子权值 + 她的右儿子权值**。因为 1~4 的和就是等于 1~2 的和 + 2~3 的和。

根据这个思路，我们就可以建树了，我们设一个结构体 tree，**tree[i].l 和 tree[i].r 分别表示这个点代表的线段的左右下标，tree[i].sum 表示这个节点表示的线段和。**

我们知道，一颗二叉树，**她的左儿子和右儿子编号分别是她 * 2 和她 * 2+1**

再根据刚才的性质，得到式子：**tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;** 就可以建一颗线段树了！代码如下：

```cpp
inline void build(int i,int l,int r){//递归建树
    tree[i].l=l;tree[i].r=r;
    if(l==r){//如果这个节点是叶子节点
        tree[i].sum=input[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(i*2,l,mid);//分别构造左子树和右子树
    build(i*2+1,mid+1,r);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;//刚才我们发现的性质return ;
}
```

嗯，这就是线段树的构建，你可能会问为什么要开好几倍的内存去储存一条线段。这是因为我们还没有让这个过大的数组干一些实事，那么什么是实事呢？让我们进入下一部（在你看懂这一部的情况下）

## 第二部 简单（无 pushdown）的线段树

### 1、单点修改，区间查询

其实这一章开始才是真正的线段树，我们要用线段树干什么？答案是维护一个线段（或者区间），比如你想求出一个 1~100 区间中，4~67 这些元素的和，你会怎么做？朴素的做法是 for(i=4;i<=67;i++)  sum+=a[i]，这样固然好，但是算得太慢了。

我们想一种新的方法，先想一个比较好画图的数据，比如一个长度为 4 的区间，分别是 1、2、3、4, 我们想求出第 1~3 项的和。按照上一部说的，我们要建出一颗线段树，其中点权（也就是红色）表示和：

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180919203745885-314299579.png)

然后我们要求 1~3 的和，我们先从根节点开始查询，发现她的左儿子 1~2 这个区间和答案区间 1~3 有交集，那么我们跑到左儿子这个区间。

然后，我们发现这个区间 1~2 被完全包括在答案区间 1~3 这个区间里面，那就把她的值 3 返回。

我们回到了 1~4 区间，发现她的右儿子 3~4 区间和答案区间 1~3 有交集，那么我们走到 3~4 区间

到了 3~4 区间，我们发现她并没有完全包含在答案区间 1~3 里面，但发现她的左儿子 3~3 区间和 1~3 区间又交集，那么久走到 3~3 区间

到了 3~3 区间，发现其被答案区间完全包含，就返回她的值 3 一直到最开始

3~3 区间的 3+1~2 区间的 3=6，我们知道了 1~3 区间和为 6.

有人可能会说你这样是不是疯了，我那脚都能算出 1+2+3=6，为什么这么麻烦？！

因为这才几个数，如果一百万个数，这样时间会大大增快。

我们总结一下，线段树的查询方法：

**1、如果这个区间被完全包括在目标区间里面，直接返回这个区间的值**

**2、如果这个区间的左儿子和目标区间有交集，那么搜索左儿子**

**3、如果这个区间的右儿子和目标区间有交集，那么搜索右儿子**

写成代码，就会变成这样：

```cpp
inline int search(int i,int l,int r){
    if(tree[i].l>=l && tree[i].r<=r)//如果这个区间被完全包括在目标区间里面，直接返回这个区间的值
        return tree[i].sum;
    if(tree[i].r<l || tree[i].l>r)  return 0;//如果这个区间和目标区间毫不相干，返回0
    int s=0;
    if(tree[i*2].r>=l)  s+=search(i*2,l,r);//如果这个区间的左儿子和目标区间又交集，那么搜索左儿子
    if(tree[i*2+1].l<=r)  s+=search(i*2+1,l,r);//如果这个区间的右儿子和目标区间又交集，那么搜索右儿子
    return s;
}
```

**关于那几个 if 的条件一定要看清楚，最好背下来，以防考场上脑抽推错。**

然后, 我们怎么修改这个区间的单点，其实这个相对简单很多，你要把区间的第 dis 位加上 k。

那么你从根节点开始，看这个 dis 是在左儿子还是在右儿子，在哪往哪跑，

然后返回的时候，还是按照 **tree[i].sum=tree[i*2].sum+tree[i*2+1].sum** 的原则，更新所有路过的点

如果不理解，我还是画个图吧，其中深蓝色是去的路径，浅蓝色是返回的路径，回来时候红色的 + 标记就是把这个点加上这个值。

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180919205930637-1487496115.png)

把这个过程变成代码，就是这个样子：

```cpp
inline void add(int i,int dis,int k){
    if(tree[i].l==tree[i].r){//如果是叶子节点，那么说明找到了
        tree[i].sum+=k;
        return ;
    }
    if(dis<=tree[i*2].r)  add(i*2,dis,k);//在哪往哪跑
    else  add(i*2+1,dis,k);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;//返回更新
    return ;
}
```

###  2、区间修改，单点查询

区间修改和单点查询，我们的思路就变为：如果把这个区间加上 k，相当于把这个区间涂上一个 k 的标记，然后单点查询的时候，就从上跑道下，把沿路的标记加起来就好。

这里面给区间贴标记的方式与上面的区间查找类似，原则还是那三条，只不过第一条：**如果这个区间被完全包括在目标区间里面，直接返回这个区间的值**变为了如果这个区间**如果这个区间被完全包括在目标区间里面，讲这个区间标记 k**

具体做法很像，这里贴上代码：

```cpp
inline void add(int i,int l,int r,int k){
    if(tree[i].l>=l && tree[i].r<=r){//如果这个区间被完全包括在目标区间里面，讲这个区间标记k
        tree[i].sum+=k;
        return ;
    }
    if(tree[i*2].r>=l)
        add(i*2,l,r,k);
    if(tree[i*2+1].l<=r)
        add(i*2+1,l,r,k);
}
```

然后就是单点查询了，这个更好理解了，就是 dis 在哪往哪跑，把路径上所有的标价加上就好了：

```cpp
void search(int i,int dis){
    ans+=tree[i].num;//一路加起来
    if(tree[i].l==tree[i].r)
        return ;
    if(dis<=tree[i*2].r)
        search(i*2,dis);
    if(dis>=tree[i*2+1].l)
        search(i*2+1,dis);
}
```

不知不觉，这第二章已经结束。这样的简单（原谅我用这个词）线段树，还可除了求和，还可以求区间最小最大值，还可以区间染色。

但是！这样的线段树展现不出来她的魅力，因为区间求和，树状数组比她少了一个很大的常熟。二区间最值，ST 的那神乎其技的 O（n）查询也能完爆她。这是为什么？因为线段树的魅力还没有展现出来，她最美丽的地方：pushdown 还未展现于世，如果你已经对这一章充足的了解，并且能不看博客把洛谷上树状数组模板 1、2 都能写出来，那么请你进入下一部。

## 第三部 进阶线段树

区间修改、区间查询，你可能会认为，把上一章里面的这两个模块加在一起就好了，然后你就会发现你大错特错。

因为如果对于 1~4 这个区间，你把 1~3 区间 + 1，相当于把节点 1~2 和 3 标记，但是如果你查询 2~4 时，你会发现你加的时没有标记的 2 节点和没有标记的 3~4 节点加上去，结果当然是错的。

那么我们应该怎么办？这时候 pushdown 的作用就显现出来了。

你会想到，我们只需要在查询的时候，如果我们要查的 2 节点在 1~2 区间的里面，那我们就可以把 1~2 区间标记的那个 + 1 给推下去这样就能顺利地加上了。  
怎么记录这个标记呢？我们需要记录一个 “懒标记”lazytage，来记录这个区间

区间修改的时候，我们按照如下原则：

**1、如果当前区间被完全覆盖在目标区间里，讲这个区间的 sum+k*(tree[i].r-tree[i].l+1)**

**2、如果没有完全覆盖，则先下传懒标记**

**3、如果这个区间的左儿子和目标区间有交集，那么搜索左儿子**

**4、如果这个区间的右儿子和目标区间有交集，那么搜索右儿子**

 然后查询的时候，将这个懒标记下传就好了，下面图解一下：

如图，区间 1~4 分别是 1、2、3、4，我们要把 1~3 区间 + 1。因为 1~2 区间被完全覆盖，所以将其 + 2，并将紫色的 lazytage+1，3 区间同理

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180920195821587-4845808.png)

注意我们处理完这些以后，还是要按照 **tree[i].sum=tree[i*2].sum+tree[i*2+1].sum** 的原则返回，代码如下：

```cpp
void add(int i,int l,int r,int k)
{
    if(tree[i].r<=r && tree[i].l>=l)//如果当前区间被完全覆盖在目标区间里，讲这个区间的sum+k*(tree[i].r-tree[i].l+1)
    {
        tree[i].sum+=k*(tree[i].r-tree[i].l+1);
        tree[i].lz+=k;//记录lazytage
        return ;
    }
    push_down(i);//向下传递
    if(tree[i*2].r>=l)
        add(i*2,l,r,k);
    if(tree[i*2+1].l<=r)
        add(i*2+1,l,r,k);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;
    return ;
}
```

其中的 pushdown，就是把自己的 lazytage 归零，并给自己的儿子加上，并让自己的儿子加上 k*(r-l+1)

```cpp
void push_down(int i)
{
    if(tree[i].lz!=0)
    {
        tree[i*2].lz+=tree[i].lz;//左右儿子分别加上父亲的lz
        tree[i*2+1].lz+=tree[i].lz;
        init mid=(tree[i].l+tree[i].r)/2;
        tree[i*2].data+=tree[i].lz*(mid-tree[i*2].l+1);//左右分别求和加起来
        tree[i*2+1].data+=tree[i].lz*(tree[i*2+1].r-mid);
        tree[i].lz=0;//父亲lz归零
    }
    return ;
}
```

查询的时候，和上一章的几乎一样，就是也要像修改一样加入 pushdown，这里用图模拟一下。我们要查询 2~4 区间的和，这是查询前的情况，所有紫色的代表 lazytage

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180921131618196-79075472.png)

然后，我们查到区间 1~2 时，发现这个区间并没有被完全包括在目标区间里，于是我们就 pushdown，lazytage 下传，并让每个区间 sum 加上（r-l）*lazytage。

![](https://img2018.cnblogs.com/blog/987049/201809/987049-20180921132140938-396404765.png)

然后查到 2~2 区间，发现被完全包含，所以就返 3，再搜索到 3~4 区间，发现被完全包含，那么直接返回 8，最后 3+8=11 就是答案

这里是代码实现：

```cpp
inline int search(int i,int l,int r){
    if(tree[i].l>=l && tree[i].r<=r)
        return tree[i].sum;
    if(tree[i].r<l || tree[i].l>r)  return 0;
    push_down(i);
    int s=0;
    if(tree[i*2].r>=l)  s+=search(i*2,l,r);
    if(tree[i*2+1].l<=r)  s+=search(i*2+1,l,r);
    return s;
}
```

 好了，到了这里，我们就学会了用线段树进行区间加减操作，大家可以完成洛谷的线段树模板 1.

## 第四部 乘法（根号）线段树

### 1、乘法线段树

如果这个线段树只有乘法，那么直接加入 lazytage 变成乘，然后 tree[i].sum*=k 就好了。但是，如果我们是又加又乘，那就不一样了。

当 lazytage 下标传递的时候，我们需要考虑，是先加再乘还是先乘再加。我们只需要对 lazytage 做这样一个处理。

lazytage 分为两种，分别是加法的 plz 和乘法的 mlz。

mlz 很简单处理，pushdown 时直接 * 父亲的就可以了，那么加法呢？

我们需要把原先的 plz * 父亲的 mlz 再加上父亲的 plz.

```cpp
inline void pushdown(long long i){//注意这种级别的数据一定要开long long
    long long k1=tree[i].mlz,k2=tree[i].plz;
    tree[i<<1].sum=(tree[i<<1].sum*k1+k2*(tree[i<<1].r-tree[i<<1].l+1))%p;//
    tree[i<<1|1].sum=(tree[i<<1|1].sum*k1+k2*(tree[i<<1|1].r-tree[i<<1|1].l+1))%p;
    tree[i<<1].mlz=(tree[i<<1].mlz*k1)%p;
    tree[i<<1|1].mlz=(tree[i<<1|1].mlz*k1)%p;
    tree[i<<1].plz=(tree[i<<1].plz*k1+k2)%p;
    tree[i<<1|1].plz=(tree[i<<1|1].plz*k1+k2)%p;
    tree[i].plz=0;
    tree[i].mlz=1;
    return ;
}
```

然后加法和减法的函数同理，维护 lazytage 的时候加法标记一定要记得现乘再加。

值得一提的是，计算 * 2 时一定要改成 i<<1 这样能解决很多时间，还有要开 long long，还有，函数前面要加 inline 我在其他 OJ 交这道题时，就因为没加 inline 就被卡了，交了就过了。

### 2、根号线段树

其实，根号线段树和除法线段树一样。她们乍眼一看感觉直接用 lazytage 标记除了多少，但是实际上，会出现精度问题。

c++ 的除法是向下取整，很明显，(a+b)/k！=a/k+b/k（在向下取整的情况下），而根号，很明显根号（a）+ 根号 (b)!= 根号(a+b) 那么怎么办？

第一个想法就是暴力，对于每个要改动的区间 l~r, 把里面的每个点都单独除，但这样就会把时间复杂度卡得比大暴力都慢（因为多个常数），所以怎么优化？

我们对于每个区间，维护她的最大值和最小值，然后每次修改时，如果这个区间的最大值根号和最小值的根号一样，说明这个区间整体根号不会产生误差，就直接修改（除法同理）

其中，lazytage 把除法当成减法，记录的是这个区间里每个元素减去的值。

下面是根号线段树的修改过程：

```cpp
inline void Sqrt(int i,int l,int r){
    if(tree[i].l>=l && tree[i].r<=r && (tree[i].minn-(long long)sqrt(tree[i].minn))==(tree[i].maxx-(long long)sqrt(tree[i].maxx))){//如果这个区间的最大值最小值一样
        long long u=tree[i].minn-(long long)sqrt(tree[i].minn);//计算区间中每个元素需要减去的
        tree[i].lz+=u;
        tree[i].sum-=(tree[i].r-tree[i].l+1)*u;
        tree[i].minn-=u;
        tree[i].maxx-=u;
            //cout<<"i"<<i<<" "<<tree[i].sum<<endl;
        return ;
    }
    if(tree[i].r<l || tree[i].l>r)  return ;
    push_down(i);
    if(tree[i*2].r>=l)  Sqrt(i*2,l,r);
    if(tree[i*2+1].l<=r)  Sqrt(i*2+1,l,r);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;
    tree[i].minn=min(tree[i*2].minn,tree[i*2+1].minn);//维护最大值和最小值
    tree[i].maxx=max(tree[i*2].maxx,tree[i*2+1].maxx);
    //cout<<"i"<<i<<" "<<tree[i].sum<<endl;
    return ;
}
```

然后 pushdown 没什么变化，就是要记得 tree[i].minn、tree[i].maxx 也要记得 - lazytage。

## 模板题与代码：

最后，我们给几道模板题，再贴上代码：

单点修改，区间查询：[洛谷树状数组模板 1](https://www.luogu.org/problemnew/show/P3374)

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <cstdlib>
#include <queue>
#include <stack>
#include <vector>
using namespace std;
#define MAXN 100010
#define INF 10000009
#define MOD 10000007
#define LL long long
#define in(a) a=read()
#define REP(i,k,n) for(long long i=k;i<=n;i++)
#define DREP(i,k,n) for(long long i=k;i>=n;i--)
#define cl(a) memset(a,0,sizeof(a))
inline long long read(){
    long long x=0,f=1;char ch=getchar();
    for(;!isdigit(ch);ch=getchar()) if(ch=='-') f=-1;
    for(;isdigit(ch);ch=getchar()) x=x*10+ch-'0';
    return x*f;
}
inline void out(long long x){
    if(x<0) putchar('-'),x=-x;
    if(x>9) out(x/10);
    putchar(x%10+'0');
}
long long n,m,p;
long long input[MAXN];
struct node{
    long long l,r;
    long long sum,mlz,plz;
}tree[4*MAXN];
inline void build(long long i,long long l,long long r){
    tree[i].l=l;
    tree[i].r=r;
    tree[i].mlz=1;
    if(l==r){
        tree[i].sum=input[l]%p;
        return ;
    }
    long long mid=(l+r)>>1;
    build(i<<1,l,mid);
    build(i<<1|1,mid+1,r);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline void pushdown(long long i){
    long long k1=tree[i].mlz,k2=tree[i].plz;
    tree[i<<1].sum=(tree[i<<1].sum*k1+k2*(tree[i<<1].r-tree[i<<1].l+1))%p;
    tree[i<<1|1].sum=(tree[i<<1|1].sum*k1+k2*(tree[i<<1|1].r-tree[i<<1|1].l+1))%p;
    tree[i<<1].mlz=(tree[i<<1].mlz*k1)%p;
    tree[i<<1|1].mlz=(tree[i<<1|1].mlz*k1)%p;
    tree[i<<1].plz=(tree[i<<1].plz*k1+k2)%p;
    tree[i<<1|1].plz=(tree[i<<1|1].plz*k1+k2)%p;
    tree[i].plz=0;
    tree[i].mlz=1;
    return ;
}
inline void mul(long long i,long long l,long long r,long long k){
    if(tree[i].r<l || tree[i].l>r)  return ;
    if(tree[i].l>=l && tree[i].r<=r){
        tree[i].sum=(tree[i].sum*k)%p;
        tree[i].mlz=(tree[i].mlz*k)%p;
        tree[i].plz=(tree[i].plz*k)%p;
        return ;
    }
    pushdown(i);
    if(tree[i<<1].r>=l)  mul(i<<1,l,r,k);
    if(tree[i<<1|1].l<=r)  mul(i<<1|1,l,r,k);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline void add(long long i,long long l,long long r,long long k){
    if(tree[i].r<l || tree[i].l>r)  return ;
    if(tree[i].l>=l && tree[i].r<=r){
        tree[i].sum+=((tree[i].r-tree[i].l+1)*k)%p;
        tree[i].plz=(tree[i].plz+k)%p;
        return ;
    }
    pushdown(i);
    if(tree[i<<1].r>=l)  add(i<<1,l,r,k);
    if(tree[i<<1|1].l<=r)  add(i<<1|1,l,r,k);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline long long search(long long i,long long l,long long r){
    if(tree[i].r<l || tree[i].l>r)  return 0;
    if(tree[i].l>=l && tree[i].r<=r)
        return tree[i].sum;
    pushdown(i);
    long long sum=0;
    if(tree[i<<1].r>=l)  sum+=search(i<<1,l,r)%p;
    if(tree[i<<1|1].l<=r)  sum+=search(i<<1|1,l,r)%p;
    return sum%p;
}
int main(){
    in(n);    in(m);in(p);
    REP(i,1,n)  in(input[i]);
    build(1,1,n); 

    REP(i,1,m){
        long long fl,a,b,c;
        in(fl);
        if(fl==1){
            in(a);in(b);in(c);
            c%=p;
            mul(1,a,b,c);
        }
        if(fl==2){
            in(a);in(b);in(c);
            c%=p;
            add(1,a,b,c);
        }
        if(fl==3){
            in(a);in(b);
            printf("%lld\n",search(1,a,b));
        }
    }
    return 0;
}
/*
5 4 1000
1 2 3 4 5
3 1 5
2 1 5 1
1 1 5 2

3 1 5
*/
```

区间修改，单点查询：[洛谷树状数组模板 2](https://www.luogu.org/problemnew/show/P3368)

```cpp
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <queue>
using namespace std;
int n,m;
int ans;
int input[500010];
struct node
{
    int left,right;
    int num;
}tree[2000010];
void build(int left,int right,int index)
{
    tree[index].num=0;
    tree[index].left=left;
    tree[index].right=right;
       if(left==right)
        return ;
    int mid=(right+left)/2;
    build(left,mid,index*2);
    build(mid+1,right,index*2+1);
}
void pls(int index,int l,int r,int k)
{
    if(tree[index].left>=l && tree[index].right<=r)
    {
        tree[index].num+=k;
        return ;
    }
    if(tree[index*2].right>=l)
       pls(index*2,l,r,k);
    if(tree[index*2+1].left<=r)
       pls(index*2+1,l,r,k);
}
void search(int index,int dis)
{
    ans+=tree[index].num;
    if(tree[index].left==tree[index].right)
        return ;
    if(dis<=tree[index*2].right)
        search(index*2,dis);
    if(dis>=tree[index*2+1].left)
        search(index*2+1,dis);
}
int main()
{
    int n,m;
    cin>>n>>m;
    build(1,n,1);
    for(int i=1;i<=n;i++)
        scanf("%d",&input[i]);
    for(int i=1;i<=m;i++)
    {
        int a;
        scanf("%d",&a);
        if(a==1)
        {
            int x,y,z;
            scanf("%d%d%d",&x,&y,&z);
            pls(1,x,y,z);
        }
        if(a==2)
        {
            ans=0;
            int x;
            scanf("%d",&x);
            search(1,x);
            printf("%d\n",ans+input[x]);
        }
    }
}
```

区间加法，[洛谷线段树模板 1](https://www.luogu.org/problemnew/show/P3372)

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <cstring>
#define init long long
using namespace std;
init n,m;
struct node
{
    init l,r,data;
    init lt;    
}tree[1000010];
init arr[1000010];
void build(init l,init r,init index,init arr[])
{
    tree[index].lt=0;
    tree[index].l=l;
    tree[index].r=r;
    if(l==r)
    {
        tree[index].data=arr[l];
        return ;
    }
    init mid=(l+r)/2;
    build(l,mid,index*2,arr);
    build(mid+1,r,index*2+1,arr);
    tree[index].data=tree[index*2].data+tree[index*2+1].data;
    return ;
}
void push_down(init index)
{
    if(tree[index].lt!=0)
    {
        tree[index*2].lt+=tree[index].lt;
        tree[index*2+1].lt+=tree[index].lt;
        init mid=(tree[index].l+tree[index].r)/2;
        tree[index*2].data+=tree[index].lt*(mid-tree[index*2].l+1);
        tree[index*2+1].data+=tree[index].lt*(tree[index*2+1].r-mid);
        tree[index].lt=0;
    }
    return ;
}
void up_data(init index,init l,init r,init k)
{
    if(tree[index].r<=r && tree[index].l>=l)
    {
        tree[index].data+=k*(tree[index].r-tree[index].l+1);
        tree[index].lt+=k;
        return ;
    }
    push_down(index);
    if(tree[index*2].r>=l)
        up_data(index*2,l,r,k);
    if(tree[index*2+1].l<=r)
        up_data(index*2+1,l,r,k);
    tree[index].data=tree[index*2].data+tree[index*2+1].data;
    return ;
}
init search(init index,init l,init r)
{
    if(tree[index].l>=l && tree[index].r<=r)
        return tree[index].data;
    push_down(index);
    init num=0;
    if(tree[index*2].r>=l)
        num+=search(index*2,l,r);
    if(tree[index*2+1].l<=r)
        num+=search(index*2+1,l,r);
    return num;
}
int main()
{
    cin>>n>>m;
    for(init i=1;i<=n;i++)
        cin>>arr[i];
    build(1,n,1,arr);
    for(init i=1;i<=m;i++)
    {
        init f;
        cin>>f;
        if(f==1)
        {
            init a,b,c;
            cin>>a>>b>>c;
            up_data(1,a,b,c);
        }
        if(f==2)
        {
            init a,b;
            cin>>a>>b;
            printf("%lld\n",search(1,a,b));
        }
    }
}
```

区间乘法：[洛谷线段树模板 2](https://www.luogu.org/problemnew/show/P3373)

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <cstdlib>
#include <queue>
#include <stack>
#include <vector>
using namespace std;
#define MAXN 100010
#define INF 10000009
#define MOD 10000007
#define LL long long
#define in(a) a=read()
#define REP(i,k,n) for(long long i=k;i<=n;i++)
#define DREP(i,k,n) for(long long i=k;i>=n;i--)
#define cl(a) memset(a,0,sizeof(a))
inline long long read(){
    long long x=0,f=1;char ch=getchar();
    for(;!isdigit(ch);ch=getchar()) if(ch=='-') f=-1;
    for(;isdigit(ch);ch=getchar()) x=x*10+ch-'0';
    return x*f;
}
inline void out(long long x){
    if(x<0) putchar('-'),x=-x;
    if(x>9) out(x/10);
    putchar(x%10+'0');
}
long long n,m,p;
long long input[MAXN];
struct node{
    long long l,r;
    long long sum,mlz,plz;
}tree[4*MAXN];
inline void build(long long i,long long l,long long r){
    tree[i].l=l;
    tree[i].r=r;
    tree[i].mlz=1;
    if(l==r){
        tree[i].sum=input[l]%p;
        return ;
    }
    long long mid=(l+r)>>1;
    build(i<<1,l,mid);
    build(i<<1|1,mid+1,r);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline void pushdown(long long i){
    long long k1=tree[i].mlz,k2=tree[i].plz;
    tree[i<<1].sum=(tree[i<<1].sum*k1+k2*(tree[i<<1].r-tree[i<<1].l+1))%p;
    tree[i<<1|1].sum=(tree[i<<1|1].sum*k1+k2*(tree[i<<1|1].r-tree[i<<1|1].l+1))%p;
    tree[i<<1].mlz=(tree[i<<1].mlz*k1)%p;
    tree[i<<1|1].mlz=(tree[i<<1|1].mlz*k1)%p;
    tree[i<<1].plz=(tree[i<<1].plz*k1+k2)%p;
    tree[i<<1|1].plz=(tree[i<<1|1].plz*k1+k2)%p;
    tree[i].plz=0;
    tree[i].mlz=1;
    return ;
}
inline void mul(long long i,long long l,long long r,long long k){
    if(tree[i].r<l || tree[i].l>r)  return ;
    if(tree[i].l>=l && tree[i].r<=r){
        tree[i].sum=(tree[i].sum*k)%p;
        tree[i].mlz=(tree[i].mlz*k)%p;
        tree[i].plz=(tree[i].plz*k)%p;
        return ;
    }
    pushdown(i);
    if(tree[i<<1].r>=l)  mul(i<<1,l,r,k);
    if(tree[i<<1|1].l<=r)  mul(i<<1|1,l,r,k);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline void add(long long i,long long l,long long r,long long k){
    if(tree[i].r<l || tree[i].l>r)  return ;
    if(tree[i].l>=l && tree[i].r<=r){
        tree[i].sum+=((tree[i].r-tree[i].l+1)*k)%p;
        tree[i].plz=(tree[i].plz+k)%p;
        return ;
    }
    pushdown(i);
    if(tree[i<<1].r>=l)  add(i<<1,l,r,k);
    if(tree[i<<1|1].l<=r)  add(i<<1|1,l,r,k);
    tree[i].sum=(tree[i<<1].sum+tree[i<<1|1].sum)%p;
    return ;
}
inline long long search(long long i,long long l,long long r){
    if(tree[i].r<l || tree[i].l>r)  return 0;
    if(tree[i].l>=l && tree[i].r<=r)
        return tree[i].sum;
    pushdown(i);
    long long sum=0;
    if(tree[i<<1].r>=l)  sum+=search(i<<1,l,r)%p;
    if(tree[i<<1|1].l<=r)  sum+=search(i<<1|1,l,r)%p;
    return sum%p;
}
int main(){
    in(n);    in(m);in(p);
    REP(i,1,n)  in(input[i]);
    build(1,1,n); 

    REP(i,1,m){
        long long fl,a,b,c;
        in(fl);
        if(fl==1){
            in(a);in(b);in(c);
            c%=p;
            mul(1,a,b,c);
        }
        if(fl==2){
            in(a);in(b);in(c);
            c%=p;
            add(1,a,b,c);
        }
        if(fl==3){
            in(a);in(b);
            printf("%lld\n",search(1,a,b));
        }
    }
    return 0;
}
/*
5 4 1000
1 2 3 4 5
3 1 5
2 1 5 1
1 1 5 2

3 1 5
*/
```

区间根号，[bzoj3211](https://www.lydsy.com/JudgeOnline/problem.php?id=3211)

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#define MAXN 1000010
#define REP(i,k,n) for(int i=k;i<=n;i++)
#define in(a) a=read()
using namespace std;
int read(){
    int x=0,f=1;
    char ch=getchar();
    for(;!isdigit(ch);ch=getchar())
        if(ch=='-')
          f=-1;
    for(;isdigit(ch);ch=getchar())
        x=x*10+ch-'0';
    return x*f;
}
struct node{
    int l,r;
    long long lz,sum,maxx,minn;
}tree[MAXN<<2];
int n,m,input[MAXN];
inline void build(int i,int l,int r){
    tree[i].l=l;tree[i].r=r;
    if(l==r){
        tree[i].sum=tree[i].minn=tree[i].maxx=input[l];
        return ;
    }
    int mid=(l+r)>>1;
    build(i*2,l,mid);
    build(i*2+1,mid+1,r);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;
    tree[i].minn=min(tree[i*2].minn,tree[i*2+1].minn);
    tree[i].maxx=max(tree[i*2].maxx,tree[i*2+1].maxx);
    return ;
}
inline void push_down(int i){
    if(!tree[i].lz)  return ;
    long long k=tree[i].lz;
    tree[i*2].lz+=k;
    tree[i*2+1].lz+=k;
    tree[i*2].sum-=(tree[i*2].r-tree[i*2].l+1)*k;
    tree[i*2+1].sum-=(tree[i*2+1].r-tree[i*2+1].l+1)*k;
    tree[i*2].minn-=k;
    tree[i*2+1].minn-=k;
    tree[i*2].maxx-=k;
    tree[i*2+1].maxx-=k;
    tree[i].lz=0;
    return ;
}
inline void Sqrt(int i,int l,int r){
    if(tree[i].l>=l && tree[i].r<=r && (tree[i].minn-(long long)sqrt(tree[i].minn))==(tree[i].maxx-(long long)sqrt(tree[i].maxx))){
        long long u=tree[i].minn-(long long)sqrt(tree[i].minn);
        tree[i].lz+=u;
        tree[i].sum-=(tree[i].r-tree[i].l+1)*u;
        tree[i].minn-=u;
        tree[i].maxx-=u;
            //cout<<"i"<<i<<" "<<tree[i].sum<<endl;
        return ;
    }
    if(tree[i].r<l || tree[i].l>r)  return ;
    push_down(i);
    if(tree[i*2].r>=l)  Sqrt(i*2,l,r);
    if(tree[i*2+1].l<=r)  Sqrt(i*2+1,l,r);
    tree[i].sum=tree[i*2].sum+tree[i*2+1].sum;
    tree[i].minn=min(tree[i*2].minn,tree[i*2+1].minn);
    tree[i].maxx=max(tree[i*2].maxx,tree[i*2+1].maxx);
    //cout<<"i"<<i<<" "<<tree[i].sum<<endl;
    return ;
}
inline long long search(int i,int l,int r){
    if(tree[i].l>=l && tree[i].r<=r)
        return tree[i].sum;
    if(tree[i].r<l || tree[i].l>r)  return 0;
    push_down(i);
    long long s=0;
    if(tree[i*2].r>=l)  s+=search(i*2,l,r);
    if(tree[i*2+1].l<=r)  s+=search(i*2+1,l,r);
    return s;
}
int main(){
    in(n);
    REP(i,1,n)  in(input[i]);
    build(1,1,n);
    in(m);
    int a,b,c;
    REP(i,1,m){
        in(a);in(b);in(c);
        if(a==1)
            printf("%lld\n",search(1,b,c));
        if(a==2){
            Sqrt(1,b,c);
            //for(int i=1;i<=7;i++)
            //    cout<<tree[i].sum<<" ";
           // cout<<endl;
        }
    }
}
```

OI 路漫漫，学无止境，愿线段树陪你走过半生，归来仍是少年。
