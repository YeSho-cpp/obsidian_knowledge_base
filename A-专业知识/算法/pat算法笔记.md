# 算法笔记


## 判断素数



==2.移除字符串s第一个字符==：```s.earse(s.begin())``` ==在字符串s前增加n个0==：`s.insert(0,n,'n')`

==给一些数字字符串，求这些字符串拼接起来能够构成的最小的数字的方式：==用`cmp`函数就能解决：`bool cmp(string a,string b){return a+b>b+a;}`

==3.字符问题==：`char a[] `的长度要用`strlen(a)`,是真实的长度，不包含\0
要输入一行的数据的话：如果是string s，则用`getline(cin,s)`,如果前面有输入数据，必须`getchar()`一下，如果是`char str[100],`则用`cin.getline(str,100)`,==并且只有它才可以解收空格也可以用==`gets(str)`.对于一段话要提取单词个数时，利用以下方法：

```c++

  while (cin >> word)
        {
            s.push_back(word);
            char c = getchar();
            if (c == '\n') break;
        }
```

==is_permutation==可以用来判断v2是否是v1的一个序列，返回值为0或1;

```
is_permutation(v1.begin(),v1.end(),v2.end());
```

5.**sscanf**与**sprintf**是处理字符串的利器，sscanf(str,"%d",&n),从左向右，sprintf(str,"%d",n);从右向左

在处理一些字符串判断是否是规定的格式，通过一个sscanf让a以规定格式赋值给一个temp,再用sprintf进一步以格式赋值给b,判断a,b是否相同，就可以判读是否是，

```
char a[50],b[50];double temp;
sscanf(a,"%lf",&temp);sprintf(b,"%.2lf",temp);
```

6.滑动窗口

所谓滑动窗口就是不断调节子数组的起始位置和终止位置，

![素材1](D:\桌面文件\CS 自学\笔记\素材1.gif)

滑动窗口模板

```c++
/* 滑动窗口算法框架 */
void slidingWindow(string s) {
    unordered_map<char, int> window;
    
    int left = 0, right = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/
        
        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}

```

python前缀和

```python
for i in range(n):
            sums.append(sums[-1] + nums[i])
```



==4.链表排序==：遇到链表中有无效结点还要排序，在结构体里面加一个`bool flag=false`;变量，将有效结点的flag标记为true,并统计有效节点的个数`cnt`，然后在`cmp中`这样写：

还可将所有有效结点推入`vector<int>` 中，`bool cmp(int x,int y) {return node[x].key<node[y].key} sort(v.begin(),v.end(),cmp)` 

```c++
bool cmp(node a,node b)
{
    if(a.flag==false||b.false==false)
        return a.flag>b.flag;
    else
        return a.value<b.value;
}
bool cmp(Record a,Record b)
{
    int s=strcmp(a.name,b.name);//char字符串用strcmp(a,b)
    if(s!=0)  return s<0;
}
//求数组中最大值，可以用max_element(),返回为指针
int *b=maxn_element(a,a+5);
cout<<*b;
```

==5迭代问题==：.集合s或者映射s中寻找是否存在某一数字：`s.find(num)!=s.end()`

在迭代遍历集合s或者映射s时，`auto it=s.begin();it!=s.end();it++` set获取元素的值：`*it`，map获取元素的值：it->first,it->second  **`s.end()`代表s的最后一个下标元素的后一个（一般为空），而`s.rbegin()`为是的末元素，`c.rend()`它指向容器s的第一个元素前面的位置,**

==6.pair的用法==：pair是一个变量类型，两两一对组成一个变量，可以定义pair中两个元素的类型，比如`string`和`int`,`pair<string,int>`就是`string`和`int`类型的一对pair类型，我们一般将`pair<string,int>`使用typedef重命名为p之后再使用：

```c++
int main()
{
    typedef pair<string,int> p;//建立string和int类型的一对pair类型，并将类型命名为p
    vector<p> v;
    p temp=make_pair("123",123);
    v.push_back(temp);
    cout<<temp.first<<" "<<temp.second<<endl;
    cout<<v[0].first<<" "<<v[0].second<<endl;
}
在DFS中遍历的结点需要记录某向额外属性时用vector<pair<int,int>>v;
```
2zu
abs()在头文件`#include<stdlib.h>`里面

==7.小问题==

**7.1对于时间的比较**

```c++
bool great(pnode node1, pnode node2)
{
	if (node1.hh != node2.hh) return node1.hh > node2.hh;
	if (node1.mm != node2.mm) return node1.mm > node2.mm;
	return node1.ss > node2.ss;
}
排名的实现 
for(int i=0;i<n;i++)
{
    if(i==0||stu[i].score!=stu[i].score) stu[i].r=i+1;
    else stu[i].r=stu[i-1].r;
}
对于一个主键多条信息，可以采用map<string,vector<node> > user;
```

**7.2 b进制的转换**

```c++
// 315 3 1 5
vector<int> digit;
while(n!=0)
{
    digit.push_back(n%b);
    n/=b;
}
do
{
		ans[num++] = n%b;
		n /= b;

} while (n != 0);
```

7.3 位运算应用

```c++
//1.不引入第三变量进行交换
void swap(int a,int b)
{
    a^=b;
    b^=a;
    a^=b;
}
//2.求计数和偶数 检查二进制最后一位是否为1
if (a&1==0) printf("偶数")
else printf("奇数")
//3.移位操作
```



7.hashtable数组的映射可以采用`unordered_map<string,int> mp`，**对于判断两个节点存在关系条件的防止内存超限使用unordered_map<int,bool> 代替二维数组`arr[a*maxn+b]=arr[b*maxn+a]=true`**;

7.2N皇后与区间贪心    

皇后相同对角线 那么**abs(v[j]-v[t]) == abs(j-t) **                                       

```c++
//回溯N皇后
n个皇后，皇后i的行数为v[i],要求不同行，不同列，不同对角线
用回溯法判断v[i]?=v[j] 来判断是否同行，同列
    for (int i = 0; i < k; i++) {
        cin >> n;
        vector<int> v(n);
        bool result = true;
        for (int j = 0; j < n; j++) {
            cin >> v[j];
            for (int t = 0; t < j; t++) {//与之前对比回溯
                if (v[j] == v[t] || abs(v[j]-v[t]) == abs(j-t)) {
                    result = false;
                    break;//false 代表不是N皇后
                }
            }
        }
//区间贪心 总是选择左端点最大的值排在前面
bool cmp(it a,it b)
{
   if(a.x!=b.x) return a.x>b.x;
    else return a.y<b.y;
}I[maxn]
// 一些输入信息
sort(I,I,maxn)
int ans=1,lastX=I[0].x;
for(int i=1;i<n;i++)
{
    if(I[i].y<lastX)  
        lastX=I[i].x,
        ans++;
    cout<<ans;
}               
```

==7.3 二分法的使用==

```c++
//二分法是left==right mid大了 right=mid-1,mid小了 left=mid+1;
//一些场合可以使用双指针法 i与j
//利用sort实现排序算法
//插入算法
  sort(a, a + i);
//归并排序
int k=1;
for(int i=0;i<n/k;i++)
{
    sort(a+i*k,a+(i+1)*k);
    sort(a+n/k*k,a+n)//处理尾部
}
k*=2;
```

7.5 牛顿迭代法

==牛顿迭代法是原理是根据一个初始点 在该点做切线，切线与 X 轴相交得出下一个迭代点 的坐标，再在 处做切线，依次类推，直到求得满足精度的近似解为止。==

![img](https://oscimg.oschina.net/oscnet/181f9188-8737-405f-9e74-8a88575418e6.gif)



由前面描述知道，牛顿迭代法是用来近似求解方程的，这里有两个点需要说明：

- 为啥要近似求解？很多方程可能无法直接求取其解
- 迭代法非常适合计算机编程实现，实际上计算机编程对于牛顿迭代法广为应用

来看看，数学上如何描述的？

![image-20221002145343747](C:\Users\YeSho\AppData\Roaming\Typora\typora-user-images\image-20221002145343747.png)

其中 f'(x)为函数f(x) 在xn 处的一阶导数，也就是该点的切线。

来简单推一推上面公式的由来，直线函数方程为：

​												y=kx+b

知道一个直线的一个坐标点(xn,f(xn)) 以及斜率f'(xn) 则该直线的方程就很容易可以得知：

![image-20221002145518890](C:\Users\YeSho\AppData\Roaming\Typora\typora-user-images\image-20221002145518890.png)

那么该直线与 轴的交点，就是 y=0也即f'(xn)x+f(xn)-f'(xn)xn=0等式 的解：

![image-20221002145532616](C:\Users\YeSho\AppData\Roaming\Typora\typora-user-images\image-20221002145532616.png)

7.6哈希表

平方探测的公式为**pos=（key+j*j）%msize**

```c++
 int ans = 0;//利用ans计算查找长度
    for (i = 0; i < m; i++)
    {
        cin >> l;
        for (j = 0; j <=msize; j++)//msize次
        {
            ans++;
            pos =(l+ j * j)%msize;
            if (v[pos] == l || v[pos] == 0) break;//
        }
    }
```



==7.树的技巧==

**7.1树的遍历-前序中序转后序：**

**map<int, int> level**; 用map来保存就不用担心超界问题了

```c++
void post(int root,int start,int end，int index)
{  
    if(start>end) return;
    int i=start;
    while(i<end&&pre[root]!=in[i]) i++;
    post(root+1,start,i-1,2*index);
    post(root+1+i-start,i+1,end,2*index+1)
    cout<<pre[root];//level[index]=pre[root];
}
```

**7.2树的遍历-后序中序转前序:**

树的遍历-后序中序转**层序**，只需在转前序的时候，加一个变量index，表示当前的根节点在二叉树所对应的的下标，根节点为0，左子树根节点为2*index，右子树根节点为2*index+1，将转的过程中产生的post[root]的值存储在level[index].

```c++
void pre(int root,int start,int end,int index)
{
    if(start>end) return;
    int i=start;
    while(i<end&&post[root]!=in[i]) i++;
    cout<<post[root];//level[index]=post[root];
    pre(root-1-end+i,start,i-1,2*index);
    pre(root-1,i+1,end,2*index+1);
}
```

**7.3 二叉搜索树的递归建树方法：**

```c++
node* build(node *root,int v)
{
    if(root==NULL)
    {
        root= new node();
        root->v=v;
        root->left=root->right=NULL;
    }
    else if(v<=root->v) root->left=build(root->left,v);
    else root->right=build(root->right,v);
    return root;
}
for(int i=0;i<n;i++)
{
    cin>>t;
    root=build(root,t);
}
//已知一颗二叉排序树（左小于等于，右大于）的先序可以求其后序
void getpost(int root,int end)
{
    if(root>end) return;
    int i=root+1,j=end;
    while(i<=end&&pre[root]>=pre[i]) i++;
    //从前往后找到的第一个大于等于根节点的一定是右子树根节点
    while(j>rootd&&pre[root]<pre[j]) j--;
    //从后往前找到的最后一个小于根节点的一定是左子树右下最后一个结点
   // if(i-j!=-1) return;
    getpost(root+1,j);//j为左子树最后一个节点
    getpost(i,end);//i为右子树第一个节点
    post.push_back(pre[root])
}
//当二叉排序树是完全二叉树时
//二叉排序树的中序序列是有序的，而完全二叉树左孩子编号一定为2*x,右孩子一定为2*x+1,可以通过中序得到层序；
void inorder(int root)
{
	if(root > n) return;
	inorder(2 * root);
	layer[root] = v[index++];
	inorder(2 * root + 1);
}
//二叉搜索树的前序序列，求节点的最近公共子节点是求遍历的序列中第一个处于两者之间的值
```

7.4 创建平衡二叉树模板

```c++
node* LL(node* root)                 
{                                    
    node *temp=root->rchild;
    root->rchild=temp->lchild;
    temp->lchild=root;
    return temp;
}
node* RR(node* root)
{
    node *temp=root->lchild;
    root->lchild=temp->rchild;
    temp->rchild=root;
    return temp;
}
node *LR(node* root)
{
	root->lchild = LL(root->lchild);
	return RR(root);
}
node *RL(node* root)
{
	root->rchild = RR(root->rchild);
	return LL(root);
}
int getheight(node* root)
{
    if(root==NULL) return 0;
    return max(getheight(root->lchild),getheight(root->rchild))+1;
}
node *insert(node* root,int key)
{
    if(root==NULL)
    {
        root=new node;
        *root={key,NULL,NULL};
        return root;
    }
    else if(key<root->key)
    {
        root-lchild=insert(root->lchild,key);
        if(getheight(root->lchild)-getheight(root->rchild)==2)
         root=key<v->lchild->key?RR(root):LR(root);
    }
    else
    {
        root-rchild=insert(root->rchild,key);
        if(getheight(root->lchild)-getheight(root->rchild)==-2)
         root=key>v->rchild->key?LL(root):RL(root);
    }
    return root;
}
for(int i=0;i<n;i++)
{
    cin>>key;
    root=insert(root,key);
}
```

**7.5 堆排序模板**

```c++
const int maxn=110;
int heap[maxn],n=10;
void downadjust(int low,int high)//向下调整
{
  int i=low,j=2*i;//为左孩子
  while(j<=high)//当有左孩子时
  {
    if(j+1<=high&&heap[j+1]>heap[j])  j++;
    if(heap[j]>heap[i])
    {
       swap(heap[i],heap[j]);
       i=j;j=2*j;
    }
    else break;
  }
}
void creatheap()//建堆
{
  for(int i=n/2;i>=1;i--)
    downadjust(i,n);
}
void deletetop()
{
	heap[1] = heap[n--];
	downadjust(1, n);
}
void upadjust(int low, int high)//向上调整
{
	int i = high, j = i / 2;
	while (j >= low)
	{
		if (heap[j] < heap[i])
		{
			swap(heap[i], heap[j]);
			i = j;j /= 2;
		}
		else break;
	}
}
void insert(int x)//添加元素
{
	heap[++n] = x;
	upadjust(1, n);
}
void heapsort()//堆排序
{
   creatheap();
   for(int i=n;i>1;i--)
   {
      swap(heap[i],heap[1]);
      downadjust(1,i-1);
   }
}
```




==#include<bits/stdc++.h> 万能头文件==

==9.vector的使用==

vector可以开始不定义大小，之后用resize 方法分配大小，它都能够直接将所有的值初始化为0 (不用显式地写出来，**默认就是所有的元素为0**，再也不用担心C语言里面出现的那种`int arr[10]`;结果忘记初始化为0导致的各种

```c++
vector<int> v(10);//直接定义长度为10的数组，默认每个值为0
//或者
vector<int> v1; v1.resize(8)将长度改为8
//在定义的时候就可以对vector变量进行初始化
vector<int> v3(100,9);//长度100，初始值都为9
for(auto it=c.begin();it!=c.end();it++) cout<<*it<<" ";//利用迭代器访问vector
```

==10.cctype头文件==

在`cctype`中已经定义好了判断字符应该所属的范围，直接引入这个头文件并且使用里面的函数判断即可，无需自己手写(自己手写有时候可能写错或者漏写~)

```c++
#include<cctype>
int main()
{
    char c;
    cin>>c;
    if(isalpha(c))
        cout<<"c is alpha"
    return 0;
    /*isalpha 字母（包括大小写）islower(小写字母)、isupper(大写字母)、isalnum(字母大小写加数字)
    isblank(空格和\t),isdigit()判断这个字符串是否是纯数字的字符串*/
}
```

`cctype`中除了，上面所说的用来判断某个字符是否是某种类型，还有两个经常用到的函数:` tolower`和`toupper`，作用是将某个字符转为小写或者大写，这样就不用像原来那样手动判断字符c是否是大写，如果是大写字符c=C+32;的方法将`char c `转为小写字符啦~这在字符串处理的题目中也是经常用到:

```c++
char c='A'; char t=tolower(c); cout<<t;
```

==11.c++11新特性==

c++11比如有`auto`、`to_string()`、`stoi`、`stod`、`stof`、`unordered_map`、`unordered_set`之类的

auto 可以让编译器根据初始值类型直接推断变量的类型。

![image-20220410173903599](C:\Users\YeSho\AppData\Roaming\Typora\typora-user-images\image-20220410173903599.png)

```int
int a[4]={0,1,2,3};
for(int i:a) cout<<i<<endl;//基于范围的for循环
for(int &i:a) i=i*2 //可以改变值
//对于容器来说可以用auto
for(auto i:a)
cout<<i<<" ";
//to_string与stoxx
string str="123"
int a=stoi(str);
string s=to_string(123)
```

==12.求一串数字的某个中其中**连续的一段**的方法==（数组子区间求和）:是记录它的坐标`i`之前所有的和`sum[i]`,那么从`i-j`的一段和为`sum[j]-sum[i-1]`,求连续一段和的可以利用二分法(**防止超时**):`func(int i,int &j,int&tempsum)`  

```c++
 for(int i = 1; i <= n; i++) 
 {
        scanf("%d", &sum[i]);
        sum[i] += sum[i-1]; 
 }
```

13.==输出有各种条件格式要求==时，可以用`printf`里嵌套`（if）？（） ：（）` 例如：`printf("%lld/%lld%s", m, n, flag ? ")" : "")`;

12.利用spring导致超时时，可以改`cout`为`printf（）`,**函数体不对参数修改时，无返回值时，多用&引用，防止超时**

```c++
string a = "I am a idiot.";
    printf("%s",a.c_str());
```

14.递归在函数里面递归的增加或减少，不会影响其他分支，特别是只建立一次记录就行,求树的深度或需要树的深度，在递归体内传递参数depth就行，

```c++
for (int i = 0; i < Node[id].child.size(); i++)
	{
		path[nowk] = Node[id].child[i];
		DFS(path[nowk], nowk + 1, sum + Node[path[nowk]].weight);//nowk+1了，但其他i+1后的再次for循环，nowk是未加一的状态
        
        DFS(id,depth+1)//传递depth
	}
```

15.在求很多满足某种条件的值时并且还要将所有的可能排序，可以再输入遍历后提前sort排序；

```c++
for (int i = 0; i < m; i++)
	{
		int id, num,child;
		cin >> id >> num;
		for (int j = 0; j < num; j++)
		{
			cin >> child;
			Node[id].child.push_back(child);
		}
		sort(Node[id].child.begin(), Node[id].child.end(),cmp);//提前排序
	}

```

==16.动态规划==

```c++
//1.最大连续子序列和 a=（-2，11，-4，13，-5，-2） 11+（-4）+13=20
//dp[i]为以a[i]结尾的连续序列,边界dp[0]=a[0];
dp[i]=max(a[i],dp[i-1]+a[i]);
//2.最长递增子序列 a=(1,2,3,-1,-2,7,9) (1,2,3,7,9)
//dp[i]为以a[i]结尾的最长递增子序列长度 边界dp[i]=1
dp[i]=max(1,dp[j]+1) (a[j]<=a[i]&&dp[j]+1>dp[i]);
//3.最长公共子序列 a="sadstroy" ,b="adminsorry" c="adsory" 长度为6
//dp[i][j]表示a的i号之前和b的j号之前的最长公共子序列，边界dp[0][i]=0,dp[i][0]=0;
dp[i][j]=dp[i-1][j-1]+1(a[i]==b[j]);dp[i][j]=max(dp[i][j-1],dp[i-1][j])(a[i]!=b[j]);
//4.最长回文子串 a=patzjujztaccbcc, b=atzjujzta,长度为9,这只是个逻辑是否，dp[i][j]为0或1
//dp[i][j]表示a[i]至a[j]之间的最长回文子串长度，边界dp[i][i]=1,dp[i][i+1]=1(a[i]==a[i+1])
dp[i][j]=dp[i+1][j-1](a[i]==b[j]) dp[i][j]=0(a[i]!=b[j]);
//5.背包问题
//dp[i][v]表示前i件物品恰好装入容量为v的背包中所获得的最大价值，边界dp[0][v]=0;
dp[i][v]=max(dp[i-1][v],dp[i-1][v-w[i]]+c[i]);(01背包)
dp[i][v]=max(dp[i-1][v],dp[i][v-w[i]]+c[i])(完全背包)
//6.数塔DP
//dp[i][j]表示从第i行第j个数字出发到达底层的所有路径的最大和，边界dp[n][j]=f[n][j];
//dp[i][j]=max(dp[i+1][j],dp[i+1][j+1])+f[i][j];
```

17.priority_queue优先级队列的用法

一些题目需要多次比较（例如每次加入数据比较），防止超时可以再构造体内进行排序

==结构体应用==

```c++
struct node
{
    int key,cnt;//先按cnt从大到小排列，cnt想等再按key排列
    bool operator<(const &a) const{
        return (cnt!=a.cnt)?cnt>a.cnt:key>a.key;
    }
}
```

==优先队列==

```c++
priority_queue<int, vectcor<int>  less<int> >q 最大的元素在队首
priority_queue<int, vectcor<int>  greater<int> >q 最小的元素在队首
struct fruit
{
    string name;
    int price;
    friend bool operator<(fruit f1,fruit f2) {return f1.price<f2.price};//记住
}
priority_queue<fruit> q //价格最高的在队首
struct cmp
{
     bool operator() (fruit f1,fruit f2) {return f1.price<f2.price};
}
priority_queue<int, vectcor<int>,cmp >q// 价格最高的在队首
```

18.find的应用

set的find返回的迭代器，string返回为第一次出现位置或子串，map的find的对象为键，返回位置

**`set::count()`是C++ STL中的内置函数，它返回元素在集合中出现的次数。由于set容器仅包含唯一元素，因此只能返回1或0。

# 刷题类型

## 数组

### 二分查找

[在排序数组中查找元素的第一 个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值 target，返回 `[-1, -1]`。
你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。

示例 1：

> 输入：nums = `[5,7,7,8,8,10]`, target = 8
> 输出：`[3,4]`

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507145559.png" alt="image.png" style="zoom:50%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507145852.png" alt="image.png" style="zoom:50%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507145934.png" alt="image.png" style="zoom:60%;" />

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        // [5,7,7,8,8,10]
        //  0 1 2 3 4 5
        int start=lower_bound(nums.begin(),nums.end(),target)-nums.begin(); // 第一个大于等于的位置
        if(start==nums.size()||nums[start]!=target) return {-1,-1}; 

        int end=upper_bound(nums.begin(),nums.end(),target)-nums.begin()-1; // 第一大于的位置-1

        return {start,end};

    }
}; 
```


 
二分查找包含**闭区间**、**左闭右开区间**和**开区间**3种写法

- 闭区间

	```c++
	    // 闭区间写法
	    int lower_bound(vector<int> &nums, int target) {
	        int left = 0, right = (int) nums.size() - 1; // 闭区间 [left, right]
	        while (left <= right) { // 区间不为空
	            // 循环不变量：
	            // nums[left-1] < target
	            // nums[right+1] >= target
	            int mid = left + (right - left) / 2;
	            if (nums[mid] < target)
	                left = mid + 1; // 范围缩小到 [mid+1, right]
	            else
	                right = mid - 1; // 范围缩小到 [left, mid-1]
	        }
	        return left; // 或者 right+1
	    }
	```

- 开区间
	```c++
	int lower_bound3(vector<int> &nums, int target) {
	        int left = -1, right = nums.size(); // 开区间 (left, right)
	        while (left + 1 < right) { // 区间不为空
	            // 循环不变量：
	            // nums[left] < target
	            // nums[right] >= target
	            int mid = left + (right - left) / 2;
	            if (nums[mid] < target)
	                left = mid; // 范围缩小到 (mid, right)
	            else
	                right = mid; // 范围缩小到 (left, mid)
	        }
	        return right; // 或者 left+1
	    }
	```

- 左闭右开
	```c++
	// 左闭右开区间写法
	    int lower_bound2(vector<int> &nums, int target) {
	        int left = 0, right = nums.size(); // 左闭右开区间 [left, right)
	        while (left < right) { // 区间不为空
	            // 循环不变量：
	            // nums[left-1] < target
	            // nums[right] >= target
	            int mid = left + (right - left) / 2;
	            if (nums[mid] < target)
	                left = mid + 1; // 范围缩小到 [mid+1, right)
	            else
	                right = mid; // 范围缩小到 [left, mid)
	        }
	        return left; // 或者 right
	    }
	```


牛顿迭代法 


一些trick公式
$(n+1)^2-n^2=2n+1$


[寻找峰值](https://leetcode.cn/problems/find-peak-element/description/)

峰值元素是指其值严格大于左右相邻值的元素。
给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。
你可以假设 `nums[-1] = nums[n] = -∞` 。
你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。

示例 1：

> 输入：`nums = [1,2,3,1]`
> 输出：2
> 解释：3 是峰值元素，你的函数应该返回其索引2。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507171446.png" alt="image.png" style="zoom:50%;" />


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507171553.png" alt="image.png" style="zoom:50%;" />

其实就是当我们站在一个位置时，如果它的右边比它大，那么它右边一定存在峰值，同样的如果它的右边比它小，那么它左边一定存在峰值

```c++
    int findPeakElement(vector<int>& nums) {
        int search(vector<int> & nums, int target) {
             int left = -1, right = nums.size() - 1; // 开区间 (-1, n-1)
        while (left + 1 < right) { // 开区间不为空
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[mid + 1]) right = mid; // 蓝色
            else left = mid; // 红色
        }
        return right;
    }
```

```go
func findPeakElement(nums []int) int {
    // [-100,1,2,3,1,-100]

    // 一个位置mid
    // mid<mid+1 结果一定在右边
    // mid>mid+1

    left,right:=0,len(nums)-1
    for left<right{
        mid:=left+(right-left)/2
        if nums[mid]<nums[mid+1]{
            left=mid+1
        }else{
            right=mid
        }
    }
    return left
}
```

### 滑动窗口

滑动窗口：就是不断的调节子序列的起始位置和终止位置，从而得出我们要想的结果

<img src="https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif" alt="image.png" style="zoom:80%;" />

可以发现滑动窗口也可以理解为双指针法的一种！只不过这种解法更像是一个窗口的移动，所以叫做滑动窗口更适合一些。

实现滑动窗口，主要确定如下三点：
- 窗口内是什么？
- 如何移动窗口的起始位置？
- 如何移动窗口的结束位置？


滑动窗口模板

```c++
/* 滑动窗口算法框架 */
void slidingWindow(string s) {
    unordered_map<char, int> window;
    
    int left = 0, right = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/
        
        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}

```

python前缀和

```python
for i in range(n):
            sums.append(sums[-1] + nums[i])
```


滑动窗口一些常用的STL

```c++
unordered_map 用来保存一些判断用的逻辑 
查找是否存在用count方法，不要用find
可以经常用size()判断数量
```

```python
可以用Counter直接得到一个键值对，value就是出现次数 
k,v in need.items() 
查找是否存在用 [not] in need.keys()
```

### 螺旋矩阵

这类题目就是，顺时针打印矩阵的顺序是 **“从左向右、从上向下、从右向左、从下向上”** 循环。

```c++
int left=0,right=n-1,top=0,bottom=m-1;
```

<img src="https://pic.leetcode-cn.com/ccff416fa39887c938d36fec8e490e1861813d3bba7836eda941426f13420759-Picture1.png" alt="image.png" style="zoom:60%;" />

```c++
vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>>result(n,vector<int>(n));
        int left=0,right=n-1,top=0,bottom=n-1;
        int num=1;
        while(num<=n*n){
            // 从左向右
            for(int i=left;i<=right;++i) result[top][i]=num++;
            ++top;
            // 从上到下
            for(int i=top;i<=bottom;++i) result[i][right]=num++;
            --right;

            // 从右向左
            for(int i=right;i>=left;i--) result[bottom][i]=num++;
            --bottom;

            // 从下到上
            for(int i=bottom;i>=top;i--) result[i][left]=num++;
            ++left;

        }
        return result;
```


### 下一个排列

整数数组的**下一个排列**是指其整数的下一个字典序更大的排列。

关键点：
1. 找到需要改变的最右边的位置（第一个下降的元素）。
2. 在该位置右边找到刚好大于它的数，交换它们。
3. 将该位置右边的序列反转，使其成为最小的排列。

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n=nums.size();
        int j=n-2;

        // 找到第一个下降的元素
        while(j>=0&&nums[j+1]<=nums[j]){
            --j;
        }
        if(j>=0){
            int pos=n-1;
            while(nums[pos]<=nums[j]){
                --pos;
            }
            swap(nums[j],nums[pos]);
        }
        reverse(nums.begin()+j+1,nums.end());
    }
};
```


```text
步骤 1: 找到第一个下降的元素
从右向左遍历，找到第一个比右邻居小的元素。
[1, 2, 3, 5, 4]
^
j = 2 (指向3)，因为3 < 5

步骤 2: 在j右侧找到比nums[j]大的最小元素
在[5,4]中找到比3大的最小数，是4
[1, 2, 3, 5, 4]
^
pos = 4 (指向4)

步骤 3: 交换这两个元素
交换nums[j]和nums[pos]
[1, 2, 4, 5, 3]

步骤 4: 反转j之后的所有元素
反转[5,3]
[1, 2, 4, 3, 5]

这就是最终的下一个排列。
```


## 链表

### 链表的理论基础


- 链表的定义
	```c
	// 单链表
	struct ListNode {
	    int val;  // 节点上存储的元素
	    ListNode *next;  // 指向下一个节点的指针
	};
```


常用栈或者其他结构来解决

可以用递归解决一些问题

### 反转链表

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

<img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" alt="image.png" style="zoom:60%;" />

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507184413.png" alt="image.png" style="zoom:60%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240507184443.png" alt="image.png" style="zoom:60%;" />

> 反转结束后,从原来的链表上看:
> pre指向反转这一段的末尾
> cur指向反转这一段后续的下一个节点


```C++
ListNode *reverseList(ListNode *head) {
	ListNode *pre = nullptr, *cur = head;
	while (cur) {
		ListNode *nxt = cur->next;
		cur->next = pre;
		pre = cur;
		cur = nxt;
	}
	return pre;
}
```


[翻转链表2](https://leetcode.cn/problems/reverse-linked-list-ii/description/)

给你单链表的头指针 head 和两个整数left和right，其中 `left <= right` 。请你反转从位置left到位置right的链表节点，返回 反转后的链表 。

<img src="https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg" alt="image.png" style="zoom:60%;" />

**输入：** head = `[1,2,3,4,5]`, left = 2, right = 4
**输出：** `[1,4,3,2,5]`

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240508113037.png" alt="image.png" style="zoom:40%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240508113105.png" alt="image.png" style="zoom:40%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240508113157.png" alt="image.png" style="zoom:50%;" />

```c++
class Solution {
public:
    ListNode *reverseBetween(ListNode *head, int left, int right) {
        ListNode *dummy = new ListNode(0, head), *p0 = dummy;
        for (int i = 0; i < left - 1; ++i)
            p0 = p0->next;

        ListNode *pre = nullptr, *cur = p0->next;
        for (int i = 0; i < right - left + 1; ++i) {
            ListNode *nxt = cur->next;
            cur->next = pre; // 每次循环只修改一个 next，方便大家理解
            pre = cur;
            cur = nxt;
        }

        // 见视频
        p0->next->next = cur;
        p0->next = pre;
        return dummy->next;
    }
};
```


### 快慢指针

[链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/description/)

给你单链表的头结点 head ，请你找出并返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。
<img src="https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg" alt="image.png" style="zoom:60%;" />

> 输入：head = `[1,2,3,4,5]`
> 输出：`[3,4,5]`
> 解释：链表只有一个中间结点，值为3 。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240511233842.png" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240511233859.png" alt="image.png" style="zoom:60%;" />
```c++
class Solution {
public:
    ListNode *middleNode(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```

[环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/ "https://leetcode.cn/problems/linked-list-cycle-ii/description/")

给定一个链表的头节点`head`，返回链表开始入环的第一个节点。 如果链表无环，则返回`null`。
如果链表中有某个节点，可以通过连续跟踪`next`指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果`pos`是-1，则在该链表中没有环。注意：pos不作为参数进行传递，仅仅是为了标识链表的实际情况。
不允许修改 链表。
<img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png" alt="image.png" style="zoom:60%;" />
> 输入：head = `[3,2,0,-4]`, pos = 1
> 输出：返回索引为 1 的链表节点
> 解释：链表中有一个环，其尾部连接到第二个节点。


<img src="https://pic.leetcode-cn.com/a4788076d4f3ad247c2023f92bb1585d05c5132ece7ed1205e2e171e25648adc-Picture1.png" alt="image.png" style="zoom:40%;" />

$a + b = c$ fast 走的步数是 slow 步数的2倍，f走了$2x$步，s走了$x$步。假设在环的起点开始$k$处相遇，则有：$a + b + k = 2x$，$a + k = x$
所以$x = b$，$x$的起点是从head开始，而$b$长度从环节点开始，它们长度相同，起点不同，终点也不同，起点开始的差距也就是他们终点的差距，所以head和slow在$k$处一起走相遇的位置就是环的起点

```c++
        // 快慢指针 
        ListNode* slow=head,*fast=head;
        while (fast&&fast->next) {
            fast=fast->next->next;
            slow=slow->next;
            if(slow==fast){
                while (slow!=head) {
                    slow=slow->next;
                    head=head->next;
                }
                return head;
            }
        }
        return nullptr;
```

### 链表删除

[删除排序链表中的重复元素II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/)

<img src="https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg" alt="image.png" style="zoom:60%;" />
**输入：** head = `[1,2,3,3,4,4,5]`
**输出：** `[1,2,5]`

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240512080451.png" alt="image.png" style="zoom:40%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240512080634.png" alt="image.png" style="zoom:40%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240512080712.png" alt="image.png" style="zoom:40%;" />

```c++
class Solution {
public:
    ListNode *deleteDuplicates(ListNode *head) {
        ListNode *dummy = new ListNode(0, head), *cur = dummy;
        while (cur->next && cur->next->next) {
            int val = cur->next->val;
            if (cur->next->next->val == val)
                while (cur->next && cur->next->val == val)
                    cur->next = cur->next->next; 
            else
                cur = cur->next;
        }
        return dummy->next;
    }
};
```

### 一些重要技能点

#### 链表相交


设「第一个公共节点」为 node ，「链表 headA」的节点数量为 a ，「链表 headB」的节点数量为 b ，「两链表的公共尾部」的节点数量为 c ，则有：

- 头节点 headA 到 node 前，共有 `a−c` 个节点；
- 头节点 headB 到 node 前，共有 `b−c` 个节点；

<img src="https://pic.leetcode-cn.com/1615224578-EBRtwv-Picture1.png" alt="image.png" style="zoom:40%;" />

考虑构建两个节点指针 A​ , B 分别指向两链表头节点 `headA` , `headB` ，做如下操作：

- 指针 A 先遍历完链表`headA` ，再开始遍历链表`headB` ，当走到`node`时，共走步数为：
$$a+(b−c)$$
- 指针 B 先遍历完链表`headB`，再开始遍历链表`headA`，当走到`node`时，共走步数为：
$$b+(a−c)$$
如下式所示，此时指针A , B 重合，并有两种情况：

$$a+(b−c)=b+(a−c)$$
1. 若两链表 有 公共尾部 (即 `c>0` ) ：指针 A , B 同时指向「第一个公共节点」node 。
2. 若两链表 无 公共尾部 (即 `c=0` ) ：指针 A , B 同时指向 `null` 。

```c++
        // 思路法
        auto p1 = headA,p2 = headB;
        while(p1!=p2){
            p1=p1==nullptr?headB:p1->next;
            p2=p2==nullptr?headA:p2->next;
        }   
        return p1;
```

## 哈希表

### 哈希表理论基础

哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素，如下图所示：

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210104234805168.png" alt="image.png" style="zoom:60%;" />

**一般哈希表都是用来快速判断一个元素是否出现集合里。**

### 哈希函数

哈希函数，把学生的姓名直接映射为哈希表上的索引，然后就可以通过查询索引下标快速知道这位同学是否在这所学校里了。

哈希函数如下图所示，通过hashCode把名字转化为数值，一般hashcode是通过特定编码方式，可以将其他数据格式转化为不同的数值，这样就把学生名字映射为哈希表上的索引数字了。

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/2021010423484818.png" alt="image.png" style="zoom:60%;" />


### 常见的三种哈希结构
当我们想使用哈希法来解决问题的时候，我们一般会选择如下三种数据结构。

- 数组
- set(集合）
- map(映射)

在C++中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合                 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率    | 增删效率    |
| ------------------ | ---- | ---- | -------- | ------ | ------- | ------- |
| std::set           | 红黑树  | 有序   | 否        | 否      | O(logn) | O(logn) |
| std::multiset      | 红黑树  | 有序   | 是        | 否      | O(logn) | O(logn) |
| std::unordered_set | 哈希表  | 无序   | 否        | 否      | O(1)    | O(1)    |
一些去重操作可以借助于set

## 字符串

### stringstream

`std::stringstream`是一个可以同时进行输入和输出操作的字符串流。它非常灵活，可以用来读取字符串中的数据，也可以向其中添加数据，就像使用文件流或标准输入/输出一样。`std::stringstream`经常被用于数据的转换和字符串的拼接。

例如，将整数转换为字符串，或者从字符串中解析出整数：

```c
#include <iostream>
#include <sstream>
#include <string>

int main() {
    std::stringstream ss;
    ss << 100; // 向stringstream写入整数100
    std::string str = ss.str(); // 从stringstream获取字符串表示
    std::cout << str << std::endl; // 输出: 100

    ss.str(""); // 清空stringstream
    ss << "200";
    int num;
    ss >> num; // 从字符串"200"中读取整数到num
    std::cout << num << std::endl; // 输出: 200

    return 0;
}
```

通过提供字符串版本的流操作，极大地简化了字符串处理的复杂性，特别是在需要将字符串与其他数据类型之间相互转换时。

`>>`操作符进行输入时，它默认会跳过前导的空白字符（包括空格、制表符和换行符），并且在遇到下一个空白字符时停止读取。这意味着，使用`>>`操作符读取字符串数据时，它会读取空白字符之前的字符序列作为字符串。

```cpp
    // 辅助函数：分割字符串
    vector<string> split(string s, char delimiter) {
        vector<string> tokens;
        string token;
        stringstream tokenStream(s);
        while (getline(tokenStream, token, delimiter)) {
            tokens.push_back(token);
        }
        return tokens;
    }
```


### 一些技巧

1. 原地左移字符串或者右移字符串k位
	- 先在临界点各自反转，再整体反转

	```c++
	  int n;
	  cin>>n;
	  string s;
	  cin>>s;
	  auto len=s.length();
	  std::reverse(s.begin(),s.begin()+len-n);
	  std::reverse(s.begin()+len-n,s.end());
	  std::reverse(s.begin(),s.end());
	  cout<<s;
	```

#### kmp
	
- next数组就是一个前缀表（prefix table）,**前缀表是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配。**
- 前缀表统一减一

```c++
class KMP {
private:
    std::vector<int> next;

    void computeNext(const std::string& pattern) {
        int m = pattern.length();
        next.resize(m);
        next[0] = 0;
        int len = 0;
        int i = 1;

        while (i < m) {
            if (pattern[i] == pattern[len]) {
                len++;
                next[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = next[len - 1];
                } else {
                    next[i] = 0;
                    i++;
                }
            }
        }
    }

public:
    std::vector<int> search(const std::string& text, const std::string& pattern) {
        std::vector<int> result;
        int n = text.length();
        int m = pattern.length();

        computeNext(pattern);

        int i = 0; // index for text
        int j = 0; // index for pattern

        while (i < n) {
            if (pattern[j] == text[i]) {
                j++;
                i++;
            }

            if (j == m) {
                result.push_back(i - j);
                j = next[j - 1];
            } else if (i < n && pattern[j] != text[i]) {
                if (j != 0) {
                    j = next[j - 1];
                } else {
                    i++;
                }
            }
        }

        return result;
    }
};
```


## 双指针

### 相向双指针

[167.两数之和II-输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/description/)

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240421153507.png?" alt="image.png" style="zoom:50%;" />


✍️**这个数组是已经排好序的，要利用这个性质**。

- 2和8大于9，2-8之前的任何两个数相加都会大于9，8和中间任何数相加大于9，所以把8去掉，
- 2和6相加小于9，2和中间的任何数相加小于9，所以把2去掉，这里就用了双向双指针的做法

- 用一个获取了多少的信息量来衡量一个算法的效率
	- 暴力做法是找两个数加起来和9比一比,那它花费O(1)的时间就只知道了O(1)的信息
	- 优化做法是是把当前剩下的最小的数和最大的数加起来和9比一比就知道其中一个数和其它任何一个数相加都是小于9的，所以这是花费O(1)的时间就只知道了O(n)的信息

```c++
    vector<int> twoSum(vector<int> &numbers, int target)    
    {
        int n=numbers.size();
        int i=0,j=n-1;
        while(i<=j){
            if(numbers[i]+numbers[j]>target){
                j--;
            }
            else if(numbers[i]+numbers[j]<target){
                i++;
            }
            else {
                return {i+1,j+1};
            }
        }
        return {0,0};
    }
```

[三数之和](https://leetcode.cn/problems/3sum/description/)

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

我们枚举i,这个问题就变成两数之和了。

```c++
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n=nums.size();
        sort(nums.begin(),nums.end());
        vector<vector<int>>res;
        // [-4,-1,-1,0,1,2]
        // i<j<k;
        for(int i=0;i<n-2;i++){
            if(i>0&&nums[i-1]==nums[i]) continue;
            if(nums[i]+nums[i+1]+nums[i+2]>0) break;
            if(nums[i]+nums[n-1]+nums[n-2]<0) continue;
            int target=-nums[i];
            int j=i+1,k=n-1;
            while(j<k){
                if(nums[j]+nums[k]==target) 
                {
                    res.push_back({nums[i],nums[j],nums[k]});
                    j++;
                    while(j<k&&nums[j]==nums[j-1]) j++;
                    k--;
                    while(j<k&&nums[k]==nums[k+1]) k--;
                }
                else nums[j]+nums[k]>target?--k:++j;
            }
        }
        return res;
    }
```


[盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/description/)

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。
找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
**说明：** 你不能倾斜容器。

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg" alt="image.png" style="zoom:60%;" />

这道题要求最大值，我们遍历的时候不断判断得到最大值。
我们用相向双指针，移动的条件时什么？
那边的高度低，移动那边的值,反证法：移动高度高的一边的结果有两种：
1. 移动后的高度仍然大于原来高度低的一边，因为雨水的高度取决于高度低的一方，所以高度不变，但宽度减少，一定不如原来的面积大；
2. 移动后的高度仍然小于原来高度低的一边，那面积肯定更小了，高度、宽度都减少了

所以那边的高度低，移动那边的值。

```c
int maxArea(vector<int>& height) {
        int n = height.size();
        // [1,8,6,2,5,4,8,3,7]
        //  0 1 2 3 4 5 6 7 8
        // 1个位置，左边比它大 右边
        int sum = 0;

        int l = 0, r = n - 1;
        while (l < r) {
            sum = max(sum, (r - l) * min(height[l], height[r]));
            if (height[l] < height[r]) {
                l++;
            } else {
                --r;
            }
        }
        return sum;
    }
```

[接雨水](https://leetcode.cn/problems/trapping-rain-water/description/)

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png" alt="image.png" style="zoom:90%;" />


```c++
 int trap(vector<int>& height) {
        int n = height.size();
#if 0 // 前后缀分解
      // [0,1,0,2,1,0,1,3,2,1,2,1] 数组
      // [0,1,1,2,2,2,2,3,3,3,3,3] 前缀最大值
      // [3,3,3,3,3,3,3,3,2,2,2,1] 后缀最大值
    // 假设每个位置的桶宽度为1，那么一个能接雨水=(min(左边的最大高度,右边最大高度)-自身高度)*1
    int prev=height[0];
    vector<int>pre_num(n,0);
    vector<int>post_num(n,0);
    for(int i=0;i<n;i++){
        if (height[i]>=prev) {
            pre_num[i]=height[i];
            prev=height[i];
        }
        else {
            pre_num[i]=prev;
        }
    }
    prev=height[n-1];
    for(int i=n-1;i>=0;i--){
        if(height[i]>=prev){
            post_num[i]=height[i];
            prev=height[i];
        }
        else {
            post_num[i]=prev;
        }
    }
    int sum=0;
    for (int i=0; i<n; ++i) {
        sum+=(min(pre_num[i],post_num[i])-height[i]);
    }
    return sum;
#endif

#if 1 // 双指针

        // 因为我们是取一个点的前缀和后缀的最大值的小的一方，
        //当前缀最大值小于一个后缀值，但不一定后缀最大值时，就可以停止了，
        //因为小的一方已经确定了，这里采用双指针做法
        int l = 0, r = n - 1;
        int pre_max = 0, post_max = 0;
        int sum = 0;
        while (l < r) {
            pre_max = max(pre_max, height[l]);
            post_max = max(post_max, height[r]);
            if (pre_max < post_max) {
                sum += pre_max - height[l];
                l++;
            } else {
                sum += post_max - height[r];
                r--;
            }
        }
        return sum;
#endif
}
```

这道题还可以用[[pat算法笔记#栈的运用|单调栈]]来做

## 栈与队列

### 栈与队列的互相实现

- 用栈实现队列

<img src="https://code-thinking.cdn.bcebos.com/gifs/232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97%E7%89%88%E6%9C%AC2.gif" alt="image.png" style="zoom:60%;" />

```c++
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        // 只有当stOut为空的时候，再从stIn里导入数据（导入stIn全部数据）
        if (stOut.empty()) {
            // 从stIn导入数据直到stIn为空
            while(!stIn.empty()) {
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }
```


<img src="https://code-thinking.cdn.bcebos.com/gifs/225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.gif" alt="image.png" style="zoom:60%;" />

```c++
/** Removes the element on top of the stack and returns that element. */
    int pop() {
        int size = que1.size();
        size--;
        while (size--) { // 将que1 导入que2，但要留下最后一个元素
            que2.push(que1.front());
            que1.pop();
        }

        int result = que1.front(); // 留下的最后一个元素就是要返回的值
        que1.pop();
        que1 = que2;            // 再将que2赋值给que1
        while (!que2.empty()) { // 清空que2
            que2.pop();
        }
        return result;
    }
```


### 栈的运用

1. 匹配括号



	<img src="https://code-thinking.cdn.bcebos.com/gifs/225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.gif" alt="image.png" style="zoom:60%;" />
	```c
	class Solution {
	public:
	    bool isValid(string s) {
	        if (s.size() % 2 != 0) return false; // 如果s的长度为奇数，一定不符合要求
	        stack<char> st;
	        for (int i = 0; i < s.size(); i++) {
	            if (s[i] == '(') st.push(')');
	            else if (s[i] == '{') st.push('}');
	            else if (s[i] == '[') st.push(']');
	            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
	            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
	            else if (st.empty() || st.top() != s[i]) return false;
	            else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
	        }
	        // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
	        return st.empty();
	    }
	};
	```

2. 中缀算术表达式
	逆波兰表达式，也叫做后缀表达式。
	我们平时见到的运算表达式是中缀表达式，即 "`操作数① 运算符② 操作数③`" 的顺序，运算符在两个操作数中间。
	但是后缀表达式是 "`操作数① 操作数③ 运算符②`" 的顺序，运算符在两个操作数之后。
	- 中缀表达式是其对应的语法树的中序遍历；
	- 后缀表达式是其对应的语法树的后序遍历；
	- 前缀表达式是其对应的语法树的前序遍历；

	下图中左边是中缀表达式，中间是其对应的语法树，右边是语法树转成的后缀表达式。

	<img src="http://pic.leetcode-cn.com/1616207537-QbiuhP-image.png" alt="image.png" style="zoom:70%;" />

	- 对逆波兰表达式求值的过程是：
		- 如果遇到数字就进栈；
		- 如果遇到操作符，就从栈顶弹出两个数字分别为 num2（栈顶）、num1（栈中的第二个元素）；计算 num1 运算 num2

	```c++
	stack<long long> st; 
	        for (int i = 0; i < tokens.size(); i++) {
	            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/") {
	                long long num1 = st.top();
	                st.pop();
	                long long num2 = st.top();
	                st.pop();
	                if (tokens[i] == "+") st.push(num2 + num1);
	                if (tokens[i] == "-") st.push(num2 - num1);
	                if (tokens[i] == "*") st.push(num2 * num1);
	                if (tokens[i] == "/") st.push(num2 / num1);
	            } else {
	                st.push(stoll(tokens[i]));
	            }
	        }
	
	        int result = st.top();
	        st.pop(); // 把栈里最后一个元素弹出（其实不弹出也没事）
	        return result;
```


3. 中缀表达式改后缀表达式

思路如下：
- 如果是数字，直接加入后缀表达式
- 如果是左括号，压入栈中。
- 如果是右括号，弹出栈中元素直到遇到左括号，将弹出的运算符加入后缀表达式。
- 如果是运算符，比较其与栈顶运算符的优先级。如果当前运算符优先级低于或等于栈顶运算符，则弹出栈顶运算符并加入后缀表达式，重复此过程。然后将当前运算符压入栈中。

```c++
class Solution {
private:
    // 判断运算符优先级
    int precedence(char op) {
        if (op == '+' || op == '-')
            return 1;
        if (op == '*' || op == '/')
            return 2;
        return 0;
    }

public:
    std::string infixToPostfix(std::string infix) {
        std::stack<char> stack;
        std::string postfix = "";
        
        for (char& c : infix) {
            // 如果是数字，直接加入后缀表达式
            if (isalnum(c)) {
                postfix += c;
            }
            // 如果是左括号，压入栈中
            else if (c == '(') {
                stack.push(c);
            }
            // 如果是右括号，弹出栈中元素直到遇到左括号
            else if (c == ')') {
                while (!stack.empty() && stack.top() != '(') {
                    postfix += stack.top();
                    stack.pop();
                }
                if (!stack.empty() && stack.top() == '(')
                    stack.pop();
            }
            // 如果是运算符
            else {
                while (!stack.empty() && precedence(c) <= precedence(stack.top())) {
                    postfix += stack.top();
                    stack.pop();
                }
                stack.push(c);
            }
        }
        
        // 将栈中剩余的运算符加入后缀表达式
        while (!stack.empty()) {
            postfix += stack.top();
            stack.pop();
        }
        
        return postfix;
    }
};

int main() {
    Solution solution;
    std::string infix = "a+b*(c^d-e)^(f+g*h)-i";
    std::string postfix = solution.infixToPostfix(infix);
    std::cout << "Infix: " << infix << std::endl;
    std::cout << "Postfix: " << postfix << std::endl;
    return 0;
}
```


## 单调栈

这是单调递减
```c++
            while (!st.empty() && nums[st.top()] <= nums[i]) {
                st.pop();
                // 其他逻辑
            }
            st.push_back(i);
```

这是单调递增 
```c++
            while (!st.empty() && nums[st.top()] >= nums[i]) {
                st.pop();
                // 其他逻辑
            }
            st.push_back(i);
```

[每日温度](https://leetcode.cn/problems/daily-temperatures/)

给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 `answer[i]` 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。

**示例 1:**

> **输入:** `temperatures` = `[73,74,75,71,69,72,76,73]`
> **输出:** `[1,1,4,2,1,1,0,0]`

从右向左
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240517054511.png" alt="image.png" style="zoom:40%;" />
- 想象成一座座山,我们只往上看,不往下看。
- 由于5的出现,2和3被挡住了,**2和3永远不可能是5之前某个数的下一个更大的数**，把这两个数划掉
- 继续向左遍历,永远无法再看到2和3了。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240401105136.png" alt="image.png" style="zoom:40%;" />

- 及时去掉无用数据，
- 保证栈中数据有序。


```c++
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int len = temperatures.size();
        vector<int> res(len, 0);
        stack<int> st;
#if 0 // 单调栈
        
        for (int i = len - 1; i >= 0; i--) {
            // 出栈
            while (!st.empty() && temperatures[i] > temperatures[st.top()])
                st.pop();
            // 记录值
            if (!st.empty())
                res[i] = st.top() - i;
            // 入栈
            st.push(i);
        }
#endif
```

从左向右
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240517055335.png" alt="image.png" style="zoom:40%;" />

- 对于4和3来说,5就是它们的下一个更高温度。
- 既然答案已经算出来了,也就不需要在栈里了。
- 从左到右，栈里面存储的数都没找到更大的数，一旦找到一个比栈顶大的数，就立刻更新栈顶的答案，同时栈顶元素出栈

```c++
#if 1 // 单调栈

        for(int i=0;i<len;++i){
            while(!st.empty()&&temperatures[i]>temperatures[st.top()]){
                int index=st.top();
                st.pop();
                res[index]=i-index;
            }
            st.push(i);
        }

#endif
```


[接雨水](https://leetcode.cn/problems/trapping-rain-water/)

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png" alt="image.png" style="zoom:80%;" />

> **输入：** height = `[0,1,0,2,1,0,1,3,2,1,2,1]`
> **输出：** 6
> **解释：** 上面是由数组 `[0,1,0,2,1,0,1,3,2,1,2,1]` 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240517060902.png" alt="image.png" style="zoom:40%;" />
面积由当前元素下标、栈顶元素小标、栈顶下面元素的下标

```c++
#if 0 // 单调栈
        // 遍历这个数组，如果一个坐标i    height[i-1]<height[i]<height[i+1]
        // height[i]=min(height[i-1],height[i+1]) 
        int res=0;
        stack<int>st;
        for(int i=0;i<height.size();++i){
            while(!st.empty()&&height[i]>height[st.top()]){
                auto index=height[st.top()];
                st.pop();
                if(st.empty()) break;
                auto left=st.top();
                res+=((i-left-1)*(min(height[left],height[i])-index));

            }
            st.push(i);
        }
        return res;
#endif
```

### 单调队列


单调队列 = 单调栈 + 滑动窗口
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240331162907.png" alt="image.png" style="zoom:60%;" />

- 及时去掉无用数据，保证双端队列有序
	- 当前数字>队尾，弹出队尾 (和单调栈一样)。
	- 弹出队首不在窗口内的元素

这是单调递减
```c++
            while (!dq.empty() && nums[dq.back()] <= nums[i]) {
                dq.pop_back();
                // 其他逻辑
            }
            dq.push_back(i);
```

这是单调递增

```c++
            while (!dq.empty() && nums[dq.back()] >= nums[i]) {
                dq.pop_back();、
                // 其他逻辑
            }
            dq.push_back(i);
```


[滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 `_`滑动窗口中的最大值_

**示例 1：**
```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240517061758.png" alt="image.png" style="zoom:60%;" />
由于4的出现,左边的2和1, 比4**小**,离开窗口的时间又比4**早**, 所以永远不会作为窗口最大值了。

```c++
#if 1 // 用单调队列
        deque<int> dq;
        for (int i = 0; i < nums.size(); ++i) {
            // 出
            if (i >= k && (i-dq.front())>=k) {
                dq.pop_front();
            }
            // 进
            while (!dq.empty() && nums[dq.back()] <= nums[i]) {
                dq.pop_back();
            }
            dq.push_back(i);
            // 记录答案
            if (i >= k - 1) {
                res.push_back(nums[dq.front()]);
            }
        }
        return res;
#endif
```

### 优先队列

```c++
//小顶堆
priority_queue <int,vector<int>,greater<int> > q;
//大顶堆
priority_queue <int,vector<int>,less<int> >q;
//默认大顶堆
priority_queue<int> a;
```


```c++
static bool cmp(pair<int, int>& m, pair<int, int>& n) {
	return m.second > n.second;}
priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> q(cmp);
```


## 二叉树

### 二叉树理论基础篇

#### 二叉树的种类


1. 满二叉树
	只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，深度为k，有$2^k-1$个节点的二叉树

	<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806185805576.png" alt="image.png" style="zoom:60%;" />

2. 完全二叉树
	除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置
	**堆就是一棵完全二叉树**
	
	<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200920221638903.png" alt="image.png" style="zoom:60%;" />
3. 二叉搜索树
	**二叉搜索树是一个有序树**
	- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
	- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
	- 它的左、右子树也分别为二叉排序树
	- 进行中序遍历时，遍历得到的节点值序列应该是严格递增的。 

	<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190304693.png" alt="image.png" style="zoom:60%;" />

4. 平衡二叉搜索树
	它是一棵空树或它的左右两个子树的**高度差的绝对值不超过1**，并且左右两个子树都是一棵平衡二叉树。
	C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树，所以map、set的增删操作时间时间复杂度是logn
	<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190511967.png" alt="image.png" style="zoom:60%;" />

#### 二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
2. 广度优先遍历：一层一层的去遍历

- 深度优先遍历
    - 前序遍历（递归法，迭代法）
    - 中序遍历（递归法，迭代法）
    - 后序遍历（递归法，迭代法）
- 广度优先遍历
    - 层次遍历（迭代法）

	<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806191109896.png" alt="image.png" style="zoom:50%;" />

**二叉树的定义**
```c
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

### 递归的本质


- 原问题:计算整棵树的最大深度
- 子问题:计算左/右子树的最大深度
- **子问题**与**原问题**是相似的
- 类比循环，执行的代码也应该是相同的但子问题需要把计算结果**返给上一级**问题
- 这更适合用**递归**实现

- 由于子问题的规模比原问题**小**
- 不断**递**下去，总会有个**尽头**
- 即递归的**边界条件**(base case)
- 直接返回它的答案(**归**)


### 二叉树的递归遍历

**前序遍历**：

```c
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // 中
        traversal(cur->left, vec);  // 左
        traversal(cur->right, vec); // 右
    }
```

**中序遍历**：

```c
void traversal(TreeNode* cur, vector<int>& vec) {
    if (cur == NULL) return;
    traversal(cur->left, vec);  // 左
    vec.push_back(cur->val);    // 中
    traversal(cur->right, vec); // 右
}
```

**后序遍历**：

```c
void traversal(TreeNode* cur, vector<int>& vec) {
    if (cur == NULL) return;
    traversal(cur->left, vec);  // 左
    traversal(cur->right, vec); // 右
    vec.push_back(cur->val);    // 中
}
```

**前序遍历（迭代法）**

<img src="https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif" alt="image.png" style="zoom:60%;" />
```c++
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                       // 中
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);           // 右（空节点不入栈）
            if (node->left) st.push(node->left);             // 左（空节点不入栈）
        }
        return result;
    }
```

中序遍历（迭代法）

**在使用迭代法写中序遍历，就需要借用指针的遍历来帮助访问节点，栈则用来处理节点上的元素**

<img src="https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif" alt="image.png" style="zoom:60%;" />
```c++
vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
                st.pop();
                result.push_back(cur->val);     // 中
                cur = cur->right;               // 右
            }
        }
        return result;
    }
```

后序遍历（迭代法）

```c++
vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序 （空节点不入栈）
            if (node->right) st.push(node->right); // 空节点不入栈
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
```


统一写法
- 迭代法中序遍历
	```c++
	vector<int> inorderTraversal(TreeNode* root) {
	        vector<int> result;
	        stack<TreeNode*> st;
	        if (root != NULL) st.push(root);
	        while (!st.empty()) {
	            TreeNode* node = st.top();
	            if (node != NULL) {
	                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
	                if (node->right) st.push(node->right);  // 添加右节点（空节点不入栈）
	
	                st.push(node);                          // 添加中节点
	                st.push(NULL); // 中节点访问过，但是还没有处理，加入空节点做为标记。
	
	                if (node->left) st.push(node->left);    // 添加左节点（空节点不入栈）
	            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
	                st.pop();           // 将空节点弹出
	                node = st.top();    // 重新取出栈中元素
	                st.pop();
	                result.push_back(node->val); // 加入到结果集
	            }
	        }
	        return result;
	```


- 迭代法前序遍历
	```c++
	vector<int> preorderTraversal(TreeNode* root) {
	        vector<int> result;
	        stack<TreeNode*> st;
	        if (root != NULL) st.push(root);
	        while (!st.empty()) {
	            TreeNode* node = st.top();
	            if (node != NULL) {
	                st.pop();
	                if (node->right) st.push(node->right);  // 右
	                if (node->left) st.push(node->left);    // 左
	                st.push(node);                          // 中
	                st.push(NULL);
	            } else {
	                st.pop();
	                node = st.top();
	                st.pop();
	                result.push_back(node->val);
	            }
	        }
	        return result;
	```


- 迭代法后序遍历
	```c++
	 vector<int> postorderTraversal(TreeNode* root) {
	        vector<int> result;
	        stack<TreeNode*> st;
	        if (root != NULL) st.push(root);
	        while (!st.empty()) {
	            TreeNode* node = st.top();
	            if (node != NULL) {
	                st.pop();
	                st.push(node);                          // 中
	                st.push(NULL);
	
	                if (node->right) st.push(node->right);  // 右
	                if (node->left) st.push(node->left);    // 左
	
	            } else {
	                st.pop();
	                node = st.top();
	                st.pop();
	                result.push_back(node->val);
	            }
	        }
	        return result;
	```


[验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/description/)]

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

<img src="https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg" alt="image.png" style="zoom:60%;" />

> **输入：** root = `[2,1,3]`
> **输出：** true


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240514231056.png" alt="image.png" style="zoom:40%;" />

由上图可知，在递归时，除了传入节点，还要传入**开区间得范围**，对于每个节点判断是否在开区间内，然后往下递归，如果往左边递归，就把开区间的右边界更新为节点值，往右递归，就把开区间的左边界更效为节点值，这种先访问节点，再递归左右子树的方法就是前序遍历

```c++
class Solution {
public:
    bool isValidBST(TreeNode *root, long left = LONG_MIN, long right = LONG_MAX) {
        if (root == nullptr)
            return true;
        long x = root->val;
        return left < x && x < right &&
               isValidBST(root->left, left, x) &&
               isValidBST(root->right, x, right);
    }
};
```

中序遍历二叉搜索树是一个递增的序列，我们只要每次和上一个值比较就行

```c++
class Solution {
    long pre = LONG_MIN;
public:
    bool isValidBST(TreeNode *root) {
        if (root == nullptr)
            return true;
        if (!isValidBST(root->left) || root->val <= pre)
            return false;
        pre = root->val;
        return isValidBST(root->right);
    }
};
```

我们还可以把节点的值往上面传，

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240515054616.png" alt="image.png" style="zoom:60%;" />
要先判断左右子树，再访问节点，这是后序遍历，空节点返回(∞，-∞)，因为父节点一定满足，因为这个范围不存在

```c++
class Solution {
    pair<long, long> dfs(TreeNode *node) {
        if (node == nullptr)
            return {LONG_MAX, LONG_MIN};
        auto[l_min, l_max] = dfs(node->left);
        auto[r_min, r_max] = dfs(node->right);
        long x = node->val;
        // 也可以在递归完左子树之后立刻判断，如果发现不是二叉搜索树，就不用递归右子树了
        if (x <= l_max || x >= r_min)
            return {LONG_MIN, LONG_MAX}; // 这个范围一旦返回后面的节点都会失败
        return {min(l_min, x), max(r_max, x)};
    }

public:
    bool isValidBST(TreeNode *root) {
        return dfs(root).second != LONG_MAX;
    }
};
```

### 二叉树层序遍历


[二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。
<img src="https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg" alt="image.png" style="zoom:60%;" />

> 输入：root = `[3,9,20,null,null,15,7]`
> 输出：`[[3],[9,20],[15,7]]`


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240516223136.png" alt="image.png" style="zoom:40%;" />
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240516223926.png" alt="image.png" style="zoom:40%;" />

方法一：两个数组

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode *root) {
        if (root == nullptr) return {};
        vector<vector<int>> ans;
        vector<TreeNode*> cur = {root};
        while (cur.size()) {
            vector<TreeNode*> nxt;
            vector<int> vals;
            for (auto node : cur) {
                vals.push_back(node->val);
                if (node->left)  nxt.push_back(node->left);
                if (node->right) nxt.push_back(node->right);
            }
            cur = move(nxt);
            ans.emplace_back(vals);
        }
        return ans;
    }
};
```

一个队列

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode *root) {
        
        vector<vector<int>> ans;
        queue<TreeNode *> q;
        if(root)
	        q.push(root);
        while (!q.empty()) {
            vector<int> vals;
            for (int n = q.size(); n--;) {
                auto node = q.front();
                q.pop();
                vals.push_back(node->val);
                if (node->left)  q.push(node->left);
                if (node->right) q.push(node->right);
            }
            ans.emplace_back(vals);
        }
        return ans;
    }
};
```

递归写法

```c++
void order(TreeNode* cur, vector<vector<int>>& result, int depth)
    {
        if (cur == nullptr) return;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
```

求深度适合用前序遍历，而求高度适合用后序遍历。
### 树的遍历转换

1. 中序后序得前序
```c++
void pre(int root,int start,int end,int index)
{
    if(start>end) return;
    int i=start;
    while(i<end&&post[root]!=in[i]) i++;
    cout<<post[root];//level[index]=post[root];
    pre(root-1-end+i,start,i-1,2*index);
    pre(root-1,i+1,end,2*index+1);
}
```

2. 中序后序建树

```c++
unordered_map<int, int> inMap; // 存储中序遍历中每个值的索引

TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
	// 填充inMap，便于快速查找中序遍历中任意值的索引
	for (int i = 0; i < inorder.size(); i++) {
		inMap[inorder[i]] = i;
	}
	// 调用build函数开始构建二叉树
	return build(postorder, 0, postorder.size() - 1, 0, inorder.size() - 1);
}

TreeNode* build(vector<int>& postorder, int postStart, int postEnd, int inStart, int inEnd) {
	if (postStart > postEnd || inStart > inEnd) return nullptr; // 基本情况

	TreeNode* root = new TreeNode(postorder[postEnd]); // 后序遍历的最后一个元素是根节点
	int inRoot = inMap[root->val]; // 在中序遍历中找到根节点的位置
	int numsLeft = inRoot - inStart; // 左子树中的节点数量

	// 递归构建左子树和右子树
	root->left = build(postorder, postStart, postStart + numsLeft - 1, inStart, inRoot - 1);
	root->right = build(postorder, postStart + numsLeft, postEnd - 1, inRoot + 1, inEnd);

	return root;
}
```

3. 中序前序得后序

```c++
void post(int root,int start,int end，int index)
{  
    if(start>end) return;
    int i=start;
    while(i<end&&pre[root]!=in[i]) i++;
    post(root+1,start,i-1,2*index);
    post(root+1+i-start,i+1,end,2*index+1)
    cout<<pre[root];//level[index]=pre[root];
}
```

4. 中序前序建树
```c++
    TreeNode* build(vector<int>& preorder, int preStart, int preEnd,
                    int inStart, int inEnd) {
        if (preStart > preEnd || inStart > inEnd)
            return nullptr;
        TreeNode* root = new TreeNode(preorder[preStart]);
        int inRoot = inMap[root->val];
        int numsLeft = inRoot - inStart;
        root->left = build(preorder, preStart + 1, preStart + numsLeft,
                           inStart, inRoot - 1);
        root->right = build(preorder, preStart + numsLeft + 1, preEnd,
                            inRoot + 1, inEnd);
        return root;
    }
```


### 公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
[百度百科](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin "https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin")中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

### <img src="https://assets.leetcode.com/uploads/2018/12/14/binarytree.png" alt="image.png" style="zoom:80%;" />
> 输入：root = `[3,5,1,6,2,0,8,null,null,7,4]`, p = 5, q = 1
> 输出：3
> 解释：节点 5 和节点 1 的最近公共祖先是节点 3 。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240516140928.png" alt="image.png" style="zoom:60%;" />

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // 如果根节点为空，或者根节点等于p或q中的任何一个，直接返回根节点
        if (!root || root == p || root == q) return root;
        
        // 在左子树中查找p和q的最近公共祖先
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        // 在右子树中查找p和q的最近公共祖先
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        
        // 如果左子树查找结果不为空，且右子树查找结果也不为空，说明p和q分别在根节点的两侧，当前根节点即为最近公共祖先
        if (left && right) return root;
        
        // 如果左子树为空，那么p和q都不在左子树中，返回右子树的查找结果
        // 如果右子树为空，那么p和q都不在右子树中，返回左子树的查找结果
        // 如果左右子树查找结果都为空，那么p和q都不在这个树中，返回NULL
        return left ? left : right;
    }
};
```


### 二叉搜索树

#### 二叉搜索树的验证或者求值

进行中序遍历时，遍历得到的节点值序列应该是严格递增的。

```c++
unordered_map、int、TreeNode* pre;// 定义一些全局变量记录当前节点之前的状态
auto isValidBST(TreeNode* root) {
	if (root == NULL) return ...;
	auto left = isValidBST(root->left);
	// 因为BST的中序遍历是有序，全局变量和root是在有序数组中是相邻关系，这里可以做逻辑处理 
	// ....
	auto right = isValidBST(root->right);

	return ...;
}
```


#### 搜索树的删除

- 如果目标节点大于当前节点值，则去右子树中删除；
- 如果目标节点小于当前节点值，则去左子树中删除；
- 如果目标节点就是当前节点，分为以下三种情况：
	- 其无左子：其右子顶替其位置，删除了该节点；
	- 其无右子：其左子顶替其位置，删除了该节点；
	- 其左右子节点都有：其**左子树转移到其右子树的最左节点的左子树上**，然后右子树顶替其位置，由此删除了该节点。

第三种情况图示如下

<img src="https://pic.leetcode-cn.com/1611932922-MelojG-450.jpg" alt="image.png" style="zoom:20%;" />

```c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) 
    {
        if (root == nullptr)    return nullptr;
        if (key < root->val)    root->left = deleteNode(root->left, key);  // 去左子树删除
        else if (key > root->val)    root->right = deleteNode(root->right, key);     // 去右子树删除
        else    // 当前节点就是要删除的节点
        {
            if (! root->left)   return root->right; // 情况1，欲删除节点无左子
            if (! root->right)  return root->left;  // 情况2，欲删除节点无右子
            TreeNode* node = root->right;           // 情况3，欲删除节点左右子都有 
            while (node->left)          // 寻找欲删除节点右子树的最左节点
                node = node->left;
            node->left = root->left;    // 将欲删除节点的左子树成为其右子树的最左节点的左子树
            root = root->right;         // 欲删除节点的右子顶替其位置，节点被删除
        }
        return root;    
    }
};
```


### 线索二叉树

线索二叉树（Threaded Binary Tree）是一种特殊的二叉树结构，它的主要特点是利用二叉树中的空指针来存储额外的信息，从而加快树的遍历速度。让我详细解释一下：

1. 基本概念：  
    在普通的二叉树中，叶子节点的左右子树指针（对于没有左或右子节点的内部节点也是如此）通常为空（null）。线索二叉树利用这些空指针来存储有用的信息。
2. 线索化过程：
    - 对于没有左子节点的节点，其左指针指向中序遍历序列的前驱节点。
    - 对于没有右子节点的节点，其右指针指向中序遍历序列的后继节点。
3. 线索的类型：
    - 左线索：指向前驱的指针。
    - 右线索：指向后继的指针。
4. 标记：  
    通常需要额外的标记来区分一个指针是普通的子节点指针还是线索。


<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7a/Threaded_tree.svg/600px-Threaded_tree.svg.png" alt="image.png" style="zoom:60%;" />

```c++
struct ThreadedNode {
    int val;
    ThreadedNode* left;
    ThreadedNode* right;
    bool leftThread;   // 标记左指针是否为线索
    bool rightThread;  // 标记右指针是否为线索

    ThreadedNode(int x) : val(x), left(nullptr), right(nullptr), leftThread(false), rightThread(false) {}
};
```

```c++
class ThreadBinaryTree{
private:
	ThreadedNode* root; 
	ThreadedNode* prev; // 用于线索化过程中记录前一个节点
public:
	void inorderThread(ThreadedNode* node){
	if(!node) return;
	// 线索化左子树 
	inorderThreading(node->left);

	// 处理当前节点
	if (node->left == nullptr) 
	{ 
		node->left = prev; 
		node->leftThread = true; 
	}

	if(node->right==nullptr&&prev->right == nullptr){
		prev->right=ndoe;
		prev->rightThread = true;
	}
	prev=node;

	// 线索化右子树 
	inorderThreading(node->right);
	}
}

public:
    ThreadedBinaryTree() : root(nullptr), prev(nullptr) {}

    // 将普通二叉树转换为线索二叉树
    void createThreadedBinaryTree(ThreadedNode* root) {
        this->root = root;
        prev = nullptr;
        inorderThreading(root);
        
        // 处理最后一个节点
        if (prev != nullptr && prev->right == nullptr) {
            prev->rightThread = true;
        }
    }

    // 中序遍历线索二叉树
    void inorderTraversal() {
        ThreadedNode* current = leftmost(root);
        while (current != nullptr) {
            std::cout << current->val << " ";
            current = inorderSuccessor(current);
        }
        std::cout << std::endl;
    }
```



## 其他树

### 线段树

线段树可以在O(logn)的时间复杂度内实现**单点修改、区间修改、区间查询**（**区间求和，求区间最大值，求区间最小值**）等操作。

线段树将每个长度不为1的区间划分成左右两个区间递归求解，把整个线段划分为一个树形结构，通过合并左右两区间信息来求得该区间的信息。

有个大小为5的数组`a={10,11,12,13,14}`,要将其转化为线段树,有以下做法:设线段树的根节 点编号为1,用数组d来保存我们的线段树,d用来保存线段树_上编号为i的节点的值(这里每个 节点所维护的值就是这个节点所表示的区间总和)。

我们先给出这棵线段树的形态，如图所示：

<img src="https://oi-wiki.org/ds/images/segt1.svg" alt="image.png" style="zoom:60%;" />

图中每个节点中用红色字体标明的区间，表示该节点管辖的 a 数组上的位置区间。如 d₁ 所管辖的区间就是 `[1,5]` (a₁,a₂,⋯,a₅)，即 d₁ 所保存的值是 a₁+a₂+⋯+a₅，d₁ = 60 表示的是 a₁+a₂+⋯+a₅ = 60。

通过观察不难发现，dᵢ 的左儿子节点就是 d₂ₓᵢ，dᵢ 的右儿子节点就是 d₂ₓᵢ₊₁。如果 dᵢ 表示的是区间 `[s,t]`（即 dᵢ = aₛ+aₛ₊₁+⋯+aₜ）的话，那么 dᵢ 的左儿子节点表示的是区间 `[s,⌊(s+t)/2⌋]`，dᵢ 的右儿子表示的是区间 `[⌊(s+t)/2⌋+1,t]`。

在实现时，我们考虑递归建树，设当前的根节点为 p，如果根节点管辖的区间长度已经是 1，则可以直接根据 a 数组上相应位置的值初始化该节点。否则我们将该区间从中点处分割为两个子区间，分别进入左右子节点递归建树，最后合并两个子节点的信息。

时间复杂度
- 构建：O(n log n)
- 查询：O(log n + k)，其中k是结果集的大小
- 更新：O(log n)



### 前缀树

`前缀树` 是 `N 叉树` 的一种特殊形式。通常来说，一个前缀树是用来 `存储字符串` 的。前缀树的每一个节点代表一个 `字符串`（`前缀`）。每一个节点会有多个子节点，通往不同子节点的路径上有着不同的字符。子节点代表的字符串是由节点本身的 `原始字符串`，以及 `通往该子节点路径上所有的字符` 组成的。

下面是前缀树的一个例子：

<img src="https://tsejx.github.io/data-structure-and-algorithms-guidebook/static/sample.c908915f.png" alt="image.png" style="zoom:60%;" />

在上图示例中，我们在节点中标记的值是该节点对应表示的字符串。例如，我们从根节点开始，选择第二条路径 `'b'`，然后选择它的第一个子节点 `'a'`，接下来继续选择子节点 `'d'`，我们最终会到达叶节点 `"bad"`。节点的值是由从根节点开始，与其经过的路径中的字符按顺序形成的。

值得注意的是，根节点表示 `空字符串`。
前缀树的一个重要的特性是，节点所有的后代都与该节点相关的字符串有着共同的前缀。这就是 `前缀树` 名称的由来。

我们再来看这个例子。例如，以节点 "b" 为根的子树中的节点表示的字符串，都具有共同的前缀 "b"。反之亦然，具有公共前缀 "b" 的字符串，全部位于以 "b" 为根的子树中，并且具有不同前缀的字符串来自不同的分支。

前缀树有着广泛的应用，例如自动补全，拼写检查等等。
Trie Tree（字典树/前缀树/单词查找树/键树），是一种树形结构，是一种哈希树的变种。
典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。
它的优点是：最大限度地減少无谓的字符串比较，查询效率比哈希表高。

**基本性质**
1. 根节点不包含字符，除根节点外每一个节点都只包含一个字符（便于插入和查找）
2. 从根节点到某一节点，路径是经过的字符连接起来，为该节点对应的字符串
3. 每个节点的所有自节点包含的字符都不相同。

**实现字典树**

```c++
class Trie {
public:
    Trie() {
        this->children = vector<Trie *>(26, nullptr); // 每个节点可能有26个后继
        this->isEnd = false; //若结点是单词的结尾，值为 true，否则为 false
    }

    bool insert(const string & word) {
        Trie * node = this;
        for (const auto & ch : word) {
            int index = ch - 'a';
            if (node->children[index] == nullptr) {
                node->children[index] = new Trie();
            }
            node = node->children[index];
        }
        node->isEnd = true;
        return true;
    }

    bool search(const string & word) {
        Trie * node = this;
        for (const auto & ch : word) {
            int index = ch - 'a';
            if (node->children[index] == nullptr) {
                return false;
            }
            node = node->children[index];
        }
        return node != nullptr;
    }
private:
    vector<Trie *> children;
    bool isEnd;
};

```


### 红黑树

红黑树是一种自平衡的二叉搜索树。每个节点额外存储了一个 color 字段 ("RED" or "BLACK")，用于确保树在插入和删除时保持平衡。

**性质**:

一棵合法的红黑树必须遵循以下四条性质：
1. 节点为红色或黑色
2. NIL 节点（空叶子节点）为黑色
3. 红色节点的子节点为黑色
4. 从根节点到 NIL 节点的每条路径上的黑色节点数量相同

下图为一棵合法的红黑树：

<img src="https://oi-wiki.org/ds/images/rbtree-example.svg" alt="image.png" style="zoom:80%;" />
插入操作：
- 插入结点是根结点 -> 直接变黑
- 插入结点的叔叔是红色 -> 叔父爷变色,爷爷变插入结点
- 插入结点的叔叔是黑色 -> (LL,RR,LR,RL)旋转,然后变色

删除操作：
- 只有左孩子/只有右孩子：代替后变黑
- 没有孩子
	- 红节点：删除后无需调整

### 跳表

skiplist，顾名思义，首先它是一个list。实际上，它是在有序链表的基础上发展起来的。
我们先来看一个有序链表，如下图（最左侧的灰色节点表示一个空的头结点）：

<img src="http://zhangtielei.com/assets/photos_redis/skiplist/sorted_linked_list.png" alt="image.png" style="zoom:60%;" />
在这样一个链表中，如果我们要查找某个数据，那么需要从头开始逐个进行比较，直到找到包含数据的那个节点，或者找到第一个比给定数据大的节点为止（没找到）。也就是说，时间复杂度为O(n)。同样，当我们要插入新数据的时候，也要经历同样的查找过程，从而确定插入位置。

假如我们每相邻两个节点增加一个指针，让指针指向下下个节点，如下图：

<img src="http://zhangtielei.com/assets/photos_redis/skiplist/skip2node_linked_list.png" alt="image.png" style="zoom:60%;" />
这样所有新增加的指针连成了一个新的链表，但它包含的节点个数只有原来的一半（上图中是7, 19, 26）。现在当我们想查找数据的时候，可以先沿着这个新链表进行查找。当碰到比待查数据大的节点时，再回到原来的链表中进行查找。比如，我们想查找23，查找的路径是沿着下图中标红的指针所指向的方向进行的：

<img src="http://zhangtielei.com/assets/photos_redis/skiplist/search_path_on_skip2node_list.png" alt="image.png" style="zoom:60%;" />

利用同样的方式，我们可以在上层新产生的链表上，继续为每相邻的两个节点增加一个指针，从而产生第三层链表。如下图：

<img src="http://zhangtielei.com/assets/photos_redis/skiplist/skip2node_level3_linked_list.png" alt="image.png" style="zoom:60%;" />

skiplist正是受这种多层链表的想法的启发而设计出来的。实际上，按照上面生成链表的方式，上面每一层链表的节点个数，是下面一层的节点个数的一半，这样查找过程就非常类似于一个二分查找，使得查找的时间复杂度可以降低到O(log n)。但是，这种方法在插入数据的时候有很大的问题。新插入一个节点之后，就会打乱上下相邻两层链表上节点个数严格的2:1的对应关系。如果要维持这种对应关系，就必须把新插入的节点后面的所有节点（也包括新插入的节点）重新进行调整，这会让时间复杂度重新蜕化成O(n)。删除数据也有同样的问题。

skiplist为了避免这一问题，它不要求上下相邻两层链表之间的节点个数有严格的对应关系，而是为每个节点随机出一个层数(level)。比如，一个节点随机出的层数是3，那么就把它链入到第1层到第3层这三层链表中。为了表达清楚，下图展示了如何通过一步步的插入操作从而形成一个skiplist的过程：

<img src="http://zhangtielei.com/assets/photos_redis/skiplist/skiplist_insertions.png" alt="image.png" style="zoom:60%;" />

在分析之前，我们还需要着重指出的是，执行插入操作时计算随机数的过程，是一个很关键的过程，它对skiplist的统计特性有着很重要的影响。这并不是一个普通的服从均匀分布的随机数，它的计算过程如下：

- 首先，每个节点肯定都有第1层指针（每个节点都在第1层链表里）。
- 如果一个节点有第i层(i>=1)指针（即节点已经在第1层到第i层链表中），那么它有第(i+1)层指针的概率为p。
- 节点最大的层数不允许超过一个最大值，记为MaxLevel。

```c++
randomLevel()
    level := 1
    // random()返回一个[0...1)的随机数
    while random() < p and level < MaxLevel do
        level := level + 1
    return level
```

randomLevel()的伪码中包含两个参数，一个是p，一个是MaxLevel。在Redis的skiplist实现中，这两个参数的取值为：

```c++
p = 1/4
MaxLevel = 32
```

### 哈夫曼树

霍夫曼算法用于构造一棵霍夫曼树,算法步骤如下:
1. 初始化:由给定的n个权值构造n棵只有一个根节点的二叉树,得到一个二叉树集合F。选取与合并 
2. 选取与合并:从二叉树集合F中选取根节点权值最小的两棵二叉树分别作为左右子树构造一棵新的二叉树,这棵新二叉树的根节点的权值为其左、右子树根结点的权值和。
3. 删除与加入:从F中删除作为左、右子树的两棵二叉树,并将新建立的二叉树加入到F中。
4. 重复2、3步,当集合中只剩下一棵二叉树时,这棵二叉树树就是霍夫曼树。

<img src="https://oi-wiki.org/ds/images/huffman-tree-2.svg" alt="image.png" style="zoom:60%;" />

```c++
// 代表小顶堆的优先队列
priority_queue<long long,vector<long long>,greater<long long>>q;
int main(){

	for(int i=0;i<n;i++){
		cin>>temp;
		q.push(temp);
	}
	while(q.size()>1){
		x=q.top();
		q.pop();
		y=q.top();
		q.pop();
		q.push(x+y); //取出堆顶的两个元素,求和后压入优先队列
		ans+=x+y;
	}
	cout<<ans<<endl;
	return 0;

}
```
## 回溯

### 回溯算法理论基础


> [!question] 什么是回溯法?
> - 回溯是递归的副产品，只要有递归就会有回溯，**回溯有一个增量构造答案的过程**，这个过程用递归实现。
> - 回溯的本质是穷举，穷举所有可能，然后选出我们想要的答案
> - 如果想让回溯法高效一些，可以加一些**剪枝**的操作
> - 回溯法解决的都是在集合中递归查找子集，**集合的大小就构成了树的宽度，递归的深度，都构成的树的深度**。


回溯法，一般可以解决如下几种问题：
- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等
   
回溯搜索的遍历过程

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210130173631174.png" alt="image.png" style="zoom:50%;" />

```c++
/* 回溯算法框架 */
void backtrack(State *state, vector<Choice *> &choices, vector<State *> &res) {
    // 判断是否为解
    if (isSolution(state)) {
        // 记录解
        recordSolution(state, res);
        // 不再继续搜索
        return;
    }
    // 遍历所有选择
    for (Choice choice : choices) {
        // 剪枝：判断选择是否合法
        if (isValid(state, choice)) {
            // 尝试：做出选择，更新状态
            makeChoice(state, choice);
            backtrack(state, choices, res);
            // 回退：撤销选择，恢复到之前的状态
            undoChoice(state, choice);
        }
    }
}
```

### 子集型

以[电话号码字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)为例

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。


<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png" alt="image.png" style="zoom:80%;" />
示例 1：
```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```


回溯三问：
- 当前操作? 枚举`path[i]`要填入的字母
- 子问题? 构造字符串>=i的部分
- 下一个子问题? 构造字符串>=i+1的部分

$$dfs(i) --> dfs(i+1)$$

```c++
vector<string> letterCombinations(string digits) {
        string path;
        vector<string> result;
        int n = digits.size();
        vector<string> mapping = {"",    "",    "abc",  "def", "ghi",
                                  "jkl", "mno", "pqrs", "tuv", "wxyz"};
        function<void(int)> dfs = [&](int i) {
            // 边界条件 构造长为0的字符串
            if (i == n) {
                if (path != "")
                    result.push_back(path);
                return;
            }
            
            string str = mapping[digits[i] - '0'];
            for (auto& it : str) {
                // 当前操作
                path += it;
                // 下一个子问题
                dfs(i + 1);
                path.pop_back();
            }
        };
        dfs(0);
        return result;
    }
```


i代表大于等于i的这部分，不是第i个

**子集型回溯** 
每个元素都可以**选/不选**

[子集](https://leetcode.cn/problems/subsets/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。
解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

示例 1：
```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

输入的视角（选或不选）
```c++
        vector<int>ans;
        vector<vector<int>>result;
        int n=nums.size();
        function<void(int)>dfs=[&](int i){
            // 边界条件
            if(xxx){
                result.push_back(ans);
                return;
            }
			// 当前操作可以怎么做 不确定的用for循环
			
            // 选
            ans.push_back(nums[i]);
            dfs(i+1);
            ans.pop_back();

            // 不选
            dfs(i+1);
        };
        dfs(0);
```

方法二：答案的视角（选哪个数）
```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int> &nums) {
        vector<vector<int>> ans;
        vector<int> path;
        int n = nums.size();
        function<void(int)> dfs = [&](int i) {
            ans.emplace_back(path);
            if (i == n) return;
            for (int j = i; j < n; ++j) { // 枚举选择的数字
                path.push_back(nums[j]);
                dfs(j + 1);
                path.pop_back(); // 恢复现场
            }
        };
        dfs(0);
        return ans;
    }
};
```


### 组合型

[组合](https://leetcode.cn/problems/combinations/)

给定两个整数 n 和 k，返回范围 `[1, n] `中所有可能的 k 个数的组合。
你可以按 任何顺序 返回答案。

```
示例 1：

输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240517064619.png" alt="image.png" style="zoom:40%;" />
- 设path长为m
- 那么还需要选d=k-m个数
- 设当前需要从`[1,i]`这i个数中选数
- 如果`i<d`
- 最后必然无法选出k个数
- 不需要继续递归
- 这是一种剪枝技巧

方法一：枚举下一个数选哪个

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> path;
        function<void(int)> dfs = [&](int i) {
            int d = k - path.size(); // 还要选 d 个数
            if (d == 0) {
                ans.emplace_back(path);
                return;
            }
            for (int j = i; j >= d; --j) {
                path.push_back(j);
                dfs(j - 1);
                path.pop_back();
            }
        };
        dfs(n);
        return ans;
    }
};
```


方法二：选或不选
```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> path;
        function<void(int)> dfs = [&](int i) {
            int d = k - path.size(); // 还要选 d 个数
            if (d == 0) {
                ans.emplace_back(path);
                return;
            }
            // 不选 i
            if (i > d) dfs(i - 1);
            // 选 i
            path.push_back(i);
            dfs(i - 1);
            path.pop_back();
        };
        dfs(n);
        return ans;
    }
};
```



### 排列型

- 当前操作? 从s中枚举`path[i]`要填入的数字x
- 子问题? 构造排列`>=i`的部分，剩余未选数字集合为s
- 下一个子问题? 构造排列`>=i+1`的部分，
- 剩余未选数字集合为`s-{x}`

dfs(i,s)=dfs(i+1,s-{x_1}),dfs(i+1,s-{x_2}), ...

> 在c++中集合s可以用一个数组on_path来代替,通过设置是否为falsel来代表是否存在

```c++

vector<vector<int>> permute(vector<int> &nums) {
	int n = nums.size();
	vector<vector<int>> ans;
	vector<int> path(n), on_path(n);
	function<void(int)> dfs = [&](int i) {
		if (i == n) {
			ans.emplace_back(path);
			return;
		}
		for (int j = 0; j < n; ++j) {
			if (!on_path[j]) {
				path[i] = nums[j];
				on_path[j] = true;
				dfs(i + 1);
				on_path[j] = false; // 恢复现场
			}
		}
	};
	dfs(0);
	return ans;
}
```

去重操作是这个

```c++
	for(int i=index;i<n&&sum+candidates[i]<=target;++i){
		if(i>index&&candidates[i]==candidates[i-1]) continue; // 去重关键点

		path.push_back(candidates[i]);
		dfs(i+1,sum+candidates[i]);
		path.pop_back();
	}
```

```c++
for (int i = 0; i < nums.size(); i++) {
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used); // 将used带入递归
                path.pop_back();
                used[i] = false;
            }
        }
```

### N皇后

不同行,不同列-->每行每列恰好有一个皇后
证明:反证法+鸽巢原理


> [!multi-column]
>
>> [!note]+ 思路
>>
>>
>>- 用一个长为$n$的数组$col$记录皇后的位置
>>- 即第$i$行的皇后在第$col[i]$列
>>- 那么$col$必须是一个0到n-1的排列 
>>- 示例1右图$col= [1,3,0,2]$，右图$col= [2,0,3,1]$
>
>> [!warning]+ 进行中的事项
>>
>>- <img src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" alt="image.png" style="zoom:50%;" />


对角线的判断逻辑是对于棋盘上的任意两个位置$(i, queens[i])$和$(row, col)$,即$|i - row| = |queens[i] - col|$，那么这两个位置处的皇后就位于同一斜线上


```c++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> solutions;
        vector<int> queens(n, -1); // 存储每行皇后的列位置
        solve(solutions, queens, n, 0);
        return solutions;
    }

    void solve(vector<vector<string>>& solutions, vector<int>& queens, int n, int row) {
        if (row == n) {
            // 所有行都成功放置了皇后，构建棋盘并添加到解集
            vector<string> board(n, string(n, '.'));
            for (int i = 0; i < n; ++i) {
                board[i][queens[i]] = 'Q';
            }
            solutions.push_back(board);
            return;
        }

        for (int col = 0; col < n; ++col) {
            if (isValid(queens, row, col)) {
                queens[row] = col; // 在当前行放置皇后
                solve(solutions, queens, n, row + 1); // 移动到下一行
                // 回溯不需要显式撤销选择，因为下一次循环会覆盖当前行的值
            }
        }
    }

    bool isValid(vector<int>& queens, int row, int col) {
        for (int i = 0; i < row; ++i) {
            if (queens[i] == col || // 同列
                abs(queens[i] - col) == abs(i - row)) { // 同对角线
                return false;
            }
        }
        return true;
    }
};
```

对于只有一种答案的回溯，dfs函数的返回值写成bool 在边界条件上返回true,在函数最后返回false
## 贪心

贪心的理论基础
- **贪心的本质是选择每一阶段的局部最优，从而达到全局最优**。
- 贪心算法不会考虑过去的决策，而是一路向前地进行贪心选择，不断缩小问题范围，直至问题被解决。

贪心算法一般分为如下四步：
- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

### 区间贪心

区间贪心问题通常是指在给定一组区间中选择尽可能多的互不重叠的区间。一个典型的例子是活动选择问题，其中每个活动都有一个开始和结束时间，目标是尽可能多地选择活动，使得这些活动之间没有时间冲突。

```c++
#include <vector>
#include <algorithm>

// 定义区间结构
struct Interval {
    int start;
    int end;
    Interval(int s, int e) : start(s), end(e) {}
};

// 区间贪心算法
class IntervalGreedy {
public:
    // 区间选择问题：选择最多的不重叠区间
    int maxNonOverlappingIntervals(std::vector<Interval>& intervals) {
        if (intervals.empty()) return 0;

        // 按结束时间排序
        std::sort(intervals.begin(), intervals.end(), 
                  [](const Interval& a, const Interval& b) {
                      return a.end < b.end;
                  });

        int count = 1;  // 至少有一个区间
        int end = intervals[0].end;

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i].start >= end) {
                count++;
                end = intervals[i].end;
            }
        }

        return count;
    }

    // 区间覆盖问题：用最少的区间覆盖一条线段
    int minIntervalsToCoverRange(std::vector<Interval>& intervals, int rangeStart, int rangeEnd) {
        // 按开始时间排序
        std::sort(intervals.begin(), intervals.end(), 
                  [](const Interval& a, const Interval& b) {
                      return a.start < b.start;
                  });

        int count = 0;
        int i = 0;
        int currentEnd = rangeStart;

        while (currentEnd < rangeEnd && i < intervals.size()) {
            int maxEnd = currentEnd;
            while (i < intervals.size() && intervals[i].start <= currentEnd) {
                maxEnd = std::max(maxEnd, intervals[i].end);
                i++;
            }

            if (maxEnd == currentEnd) return -1;  // 无法完全覆盖

            count++;
            currentEnd = maxEnd;
        }

        return currentEnd >= rangeEnd ? count : -1;
    }
};

// 使用示例
int main() {
    std::vector<Interval> intervals = {{1,3}, {2,4}, {3,5}, {6,7}};
    IntervalGreedy solver;

    // 最多不重叠区间数
    int maxNonOverlap = solver.maxNonOverlappingIntervals(intervals);
    
    // 最少区间覆盖 [1,7]
    int minCover = solver.minIntervalsToCoverRange(intervals, 1, 7);

    return 0;
}
```


## 分治

## 动态规划

### 动态规划理论知识

动态规划（dynamic programming）是一个重要的算法范式，它**将一个问题分解为一系列更小的子问题**，并通过存储子问题的解来避免重复计算，从而大幅提升时间效率。

动态规划的核心就是**状态定义?状态转移方程?** 


> [!summary] DP萌新三步
> - 思考回溯要怎么写
> - 改成记忆化搜索
> 	- 入参和返回值
> 	- 递归到哪里
> 	- 递归边界和入口
> - 1:1翻译成递推

以力扣[198题](https://leetcode.cn/problems/house-robber/description/)为例

**示例 1：**

> **输入：** `[1,2,3,1]`
> **输出：** 4
> **解释：** 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
>      偷窃到的最高金额 = 1 + 3 = 4 。

> [!summary] 回溯三问
> - 当前操作? 枚举**第**$i$个房子选/不选
> - 子问题? 从**前**$i$个房子中得到的最大金额和
> - 下一个子问题? 分类讨论:
> 	- 不选: 从前$i-1$个房子中得到的最大金额和
> 	- 选: 从前$i-2$个房子中得到的最大金额和


$$dfs(i)=max(dfs(i-1),dfs(i-2)+nums[i])$$

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240406121521.png" alt="image.png" style="zoom:50%;" />


这里⚪代表第i个子问题，可以看到dfs(2)计算了两次，是可以优化的，

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240406122021.png" alt="image.png" style="zoom:60%;" />

把递归的计算结果**保存**下来那么下次递归到同样的入参时就直接返回先前保存的结果

$$递归搜索+保存计算结果=记忆化搜索$$ 

优化后只有O(n)个节点

```c++
        vector<int>d(n,-1);
         
        function<int(int)> dfs = [&](int start) {

            if (start < 0) {
                return 0;
            }
            if(d[start]!=-1){
                return d[start];
            }
            // 不选
            int res= max(dfs(start - 1), dfs(start - 2) + nums[start]);
            d[start]=res;
            return res; 
        };

        return dfs(n - 1); 
```

空间复杂度是O(n),在归的过程中发生了实际的计算，我们从哪些点归到那个点，干脆去掉递归中的递，只留下归的过程


**自顶向下算=记忆化搜索**
**自底向上算=递推**


> [!summary] 翻译成递推
> - dfs → f数组
> - 递归 → 循环
> - 递归边界 → 数组初始值

递归和循环都是对每个i

$$dfs(i)=max(dfs(i-1),dfs(i-2)+nums[i])$$

$$f[i]=max(f[i-1]+f[i-2]+nums[i])$$

$$f[i+2]=max(f[i+1],f[i]+nums[i])$$

这个空间复杂度仍然是O(n)的

当前=$max(上一个，上上一个+nums[i]）$
$f_0$表示上上一个，$f_1$表示上一个
$newF=max(f_1,f_0+nums[i])$
$f_0=f_1$
$f_1= newF$


基础的递推除了求最大值还有求方案数
典型就是爬楼梯的各种变体

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

**输入：** n = 2
**输出：** 2
**解释：** 有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶

分类讨论:
- 如果最后一步爬了1个台阶，那么我们得先爬到i-1，要解决的问题缩小成:从0爬到i-1有多少种不同的方法。
- 如果最后一步爬了2个台阶，那么我们得先爬到i-2，要解决的问题缩小成:从0爬到i-2有多少种不同的方法。
这两种方法是互相独立的，所以根据加法原理，从0爬到i的方法数等于这两种方法数之和，即

$$dfs(i)=dfs(i−1)+dfs(i−2)$$

```c++
#if 0 // 递推
    vector<int>f(n+2,0);
    for(int i=0;i<n;++i){
        f[i+2]=max(f[i+1],f[i]+nums[i]);
    }
    return f[n+1];
#endif
```

空间优化

```c++
#if 1 //
    int f0=0,f1=0;
    int newF;
    for(int i=0;i<n;++i){
        newF=max(f1,f0+nums[i]);
        f0=f1;
        f1=newF;
    }
    return newF;
#endif
```

### 背包问题

#### 01背包

**0-1背包**:有几个物品，第i个物品的体积为$w[i]$，价值为$v[i]$每个物品至多选一个，求体积和不超过$capacity$时的最大价值和


> [!summary] 回溯三问
> - 当前操作? 枚举第$i$个物品选或不选:
> 	- 不选,剩余容量不变;选，剩余容量减少$w[i]$
> 
> - 子问题? 在剩余容量为$c$时，
> 	- 从前$i$个物品中得到的最大价值和
> 
> - 下一个子问题? 分类讨论:
> 	- 不选:在剩余容量为$c$时，
> 		- 从前$i-1$个物品中得到的最大价值和
> 	- 选:在剩余容量为$c-w[i]$时，
> 		- 从前$i-1$个物品中得到的最大价值和

$$dfs(i,c)=max(dfs(i-1,c),dfs(i-1,c-w[i])+v[i])$$


> [!summary] 常见变形
> - 至多装capacity,求方案数/最大价值和
> - 恰好装capacity,求方案数/最大/最小价值和
> - 至少装capacity,求方案数/最小价值和


来看力扣的[目标和](https://leetcode.cn/problems/target-sum/description/)

$$dfs(i,c) = dfs(i-1,c) + dfs(i-1,c- w[i])$$

$$f[i][c]=f[i-1][c]+f[i-1][c-w[i]$$

$$f[i+1][c] =f[i][c]+f[i][c-w[i]]$$
##### 记忆化搜索

```c++
#if 0 // 记忆化搜索
        int n = nums.size(),cache[n][target+1];
        memset(cache,-1,sizeof(cache));
        function<int(int, int)> dfs = [&](int i, int c) {
            if (i < 0)
                return c==0?1:0;
            int &res=cache[i][c];
            if (res != -1)
                return res;

            if(c<nums[i]) return res=dfs(i-1,c);

            return res=dfs(i-1,c)+dfs(i-1,c-nums[i]);

        };
        return dfs(n - 1, target);
#endif
```


##### 递推

```c++
#if 1 //递推
        int f[n+1][target+1];
        memset(f,0,sizeof(f));
        f[0][0]=1; // 这个初始值来自于边界条件 i==0,c==0为1
        for(int i=0;i<n;++i){
            for(int c=0;c<=target;++c){
                if(c<nums[i]) f[i+1][c]=f[i][c];
                else f[i+1][c]=f[i][c]+f[i][c-nums[i]];
            }
        }
        return f[n][target];
#endif
```

递推的初始值就是递推的边界
##### 空间优化：两个数组（滚动数组）

计算当前状态只需要用到前一个状态的信息，那么就不需要保留所有历史状态的信息，从而可以将空间复杂度从`O(n*c)`降低到`O(c)`（假设`n`是物品数量，`c`是容量限制）。具体做法是使用**滚动数组**，也就是两个一维数组交替使用，每次只存储当前状态和前一个状态的信息。**就是我们每次把$f[i+1]$算完以后，后面不会用你到$f[i]$了，也就是每时每刻只有两个数组中的元素在参与状态转移**

滚动数组这样写

`f[(i+1)%2][c]=f[i%2][c] +f[i%2][c-w[i]]`

`i%2`会将`i`的值限制在`0`和`1`之间，实现对两个状态的交替使用，从而节省空间。


```c++
#if 0 //空间优化
        int f[2][target+1];
        memset(f,0,sizeof(f));
        f[0][0]=1; // 这个初始值来自于边界条件 i==0,c==0为1
        for(int i=0;i<n;++i){
            for(int c=0;c<=target;++c){
                if(c<nums[i]) f[(i+1)%2][c]=f[i%2][c];
                else f[(i+1)%2][c]=f[i%2][c]+f[i%2][c-nums[i]];
            }
        }
        return f[n%2][target];    
#endif
```


##### 空间优化：一个数组

能不能只用一个数组呢？


$$f[c]=f[c]+f[c-w[j]$$


如果 从左到右算会把前面的覆盖掉，当更新`f[c]`的值时，`f[c-w[i]]`可能已经被之前的操作更新过，因为`c-w[i] < c`。这意味着，当你在计算`f[c]`时，用到的`f[c-w[i]]`不再是原始问题中这个容量下的最大价值，而是在加入了一些物品后的最大价值。



> [!question] **为什么01背包是从右向左算**：
> - 为了保证每个物品只被选取一次，我们在更新`f[c]`时，需要保证这次使用的是没有被当前物品影响的状态。
> - 因此，我们从大到小更新`c`，确保在计算`f[c]`时，使用的`f[c-w[i]]`是在未选择当前物品`i`的情况下的状态。
> - 如果从左向右更新，那么在更新到`f[c]`时，较小的`c`已经被当前轮次的物品更新过，这样就违反了每个物品只能选一次的限制。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240408113241.png" alt="image.png" style="zoom:60%;" />

因为只有一个数组，上图的3被更新了，不是先要的结果
从右到左算就没这个问题

```c++
#if 1 //一个数组
        int f[target+1];
        memset(f,0,sizeof(f));
        f[0]=1; // 这个初始值来自于边界条件 i==0,c==0为1
        for(int i=0;i<n;++i){
            for(int c=target;c>=nums[i];--c){
                f[c]+=f[c-nums[i]];
            }
        }
        return f[target];    
#endif
```


#### 完全背包

完全背包:有$n$种物品，第$i$种物品的体积为$w[i]$，价值为$v[i]$ 每种物品**无限次重复选**，求体积和不超过$capacity$时的最大价值和

> [!summary] 回溯三问
> - 当前操作? 枚举第$i$个物品选或不选:
> 	- 不选,剩余容量不变;选，剩余容量减少$w[i]$
> 
> - 子问题? 在剩余容量为$c$时，
> 	- 从前$i$个物品中得到的最大价值和
> 
> - 下一个子问题? 分类讨论:
> 	- 不选:在剩余容量为$c$时，
> 		- 从前$i-1$个物品中得到的最大价值和
> 	- 选:在剩余容量为$c-w[i]$时，
> 		- 从前$i$个物品中得到的最大价值和


$$dfs(i,c)=max(dfs(i-1,c),dfs(i,c-w[i])+v[i])$$

看力扣的[322题](https://leetcode.cn/problems/coin-change/)

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。
计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

**示例 1：**

> **输入：** coins = `[1, 2, 5]`, amount = `11`
> **输出：**`3` 
> **解释：** 11 = 5 + 5 + 1



##### 递推

$$f[i][c]=min(f[i-1][c],f[i][c-cins[i]]+1)$$

```c++
    #if 0 // 递推
        int n=coins.size(),f[n+1][amount+1];
        memset(f,0x3f,sizeof(f));
        f[0][0]=0;
        for(int i=0;i<n;i++){
            for(int c=0;c<=amount;++c){
                if(c<coins[i]) 
                    f[i+1][c]=f[i][c];
                else f[i+1][c]=min(f[i][c],f[i+1][c-coins[i]]+1);
            }
        }

        int ans = f[n][amount];
        return ans < 0x3f3f3f3f ? ans : -1;
    #endif
```


##### 空间优化：两个数组（滚动数组） 

```c++
    #if 0 // 滚动数组
        int n=coins.size(),f[2][amount+1];
        memset(f,0x3f,sizeof(f));
        f[0][0]=0;
        for(int i=0;i<n;i++){
            for(int c=0;c<=amount;++c){
                if(c<coins[i]) 
                    f[(i+1)%2][c]=f[i%2][c];
                else f[(i+1)%2][c]=min(f[i%2][c],f[(i+1)%2][c-coins[i]]+1);
            }
        }

        int ans = f[n%2][amount];
        return ans < 0x3f3f3f3f ? ans : -1;
    #endif
```


##### 空间优化：一个数组 

$$f[c]=min[f[c],f[c-cins[i]]+1]$$

> [!question] **为什么完全背包是从左向右算**：
> - 由于物品可以被无限次选取，在计算`f[c]`时，我们希望使用已经可能包含当前物品`i`的状态来更新更大的容量`c`。
> - 从左向右更新`c`意味着，在计算到`f[c]`时，`f[c-w[i]]`可能已经包含了物品`i`的价值，这符合完全背包允许物品多次选择的特点。
> - 如果从右向左更新，那么我们可能会错过一些因为多次选择同一物品而获得更高价值的机会。

```c++
    
    #if 1 // 一个数组
        int n=coins.size(),f[amount+1];
        memset(f,0x3f,sizeof(f));
        f[0]=0;
        for(int x:coins){
            for(int c=x;c<=amount;++c){
                f[c]=min(f[c],f[c-x]+1);
            }
        }

        int ans = f[amount];
        return ans < 0x3f3f3f3f ? ans : -1;
    #endif
```


### 边界条件

求最大值/最小值问题

- **求体积恰好为j的情况：**
    - 求最大值时，初始化`f[0] = 0`（体积为0时，价值也为0），其余`f[t] = -∞`（`t>0`），意味着除了体积为0外，其他体积的初始价值非常低，几乎不可能达到。
    - 求最小值时，初始化`f[0] = 0`，其余`f[t] = ∞`，表示除了体积为0外，其他体积的初始成本非常高。
    - 这样设置的目的是为了在更新状态时，确保只有当体积恰好为j时，才会记录相应的价值或成本。
- **求体积至多为j的情况：**
    - 不管是求最大值还是最小值，都可以将`f[0]`初始化为0，其余位置也初始化为0，因为这里我们关注的是体积不超过j时的最优值。
- **求体积至少为j的情况：**
    - 这种情况下，初始化仍然是`f[0] = 0`，其余位置也为0。
    - 在遍历时，01背包问题需要从大到小遍历，而完全背包问题则从小到大遍历。这是为了确保在更新状态时，考虑的是不同的物品（01背包）或同一物品多次（完全背包）。

求方案数问题

- **求体积恰好为j的情况：**
    - 初始化`f[0] = 1`，表示体积为0时有一种方案（什么都不取），而其余`f[t]`初始化为0，因为初始时没有其他体积的方案。
- **求体积至多为j的情况：**
    - 初始化全部为`f[t] = 1`，这里的意思是每种体积从0开始都至少有一种方案（什么都不取）。
- **求体积至少为j的情况：**
    - 初始化`f[0] = 1`，其余为0。
    - 更新策略与求最大值/最小值问题类似，01背包问题从大到小遍历，完全背包问题从小到大遍历。


### 线性DP

#### 最长公共子序列

以力扣的[1143题](https://leetcode.cn/problems/longest-common-subsequence/)为例
子序列本质上就是**选或不选**考虑最后一对字母,分别叫x和y
- 不选x不选y
- 不选x选y
- 选x不选y
- 选x选y

一般化

> [!summary] 回溯三问
> - 当前操作? 考虑s[i]和t[j]选或不选
> 	- 子问题? s的前i个字母和t的前j个字母的LCS长度
> - 下一个子问题?
> 	- s的前i-1个字母和t的前j-1个字母的LCS长度
> 	- s的前i-1个字母和t的前j个字母的LCS长度
> 	- s的前i个字母和t的前j-1个字母的LCS长度


$$dfs(i,j)=\begin{cases}
max(dfs(i-1,j),dfs(i,j-1),dfs(i-1,j-1)+1) \quad ? \quad s[i]=t[j] \\
max(dfs(i-1,j),dfs(i,j-1),dfs(i-1,j-1)) \quad ? \quad s[i]!=t[j] \\
\end{cases}$$


$$dfs(i,j)=max(dfs(i-1,j),dfs(i,j-1),dfs(i-1,j-1)+(s[i]=t[j]))$$



$$不能忽略的问题 \begin{cases} 
在s[i]=t[j]时,需要dfs(i-1,j)和dfs(i,j-1)吗? \\
在s[i]≠t[j]时,需要dfs(i-1,j-1)吗? \\
\end{cases}$$ 

最终的公式简化为

$$dfs(i,j)=\begin{cases}
dfs(i-1,j-1)+1 \quad  s[i]=t[j] \\
max(dfs(i-1,j),dfs(i,j-1)) \quad s[i]!=t[j] \\
\end{cases}$$

##### 记忆化搜索 

```c++
#if 0 // 记忆化搜索
        int cache[n][m];
        memset(cache, -1, sizeof(cache));
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i < 0 || j < 0)
                return 0;
            int& res = cache[i][j];
            if (res != -1)
                return res;
            if (text1[i] == text2[j]) {
                return res = (dfs(i - 1, j - 1) + 1);
            }

            else {
                return res = max(dfs(i - 1, j), dfs(i, j - 1));
            }
        };
        return dfs(n - 1, m - 1);
#endif
```



##### 递推

$$f[i][j]=\begin{cases}
f[i-1][j-1]+1 \quad  s[i]=t[j] \\
max(f[i-1][j],f[i][j-1]) \quad s[i]!=t[j] \\
\end{cases}$$


$$f[i+1][j+1]=\begin{cases}
f[i][j]+1 \quad  s[i]=t[j] \\
max(f[i][j+1],f[i+1][j]) \quad s[i]!=t[j] \\
\end{cases}$$


```c++
#if 1 // 递推
        int f[n + 1][m + 1];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (text1[i] == text2[j])
                    f[i + 1][j + 1] = f[i][j] + 1;
                else
                    f[i + 1][j + 1] = max(f[i + 1][j], f[i][j + 1]);
            }
        }
        return f[n][m];
#endif
```

##### 空间优化：一个数组 

我们可以观察到每次状态更新只依赖于当前行的前一个元素和上一行的元素。变量`pre`用于存储在更新`f[j]`之前的`f[j-1]`的值，这对于处理当前字符匹配的情况（`text1[i-1] == text2[j-1]`）非常关键，因为这需要我们使用未被当前行更新的上一行的值。
```c++
#if 1 // 优化一维数组
        int f[m+1];
        memset(f,0,sizeof(f));
        for (int x:text1) {
            for (int j = 0,pre=0; j < m; ++j) {
                int tmp=f[j+1];
                f[j+1]= x==text2[j]?pre+1:max(f[j+1],f[j]);
                pre=tmp; //更新prev为当前行更新前的f[j]值，用于下一次循环
            }
        }
        return f[m];
#endif
```

#### 编辑距离： 等价交换

以[72题](https://leetcode.cn/problems/edit-distance/)为例

这里要等价交换
- 插入1个数，使之相同 相当于把$t[j]$去掉了
- 删除1个数， 相当于把$s[i]$去掉了
- 修改一个数 就是把$s[i]$,$t[j]$都去掉了

$$dfs(i,j)=\begin{cases}
dfs(i-1,j-1) \quad  s[i]=t[j] \\
min(dfs(i,j-1),dfs(i-1,j),dfs(i-1,j-1))+1 \quad s[i]!=t[j] \\
\end{cases}$$


```c++
#if 1 // 记忆化搜索

        int n = word1.size(), m = word2.size(), cache[n + 1][m + 1];
        memset(cache, -1, sizeof(cache));
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i < 0)
                return j + 1; // 如果一个字符串为空，需要把另一个字符串的都去掉
            if (j < 0)
                return i + 1;
            int& res = cache[i][j];
            if (res != -1)
                return res;

            if (word1[i] == word2[j])
                return res = dfs(i - 1, j - 1);
            return res =
                       min(min(dfs(i - 1, j), dfs(i, j - 1)), dfs(i - 1, j - 1)) + 1;
        };
        return dfs(n - 1, m - 1);
#endif
```



改成递推

$$f[i+1,j+1]=\begin{cases}
f[i][j] \quad  s[i]=t[j] \\
min(f[i+1][j],f[i][j+1],f[i-1][j-1])+1 \quad s[i]!=t[j] \\
\end{cases}$$



```c++
#if 0 // 递推

        int n = word1.size(), m = word2.size();
        int f[n + 1][m + 1];
        for(int j=0;j<=m;++j) f[0][j]=j;
        for(int i=0;i<=n;++i) f[i][0]=i;
        for(int i=0;i<n;++i){
            for(int j=0;j<m;++j){
                f[i+1][j+1]=word1[i]==word2[j]?f[i][j]:min(min(f[i+1][j],f[i][j+1]),f[i][j])+1;
            }
        }
        return f[n][m];     
#endif
```

改成一个数组

```c++
#if 1 // 一个数组

    int m=word2.size(),f[m+1];
    iota(f,f+m+1,0);

    for(char x:word1){
        int pre=f[0];
        ++f[0];
        for(int j=0;j<m;++j){
            int tmp=f[j+1];
            f[j+1]= word2[j]==x?pre:min(min(f[j+1],f[j]),pre)+1;
            pre=tmp;
        }
    }
    return f[m];

#endif
```



#### 最长递增子序列

力扣的第[300](https://leetcode.cn/problems/longest-increasing-subsequence/description/)道题

- 递增子序列    IS,Increasing Subsequence
- 最长递增子序列   LIS,Longest Increasing Subsequence
- $O(n^2)$   回溯→记忆化搜索→递推
- $O(nlogn)$  二分+贪心

	(<font color=#ff0000>1</font>,6,7,<font color=#ff0000>2</font>,<font color=#ff0000>4</font>,<font color=#ff0000>5</font>,3)

- 思路1:选或不选   为了比大小,需要**知道上一个选的数字**
- 思路2:枚举选哪个   比较当前选的数字和下一个要选的数字



> [!summary] 启发思路
> - 枚举$nums[i]$作为 LIS 的末尾元素,
> - 那么需要枚举$nums[j]$作为LIS倒数第二个元素,
> - 其中$j<i$且$nums[j]<nums[i]$




> [!summary] 回溯三问
> - 子问题? 以nums[i]结尾的LIS长度
> - 当前操作? 枚举nums[j]
> - 下一个子问题? 以nums[j]结尾的LIS长度

$$dfs(i)=max\{dfs(j)\}+1 \quad  j<i且nums[j]<nums[i]$$


$$f[i]=max\{f[j]\}+1 \quad  j<i且nums[j]<nums[i]$$

##### 记忆化搜索

```c++
#if 1 // 记忆化搜索
        vector<int> cache(n + 1, 0);
        function<int(int)> dfs = [&](int i) {
        
            int &res=cache[i];
            if(res>0) return res;

            for(int j=0;j<i;++j){
                if(nums[j]<nums[i])
                    res=max(res,dfs(j));
            }

            return ++res;

        };

        int ans=0;
        for(int i=0;i<n;i++){
            ans=max(ans,dfs(i)); //这里只有1步因为有cache
        }
        return ans;
#endif
```


##### 递推

```c++
#if 1 // 递推
        vector<int>f(n+1,0);

        for(int i=0;i<n;++i){
            for(int j=0;j<i;++j){
                if(nums[j]<nums[i]){
                    f[i]=max(f[i],f[j]);
                }
            }
            f[i]+=1;
        }

        int ans=0;
        for(auto &it:f){
            ans=max(ans,it);
        }

        return ans;
#endif
```

思路3
- nums的LIS等价于nums与排序去重后的nums的LCS
	- 例如$nums=[1,3,3,2,4]$
	- 排序去重后$=[1,2,3,4]$
	- $LCS=[1,3,4]或者[1,2,4]$


##### 交换状态与状态值

进阶技巧:交换状态与状态值
- $f[i]$表示末尾元素为$nums[i]$的LIS长度
- $g[i]$表示长度为$i+1$的IS的末尾元素的最小值


```
例如nums=[1,6,7,2,4,5,3]
	g=[1]        g[0]=1
	g=[1,6]      g[0]=1 因为6>1 g[1]=6
	g=[1,6,7]    g[2]=7
	g=[1,2,7]    有一个上升的子序列1,2，这里g[1]=2
	g=[1,2,4]    有一个上升的子序列1,2,4，这里g[2]=2
	g=[1,2,4,5]
	g=[1,2,3,5]
```

- 当我们定义为最小值才更有机会扩展g的长度，因为较小的末尾元素更容易被后续的更大元素所接续。
- 没有重叠子问题，不算dp，是一个贪心问题
- g是一个严格递增的序列，要么添加一个数，要么修改一个数

- 定理:g是严格递增的。
	- 证明:假设有$g[i]≥g[j]$且$i<j$
	- 那么$g[j]$对应的IS的第i个数就应该$<g[j$],
	- 这与$g[i] ≥g[j]$矛盾。


- 推论1:一次只能更新一个位置。
	- 证明:假设更新了两个位置,会导致g不是严格递增的,因为单调递增序列不能有相同元素。

- 推论2:更新的位置是第一个$≥nums[i]$的数的下标。如果$nums[i]$比$g$的最后一个数都大,那就加到$g$的末尾。


- 证明:设更新了$g[j]$,如果$g[j]<nums[i]$,
	- 相当于把小的数给变大了,这显然不可能。
	- 另外,如果$g[j]$不是第一个$≥nums[i]$的数,
	- 那就破坏了$g$的有序性。
	- $g=[1,6,7],nums[i]=2$


- **算法**:在$g$上用**二分查找**快速找到第一个$≥nums[i]$的下标j。
	- 如果j不存在,那么$nums[i]$直接加到g末尾;
	- 否则修改$g[j]$为$nums[i]$。

	注:这个算法按分类的话,算「贪心+二分」。


```c++
#if 1 // 贪心 + 二分查找
        vector<int>g;
        for(int x:nums){
            auto it=std::lower_bound(g.begin(),g.end(),x); // 严格递增用lower_bound，非严格用upper_bound
            if(it==g.end()) {
                g.push_back(x);
            }
            else *it=x;
        }
        return g.size();
#endif
```


变形:如果LIS中可以有相同元素呢?(非严格递增)
那么$g$是非严格递增序列。
在修改的时候,和$nums[i]$相同的$g[j]$就不用改了,
而是修改$>nums[i]$的第一个$g[j]$。

```
例如nums=[1,6,7,2,2,5,2]
	g=[1]
	g=[1,6]
	g=[1,6,7]
	g=[1,2,7]
	g=[1,2,2]
	g=[1,2,2]
	g=[1,2,2,5]
	g=[1,2,2,2]
```

#### 最大子数组和

1. 前缀和+动态规划

	由于子数组的元素和等于两个前缀和的差，所以求出 nums 的前缀和，问题就变成 买卖股票的最佳时机了。本题子数组不能为空，相当于一定要交易一次。
	可以一边遍历数组计算前缀和，一边维护前缀和的最小值（相当于股票最低价格），用当前的前缀和（卖出价格）减去前缀和的最小值（买入价格），就得到了以当前元素结尾的子数组和的最大值（利润），用它来更新答案的最大值（最大利润）

	```c++
	int maxSubArray(vector<int> &nums) {
		int ans = INT_MIN;
		int min_pre_sum = 0;
		int pre_sum = 0;
		for (int x : nums) {
			pre_sum += x; // 当前的前缀和
			ans = max(ans, pre_sum - min_pre_sum); // 减去前缀和的最小值
			min_pre_sum = min(min_pre_sum, pre_sum); // 维护前缀和的最小值
		}
		return ans;
	}
	```

2. Kadane算法
	Kadane算法的基本思想非常简单直观：遍历数组，同时维护两个变量，一个用于追踪到目前为止发现的最大子数组和（全局最大），另一个用于在每个步骤中考虑当前元素是否要被加入到当前的子数组和中（局部最大）

	1. **初始化两个变量**：`currentMax` 初始化为数组的第一个元素，代表当前遍历到的点的最大子数组和；`globalMax` 也初始化为数组的第一个元素，代表全局的最大子数组和。
	2. **遍历数组**：从第二个元素开始遍历数组。
	3. **更新 `currentMax`**：对于每个元素 `nums[i]`，更新 `currentMax` 为 `max(nums[i], currentMax + nums[i])`。这里的想法是，对于每个点，你要决定是继续加入之前的子数组以增加总和，还是从当前点开始一个新的子数组。这一步骤负责维护局部最大值。
	4. **更新 `globalMax`**：然后，更新 `globalMax` 为 `max(globalMax, currentMax)`，以保持追踪到目前为止找到的最大子数组和。这一步骤负责维护全局最大值。
	5. **返回结果**：数组遍历完成后，`globalMax` 将包含整个数组的最大子数组和。

	```c++
	#if 1 // Kadane算法
	        // 这道题要和最大，不是长度最大
	
	        if (nums.empty())
	            return 0;
	        int currentMax = nums[0], globalMax = nums[0];
	        for (int i = 1; i < n; ++i) {
	            currentMax = max(nums[i], currentMax + nums[i]);
	            globalMax = max(globalMax, currentMax);
	        }
	        return globalMax;
	
	#endif
	```



### 状态机DP

#### 买卖股票的最佳时机II (不限交易次数)

力扣[第122题](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

$prices=[7,1,5,3,6,4]$
**启发思路**:最后一天发生了什么?
- 从第0天开始到第5天结束时的利润=从第0天开始到第4天结束时的利润+第5天的利润
	- 什么不干 0
	- 买入 -4
	- 出售 4


**关键词**:天数、是否持有股票
子问题? 到第i天结束时,持有/未持有股票的最大利润
下一个子问题? 到第i-1天结束时,持有/未持有股票的最大利润

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240409171056.png?" alt="image.png" style="zoom:60%;" />

- 定义dfs(i,0)表示到第i天**结束**时,未持有股票的最大利润
- 定义dfs(i,1)表示到第i天**结束**时,持有股票的最大利润

- 由于第i-1天的结束就是第i天的开始
- dfs(i-1,·)也表示到第i天**开始**时的最大利润

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240409171348.png" alt="image.png" style="zoom:60%;" />

$$dfs(i,0)=max(dfs(i-1,0),dfs(i-1,1)+prices[i])$$

$$dfs(i,1)=max(dfs(i-1,1),dfs(i-1,0)-prices[i])$$


- 递归边界:
	- $dfs(-1,0)=0$   第0天开始未持有股票,利润为0
	- $dfs(-1,1)=-∞$    第0天开始不可能持有股票

**递归入口**:$max(dfs(n-1,0),dfs(n-1,1)=dfs(n-1,0)$


##### 记忆化搜索

```c++
#if 0 // 记忆化搜索
        int cache[n][2];
        memset(cache,-1,sizeof(cache));
        function<int(int,bool)>dfs=[&](int i,bool hold){
            if(i<0){
                return hold?INT_MIN:0;
            }
            int &res=cache[i][hold];
            if(res!=-1) return res;
            if(hold){ // 持有股票
                return res=max(dfs(i-1,true),dfs(i-1,false)-prices[i]);
            }
            else{ // 
                return res=max(dfs(i-1,false),dfs(i-1,true)+prices[i]);
            }
        };
        return dfs(n-1,false);

#endif
```

##### 递推

$$f[i][0]=max(f[i-1][0],f[i-1][1]+prices[i])$$

$$f[i][1]=max(f[i-1][1],f[i-1][0]-prices[i])$$


但这样没有状态表示$f[-1][0]$和$f[-1][1]$
那就在f的最前面插入一个状态

- 最终递推式
	- $f[0][0]=0$
	- $f[0][1]=-∞$
	- $f[i+1][0]=max(f[i]][0],f[i]+prices[i])$
	- $f[i+1][1]=max(f[i][1],f[i][0]-prices[i])$

答案为$f[n][0]$

```c++
#if 1 // 递推
        vector<vector<int>>f(n+1,vector<int>(2,0));
        f[0][1]=INT_MIN;

        for(int i=0;i<n;++i){
            f[i+1][0]=max(f[i][0],f[i][1]+prices[i]);
            f[i+1][1]=max(f[i][1],f[i][0]-prices[i]);
        }
        return f[n][0];

#endif
```

##### 空间优化：一个数组 

```c++
#if 1 // 一个数组
        int f0=0,f1=INT_MIN;
        for(int p:prices){
            int new_f0=max(f0,f1+p);
            f1=max(f1,f0-p);
            f0=new_f0;
        }

#endif
```


#### 买卖股票的最佳时机IV(至多交易k次)

定义$dfs(i,j,0)$表示到第$i$天**结束**时完成**至多**$j$笔交易,未持有股票票的最大利润
定义$dfs(i,j;1)$表示到第$i$天**结束**时完成**至多**$j$笔交易,持有股票最大利润

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240409184724.png" alt="image.png" style="zoom:60%;" />

$$dfs(i,j,0)=max(dfs(i-1,j,0),dfs(i-1,j,1)+prices[i])$$

$$dfs(i,j,1)=max(dfs(i-1,j,1),dfs(i-1,j-1,0)-prices[i])$$

递归边界:
- $dfs(,-1,)=-∞$    任何情况下,j都不能为负
- $dfs(-1,j,0)=0$    第0天**开始**未持有股票,利润为0
- $dfs(-1,j,1)=-∞$  第0天**开始**不可能持有股票

递归**入口**:$max(dfs(n-1,k,0),dfs(n-1,k,1) =dfs(n-1,k,0)$

```c++
#if 0 // 记忆化搜索
        int cache[n][k+1][2];
        memset(cache,-1,sizeof(cache));
        function<int(int,int,bool)>dfs=[&](int i,int j,bool hold){
            if(j<0||(i<0&&hold)) return INT_MIN/2;
            if(i<0&&!hold) return 0;
            int &res=cache[i][j][hold];
            if(res!=-1) return res;
            if(hold){
                return res=max(dfs(i-1,j,true),dfs(i-1,j-1,false)-prices[i]);
            }
            else{
                
                return res=max(dfs(i-1,j,false),dfs(i-1,j,true)+prices[i]);

            }

        };

        return dfs(n-1,k,0);
#endif
```


##### 递推

$$f[i][j][0]=max(f[i-1][j][0],f[i-1][j][1]+prices[i]$$


$$f[i][j][1]=max(f[i-1][j][1],f[i-1][j-1][0]-prices[i]$$

但这样没有状态表示$f[-1][·][·]和[-1][-1][·]$
那就在$f$和每个$f[i]$的最前面插入一个状态

**最终递推式**
- $f[·][0][·]=-∞$
- $f[0][j][0]=0 \quad j≥1$
- $f[0][j][1]=-∞ \quad  j≥1$
- $f[i+1][j][0]=max(f[i][i]][0],f[i][j]+prices[i])$
- $f[i+1][j][1]=max(f[i][1][1],f[i][j-1][0]-prices[i]$
- 答案为$f[n][k+1][0]$

```c++
#if 1 // 递推
        int f[n+1][k+2][2];
        memset(f,-0x3f,sizeof(f));

        for(int j=1;j<=k+1;++j){
            f[0][j][0]=0;
        }

        for(int i=0;i<n;++i){
            for(int j=1;j<=k+1;++j){
                f[i+1][j][0]=max(f[i][j][0],f[i][j][1]+prices[i]);
                f[i+1][j][1]=max(f[i][j][1],f[i][j-1][0]-prices[i]);
            }
        }
        return f[n][k+1][0];
        
#endif
```


##### 空间优化

```c++
#if 1 // 空间优化
        int f[k+2][2];
        memset(f,-0x3f,sizeof(f));

        for(int j=1;j<=k+1;++j){
            f[j][0]=0;
        }

        for(int i=0;i<n;++i){
            for(int j=1;j<=k+1;++j){
                f[j][0]=max(f[j][0],f[j][1]+prices[i]);
                f[j][1]=max(f[j][1],f[j-1][0]-prices[i]);
            }
        }
        return f[k+1][0];
        
#endif
```

##### 变形

Follow up:改成**恰好/至少**交易k次,要怎么初始化?

**恰好**:$f[0][1][0]=0$,其余=$-∞$,(注意前面塞了个状态,$f[0][1]$才是恰好完成0次的状态)


**至少**:$f[i][-1][·]$等价于$f[i][0][·]$
所以每个$f[i]$的最前面不需要插入状态
「至少0次」等价于「可以无限次交易」

所以$f[i][0][·]$就是无限次交易下的最大利润,转移方程也一样
- $f[0][0][0]=0$,其余$=-∞$
- $f[i+1][0][0]=max(f[i]][0],f[i]]+prices[i])$
- $f[i+1][0][1]=max(f[i]][1],f[i][0][0]-prices[i])$


### 区间DP

- 区间DP
	- 区别
		- 线性DP:一般是在前缀/后缀上转移
		- 区间DP:从小区间转移到大区间
	- 选或不选
		- 从两侧向内缩小问题规模
		- 516.最长回文子序列
	- 枚举选哪个
		- 分割成多个规模更小的子问题
		- 1039.多边形三角剖分的最低得分

#### 最长回文子序列

s=eacbba
思路1:`[转换]`求s和反转后s的LCS(最长公共子序列)
- $s=eacbba$
- $S_{rev}=abbcae$

思路2:`[选或不选]`从两侧向内缩小问题规模

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240409202433.png" alt="image.png" style="zoom:40%;" />

(类似LCS)
定义$dfs(i,j)$表示从$s[i]$到$s[j]$的最长回文子序列的长度


$$
dfs(i,j)=\begin{cases}
dfs(i+1,j-1)+2 \quad  s[i]=s[j]  \\
max(dfs(i+1,j),dfs(i,j-1)) \quad  s[i]!=s[j] \\
\end{cases}$$ 

递归**边界：**

$dfs(i,i)=1$ 
$dfs(i+1,i)=0$
比如遇到bb时,会从$dfs(i,i+1)$递归到$dfs(i+1,i)$

递归**入口**：$dfs(0,n-1)$

##### 递推

```c++
#if 1 // 递推
      // dfs(i,j)=dfs(i+1,j-1)+2;              s[i]=s[j]
      // dfs(i,j)=max(dfs(i+1,j),dfs(i,j-1));              s[i]!=s[j]

        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][i] = 1;
        }
        
        int i = 0, j = n - 1;
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = s[i] == s[j] ? f[i + 1][j - 1] + 2
                                       : max(f[i + 1][j], f[i][j - 1]);
            }
        }
        return f[0][n - 1];
#endif
```


##### 空间优化：一个数组 

```
#if 1 // 空间优化
      // dfs(i,j)=dfs(i+1,j-1)+2;              s[i]=s[j]
      // dfs(i,j)=max(dfs(i+1,j),dfs(i,j-1));              s[i]!=s[j]

        int f[n];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; i >= 0; --i) {
            f[i]=1;
            int pre=0;
            for (int j = i + 1; j < n; ++j) {
                int tmp=f[j];
                f[j] = s[i] == s[j] ? pre + 2
                                       : max(f[j], f[j - 1]);
                pre=tmp;
            }
        }
        return f[n - 1];
#endif
```

### 树形DP

#### 二叉树和树

以力扣的[543题](https://leetcode.cn/problems/diameter-of-binary-tree/description/)为例

换个角度看直径: 从一个叶子出发向上,在某个节点「拐弯」,向下到达另一个叶子。 得到了由两条**链**拼起来的路径。(也可能只有一条链)

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240410034428.png" alt="image.png" style="zoom:50%;" />


- 遍历二叉树,在计算最长链的同时,顺带把直径算出来。
- 在当前节点「拐弯」的直径长度=左子树的最长链+右子树的最长链+2
- 返回给父节点的是以**当前节点为根的子树的最长链**=max(左子树的最长链,右子树的最长链)+1。

```c++
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
           int ans=0;
           function<int(TreeNode*)>dfs=[&](TreeNode *node){
                if(node==nullptr) return -1;

                int l_len=dfs(node->left)+1;
                int r_len=dfs(node->right)+1;
                ans=max(ans,l_len+r_len);
                return max(l_len,r_len);

           };
           dfs(root);
           return ans;
    }
};
```


[124.二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/description/)


<img src="https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg" alt="image.png" style="zoom:60%;" />

算法
遍历二叉树,在计算最大链和的同时,顺带更新答案的最大值值。

在当前节点「拐弯」的最大路径 和
=左子树最大链和+右子树最大链和+当前节点值。

返回给父节点的是max(左子树最大链和,右子树最大链和)+当前节;点值
如果这个值是负数,则返回0。

```c++
int maxPathSum(TreeNode* root) {
        int ans=-1001;

        function<int(TreeNode*)>dfs=[&](TreeNode *node){
            if(node==nullptr) return 0;
            int lval=dfs(node->left);
            int rval=dfs(node->right);
            ans=max(ans,lval+rval+node->val);

            return max(max(lval,rval)+node->val,0);
        };
        dfs(root);
        return ans;
    }
```


[相邻字符不同的最长路径](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/description/)

<img src="https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png" alt="image.png" style="zoom:60%;" />

```c++
class Solution {
public:
    int longestPath(vector<int> &parent, string &s) {
        int n = parent.size();
        vector<vector<int>> g(n);
        for (int i = 1; i < n; ++i)
            g[parent[i]].push_back(i);

        int ans = 0;
        function<int(int)> dfs = [&](int x) -> int {
            int maxLen = 0;
            for (int y : g[x]) {
                int len = dfs(y) + 1;
                if (s[y] != s[x]) { // 加了限制条件，满足条件的才去修改
                    ans = max(ans, maxLen + len);
                    maxLen = max(maxLen, len);
                }
            }
            return maxLen;
        };
        dfs(0);
        return ans + 1;
    }
};
```



[最大价值和与最小价值和的差值](https://leetcode.cn/problems/difference-between-maximum-and-minimum-price-sum/description/)

<img src="https://assets.leetcode.com/uploads/2022/12/01/example14.png" alt="image.png" style="zoom:60%;" />


```c++
long long maxOutput(int n, vector<vector<int>> &edges, vector<int> &price) {
	vector<vector<int>> g(n);
	for (auto &e : edges) {
		int x = e[0], y = e[1];
		g[x].push_back(y);
		g[y].push_back(x); // 建树
	}

	long ans = 0;
	function<pair<long, long>(int, int)> dfs = [&](int x, int fa) -> pair<long, long> { 
		long p = price[x], max_s1 = p, max_s2 = 0;
		for (int y : g[x])
			if (y != fa) {
				auto[s1, s2] = dfs(y, x);
				// 前面最大带叶子的路径和 + 当前不带叶子的路径和
				// 前面最大不带叶子的路径和 + 当前带叶子的路径和
				ans = max(ans, max(max_s1 + s2, max_s2 + s1));
				max_s1 = max(max_s1, s1 + p);
				max_s2 = max(max_s2, s2 + p); // 这里加上 p 是因为 x 必然不是叶子
			}
		return {max_s1, max_s2};
	};
	dfs(0, -1);
	return ans;
}
```

在递归中常用这个参数是当前节点坐标和父节点坐标


[打家劫舍III](https://leetcode.cn/problems/house-robber-iii/description/)

<img src="https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg" alt="image.png" style="zoom:60%;" />

儿子的儿子最多要考虑四个节点，还要判断空，写起来麻烦，选与不选可以看做一种状态机，在选当前节点的情况下最大值是多少，不选是多少

- 选或不选
	- 选当前节点:左右儿子都不能选
	- 不选当前节点:左右儿子可选可不选

- 提炼状态
	- 选当前节点时,以当前节点为根的**子树**最大点权和
	- 不选当前节点时,以当前节点为根的**子树**最大点权和

- 转移方程
	- 选=左不选+右不选+当前节点值
	- 不选=max(左选,左不选)+max(右选,右不选)

- 最终答案=max(根选,根不选)

```c++
    int rob(TreeNode* root) {
        // 有边的不能偷
        
        function<pair<int, int>(TreeNode*)>dfs=[&](TreeNode *node) -> pair<int, int> {
            if(node==nullptr)  return {0, 0};
            auto [l_rob,l_not_rob]=dfs(node->left);
            auto [r_rob,r_not_rob]=dfs(node->right);

            int rob=l_not_rob+r_not_rob+node->val; // 选
            int not_rob=max(l_rob,l_not_rob)+max(r_rob,r_not_rob);
            return {rob,not_rob};
        };  
        auto [root_rob, root_not_rob] = dfs(root);
        return max(root_rob,root_not_rob);
    }
```




[监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/description/)

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_01.png" alt="image.png" style="zoom:60%;" />

**选或者不选**
1. 选 在这个节点安装摄像头
2. 不选 在它的父节点安装摄像头
3. 不选 在它的左/右儿子安装摄像头

- 蓝色:安装摄像头
- 黄色:不安装摄像头,且它的父节点安装摄像头
- 红色:不安装摄像头,且它的至少一个儿子安装摄像头

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240410171551.png" alt="image.png" style="zoom:60%;" />

- 子树根节点为蓝色时,这棵子树最少需要多少摄像头
- 子树根节点为黄色时,这棵子树最少需要多少摄像头
- 子树根节点为红色时,这棵子树最少需要多少摄像头

- 蓝色=min(左蓝,左黄,左红)+min(右蓝,右黄,右黄,右红)+1

- 黄色=min(左蓝,左红)+min(右蓝,右红)

- 红色=min(左蓝+右红,左红+右蓝,左蓝+右蓝)

二叉树的根节点没有父节点,不可能是黄色,所以
最终答案=min(根节点为蓝色,根节点为红色)


递归边界
- 空节点不能装摄像头:蓝色为 ,表示不合法
- 空节点不需要被监控:黄色和红色都为0


```c++
    int minCameraCover(TreeNode* root) {
        function<tuple<int,int,int>(TreeNode *)>dfs=[&](TreeNode *node) ->tuple<int,int,int>{
            if(node==nullptr) return {INT_MAX/2,0,0};

            auto [l_choose,l_by_fa,l_by_children]=dfs(node->left);
            auto [r_choose,r_by_fa,r_by_children]=dfs(node->right);
            int choose=min(l_choose,l_by_fa)+min(r_choose,r_by_fa)+1;
            int by_fa=min(l_choose,l_by_children)+min(r_choose,r_by_children);
            int by_children=min({l_choose+r_by_children,l_by_children+r_choose,l_choose+r_choose});
            return {choose,by_fa,by_children};
        };
        auto [choose,_,by_children]=dfs(root);
        return min(choose,by_children);
    }
```

#### 树上前缀和
## 图论

### 图论基础

- 邻接矩阵

```c++
vector<vector<int>> graph(n + 1, vector<int>(n + 1, 0));
```

输入m个边，构造方式如下：

```c++
while (m--) {
    cin >> s >> t;
    // 使用邻接矩阵 ，1 表示 节点s 指向 节点t
    graph[s][t] = 1;
}
```

- 邻接表

邻接表 使用 **数组 + 链表**的方式来表示。 邻接表是从边的数量来表示图，有多少边 才会申请对应大小的链表。

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20240223103713.png" alt="image.png" style="zoom:60%;" />

```c++
// 节点编号从1到n，所以申请 n+1 这么大的数组
vector<list<int>> graph(n+1); // 邻接表， list为c++中的链表
```

输入m个边，构造方式如下： 

```c++
while (m--) {
    cin >> s >> t;
    // 使用邻接表 ，表示 s -> t 是相连的
    graph[s].push_back(t);
}
```

- 哈希表

```c++
struct pair_hash {
    template <class T1, class T2>
    size_t operator()(const pair<T1,T2>& p) const {
        return hash<T1>{}(p.first) ^ (hash<T2>{}(p.second) << 1);
    }
};
```

构造方式：
```c++
unordered_map<pair<int,int>, int, pair_hash> graph;

构造方式：
while (m--) {
    cin >> s >> t >> w;  // w 为边的权重，如果是无权图，可以省略或设为1
    graph[{s, t}] = w;
    // 如果是无向图，还需要添加：graph[{t, s}] = w;
}
```

特点：
- 空间复杂度：O(m)，其中m为边数 
- 查询边存在性：平均 O(1)，最坏情况 O(m) 
- 适合需要快速边查询的稀疏图 
- 灵活性高，易于添加或删除边 
- 对于大规模稀疏图，比邻接矩阵更节省空间



### 图搜索


#### 深度优先搜索 (DFS)

```c++
function<void(int,int)> dfs = [&](int x, int y) {
    for (int i = 0; i < 4; ++i) {
        int nx = x + dir[i][0];
        int ny = y + dir[i][1];
        if (nx < 0 || ny < 0 || nx >= n || ny >= m || vis[nx][ny] || !isValid(nx, ny)) 
            continue;
        vis[nx][ny] = true;
        dfs(nx, ny);
    }
};
```


#### 广度优先搜索 (BFS)

```c++
function<void(int,int)> bfs = [&](int x, int y) {
    queue<pair<int, int>> que;
    que.push({x, y});
    vis[x][y] = true;
    
    while (!que.empty()) {
        auto [cx, cy] = que.front();
        que.pop();
        
        for (int i = 0; i < 4; ++i) {
            int nx = cx + dir[i][0];
            int ny = cy + dir[i][1];
            if (nx < 0 || ny < 0 || nx >= n || ny >= m || vis[nx][ny] || !isValid(nx, ny)) 
                continue;
            que.push({nx, ny});
            vis[nx][ny] = true;
        }
    }
};
```

```c++
int cnt = 0;
for (int x = 0; x < n; x++) {
    for (int y = 0; y < m; ++y) {
        if (!vis[x][y] && isValid(x, y)) {
            dfs(x, y);  // 或 bfs(x, y);
            ++cnt;
        }
    }
}
```

#### 边缘DFS/BFS

考虑从边缘到内部的流动或连通性

```c++
    // 5. 从边缘开始搜索
    // 第一组边界
    for (int i = 0; i < n; i++) {
        if (!visited[i][0]) dfs(i, 0);
        if (!visited[i][m-1]) dfs(i, m-1);
    }
    for (int j = 0; j < m; j++) {
        if (!visited[0][j]) dfs(0, j);
        if (!visited[n-1][j]) dfs(n-1, j);
    }
```

[太平洋大西洋水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/description/)

有一个 `m × n` 的矩形岛屿，与 **太平洋** 和 **大西洋** 相邻。 **“太平洋”** 处于大陆的左边界和上边界，而 **“大西洋”** 处于大陆的右边界和下边界。
这个岛被分割成一个由若干方形单元格组成的网格。给定一个 `m x n` 的整数矩阵 `heights` ， `heights[r][c]` 表示坐标 `(r, c)` 上单元格 **高于海平面的高度** 。
岛上雨水较多，如果相邻单元格的高度 **小于或等于** 当前单元格的高度，雨水可以直接向北、南、东、西流向相邻单元格。水可以从海洋附近的任何单元格流入海洋。
返回网格坐标 `result` 的 **2D 列表** ，其中 `result[i] = [ri, ci]` 表示雨水从单元格 `(ri, ci)` 流动 **既可流向太平洋也可流向大西洋** 。

**示例 1：**

```
输入: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
输出: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
示例 2：

输入: heights = [[2,1],[1,2]]
输出: [[0,0],[0,1],[1,0],[1,1]]
```

<img src="https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg" alt="image.png" style="zoom:60%;" />

核心代码


```c++
      for(int i=0;i<n;i++){
        dfs(i,0,pacific);
        dfs(i,m-1,atlantic);
      }

      for(int i=0;i<m;i++){
        dfs(0,i,pacific);
        dfs(n-1,i,atlantic);
      }

```


### 并查集

并查集模板

```c++
int findfather(int x)
{
    if(x=father[x]) return x;
    else//压缩路径
    {
        int f=findfather(father[x]);
        father[x]=f;
        return f;
    }
}
void Union(int a,int b)//并查集合并代码
{
    int faA=findfather(a);
    int faB=findfather(b);
    if(faA!=faB) father[faB]=faA;
}                         //numTrees                          //(numBirds)
for(int i=1;i<maxn;i++)//统计有多少个团体，每个团体多少人(cnt[i]),一共有多少人
{
    if(exist[i]==true)
    {
        int root=findfather(i);
        cnt[root]++;
    }
}
int numTrees=0,numBirds=0;
for(int i=1;i<=maxn;i++)
{
    if(exist[i]==true&&cnt[i]!=0)
    {
        numTrees++;
        numBirds+=cnt[i];
    }
}
```


[寻找图中是否存在路径](https://leetcode.cn/problems/find-if-path-exists-in-graph/)

有一个具有 `n` 个顶点的**双向**图，其中每个顶点标记从`0`到`n - 1`（包含`0`和`n - 1`）。图中的边用一个二维整数数组`edges`表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由**最多一条**边连接，并且没有顶点存在与自身相连的边。
请你确定是否存在从顶点`source`开始，到顶点 `destination` 结束的**有效路径** 。
给你数组 `edges` 和整数 `n`、`source` 和 `destination`，如果从`source`到 `destination`存在**有效路径** ，则返回 `true`，否则返回 `false` 。

```c++
class Solution {
public:

    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<int>father(n,0);
        function<int(int x)>findfather=[&](int x)->int{
            if(x==father[x]) return x;
            else{
                int f=findfather(father[x]);
                father[x]=f;
                return f;
            }
        };
        function<void(int,int)>Union=[&](int x,int y){
            int faA=findfather(x);
            int faB=findfather(y);
            if(faA!=faB) father[faB]=faA;
        };
        for(int i=0;i<n;++i) father[i]=i;

        for(auto &item:edges){
            Union(item[0],item[1]);
        }

        return findfather(source)==findfather(destination);
    }
}; 
```


### 最短路径

#### Dijkstra算法

使用 Dijkstra 算法，可以寻找图中节点之间的最短路径。特别是，可以**在图中寻找一个节点（称为“源节点”）到所有其它节点的最短路径**，生成一个最短路径树。


<img src="https://www.freecodecamp.org/news/content/images/2020/06/image-76.png" alt="image.png" style="zoom:60%;" />

初始的距离列表如下:
- 源节点到它自身的距离为0。示例中的源节点定为节点0,不过你也可
- 以选择任意其它节点作为源节点。
源节点到其它节点的距离还没有确定,所以先标记为无穷大。

<img src="https://www.freecodecamp.org/news/content/images/2020/06/image-77.png" alt="image.png" style="zoom:60%;" />
还有一个列表用来记录哪些节点未被访问（即尚未被包含在路径中）

<img src="https://www.freecodecamp.org/news/content/images/2020/06/image-78.png" alt="image.png" style="zoom:60%;" />


```c++

using namespace std;
constexpr int INF = numeric_limits<int>::max() / 2;  // 防止整数溢出的无穷大值
int n;                                  // 节点数量
int m;                                  // 边的数量
int b;                                  // 起始节点
int s;                                  // 终止节点
vector<vector<int>> G;                  // 邻接矩阵
vector<int> weight;                     // 节点权重
vector<int> num;                        // 最短路径数量
vector<int> d;                          // 到各点的最短距离
vector<int> w;                          // 到各点的总权重
vector<bool> vis;                       // 访问标记

void Dijkstra(const int start, const int end) {
    // 初始化各个vector
    w.assign(n, 0);
    w[start] = weight[start];
    num.assign(n, 0);
    num[start] = 1;
    d.assign(n, INF);
    d[start] = 0;
    vis.assign(n, false);

    for (int i = 0; i < n; i++) {
        // 找到当前未访问的最短距离节点
        int u = -1;
        int MIN = INF;
        for (int j = 0; j < n; j++) {
            if (!vis[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        }

        if (u == -1) return;
        vis[u] = true;

        // 更新相邻节点
        for (int v = 0; v < n; v++) {
            if (!vis[v] && G[u][v] != INF) {
                if (d[u] + G[u][v] < d[v]) {
                    // 找到更短路径
                    d[v] = d[u] + G[u][v];
                    num[v] = num[u];
                    w[v] = w[u] + weight[v];
                }
                else if (d[u] + G[u][v] == d[v]) {
                    // 找到相同长度的路径
                    if (w[u] + weight[v] > w[v]) {
                        w[v] = w[u] + weight[v];
                    }
                    num[v] += num[u];
                }
            }
        }
    }
}

int main() {
    ios::sync_with_stdio(false);        // 优化输入输出
    cin.tie(nullptr);

    // 输入基本信息
    cin >> n >> m >> b >> s;

    // 初始化所有vector
    G.assign(n, vector<int>(n, INF));   // n x n的邻接矩阵
    weight.resize(n);                    // 节点权重数组

    // 输入节点权重
    for (int i = 0; i < n; i++) {
        cin >> weight[i];
    }

    // 输入边的信息
    for (int i = 0; i < m; i++) {
        int u, v, c;
        cin >> u >> v >> c;
        G[u][v] = c;
        G[v][u] = c;
    }

    Dijkstra(b, s);
    cout << num[s] << ' ' << w[s];

    return 0;
}
```


#### prim算法

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241024041250.png" alt="image.png" style="zoom:60%;" />



```c++
#include <iostream>
#include <algorithm>
#include <limits>
using namespace std;

constexpr int MAXV = 1000;                      // 最大顶点数
constexpr int INF = numeric_limits<int>::max() / 2;  // 设INF为一个很大的数

int n, m;                                       // n为顶点数，m为边数
vector<vector<int>> G;                          // 邻接矩阵
vector<int> d;                                  // 顶点与集合S的最短距离
vector<bool> vis;                               // 标记数组

int prim() {    // 默认0号为初始点，函数返回最小生成树的边权之和
    d.assign(MAXV, INF);                        // 初始化d数组为INF
    d[0] = 0;                                   // 只有0号顶点到集合S的距离为0
    vis.assign(MAXV, false);                    // 初始化vis数组为false
    int ans = 0;                                // 存放最小生成树的边权之和
    
    for(int i = 0; i < n; i++) {               // 循环n次
        int u = -1;
        int MIN = INF;                          // u使d[u]最小，MIN存放该最小的d[u]
        
        for(int j = 0; j < n; j++) {           // 找到未访问的顶点中d[]最小的
            if(!vis[j] && d[j] < MIN) {
                u = j;
                MIN = d[j];
            }
        }
        
        if(u == -1) return -1;                 // 找不到小于INF的d[u]，则不连通
        
        vis[u] = true;                         // 标记u为已访问
        ans += d[u];                           // 将与集合S距离最小的边加入最小生成树
        
        for(int v = 0; v < n; v++) {
            // v未访问 && u能到达v && 以u为中介点可以使v离集合S更近
            if(!vis[v] && G[u][v] != INF && G[u][v] < d[v]) {
                d[v] = G[u][v];                // 更新d[v]
            }
        }
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);              // 优化输入输出
    cin.tie(nullptr);
    
    cin >> n >> m;                            // 输入顶点数和边数
    
    // 初始化邻接矩阵
    G.assign(MAXV, vector<int>(MAXV, INF));
    
    // 输入边的信息
    for(int i = 0; i < m; i++) {
        int u, v, w;
        cin >> u >> v >> w;                   // 输入边的两个顶点和权值
        G[u][v] = G[v][u] = w;               // 无向图
    }
    
    int ans = prim();                        // 调用prim算法
    cout << ans << '\n';
    
    return 0;
}
```

### Kruskal 

- Kruskal算法模板

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

constexpr int MAXV = 110;
constexpr int MAXE = 10010;

// 边集定义部分
struct Edge {
    int u, v;    // 边的两个端点编号
    int cost;    // 边权
};

vector<Edge> E;  // 边集数组

// 并查集部分
vector<int> father;    // 并查集数组

int findFather(int x) {   // 并查集查询函数
    int a = x;
    while(x != father[x]) {
        x = father[x];
    }
    // 路径压缩
    while(a != father[a]) {
        int z = a;
        a = father[a];
        father[z] = x;
    }
    return x;
}

// Kruskal部分，返回最小生成树的边权之和
int kruskal(int n, int m) {
    int ans = 0, Num_Edge = 0;
    
    // 并查集初始化
    father.resize(n);
    for(int i = 0; i < n; i++) {    
        father[i] = i;              
    }
    
    // 使用lambda表达式进行排序
    sort(E.begin(), E.end(), [](const Edge& a, const Edge& b) {
        return a.cost < b.cost;
    });
    
    for(int i = 0; i < m; i++) {    // 枚举所有边
        int faU = findFather(E[i].u);  // 查询测试边两个端点所在集合的根结点
        int faV = findFather(E[i].v);
        
        if(faU != faV) {            // 如果不在一个集合中
            father[faU] = faV;       // 合并集合
            ans += E[i].cost;        // 边权之和增加
            Num_Edge++;              // 当前生成树的边数加1
            if(Num_Edge == n - 1) break;  // 边数等于顶点数减1时结束
        }
    }
    
    return (Num_Edge == n - 1) ? ans : -1;  // 使用三目运算符简化返回
}

int main() {
    ios::sync_with_stdio(false);    // IO优化
    cin.tie(nullptr);
    
    int n, m;
    cin >> n >> m;    // 顶点数、边数
    
    // 预分配空间
    E.reserve(m);
    
    // 读入边的信息
    for(int i = 0; i < m; i++) {
        Edge e;
        cin >> e.u >> e.v >> e.cost;
        E.push_back(e);
    }
    
    int ans = kruskal(n, m);
    cout << ans << '\n';
    
    return 0;
}
```


### 拓扑排序
```c++
//1.将所有入度为0的边加入队列。2.取队首，删去所有相连的边，将相关顶点的入度-1，再次将所有入度为0的边加入队列，直到队空；
class TopologicalSort {
private:
    int n; // 顶点数
    std::vector<std::vector<int>> graph;
    std::vector<int> indegree;
public:
    TopologicalSort(int vertices) : n(vertices) {
        graph.resize(n);
        indegree.resize(n, 0);
    }
    void addEdge(int from, int to) {
        graph[from].push_back(to);
        indegree[to]++;
    }
    std::vector<int> sort() {
        std::vector<int> result;
        std::queue<int> q;

        // 将所有入度为0的顶点加入队列
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            result.push_back(u);

            for (int v : graph[u]) {
                if (--indegree[v] == 0) {
                    q.push(v);
                }
            }
        }
        // 如果结果中的顶点数不等于总顶点数，说明图中有环
        if (result.size() != n) {
            return {}; // 返回空vector表示存在环
        }
        return result;
    }
};
```

### dijkstra


```c++
const int INF = std::numeric_limits<int>::max();

class Graph {
private:
    int V; // 顶点数
    std::vector<std::vector<std::pair<int, int>>> adj; // 邻接表，pair<目标顶点, 权重>

public:
    Graph(int vertices) : V(vertices) {
        adj.resize(V);
    }

    void addEdge(int u, int v, int w) {
        adj[u].emplace_back(v, w);
        // 如果是无向图，还需要加上下面这行
        // adj[v].emplace_back(u, w);
    }

    std::vector<int> dijkstra(int src) {
        std::vector<int> dist(V, INF);
        std::vector<bool> visited(V, false);
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;

        dist[src] = 0;
        pq.push({0, src});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();

            if (visited[u]) continue;
            visited[u] = true;

            for (const auto& [v, weight] : adj[u]) {
                if (!visited[v] && dist[u] != INF && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.push({dist[v], v});
                }
            }
        }

        return dist;
    }
};
```



[课程表](https://leetcode.cn/problems/course-schedule/description/?envType=study-plan-v2&envId=top-100-liked)

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。
在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则**必须**先学习课程 `bi` 。
- 例如，先修课程对`[0, 1]`表示：想要学习课程 `0` ，你需要先完成课程`1` 。
请你判断是否可能完成所有课程的学习？如果可以，返回`true`；否则，返回`false` 。

示例 1：

输入：numCourses = 2, prerequisites = `[[1,0]]`
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int>indegree(numCourses,0);
        int n=prerequisites.size();
        vector<vector<int>>g(numCourses,vector<int>());
        for(int i=0;i<n;i++){
            int x=prerequisites[i][0];
            int y=prerequisites[i][1];
            indegree[y]++;
            g[x].push_back(y);
        }
        queue<int>que;

        for(int i=0;i<numCourses;i++){
            if(indegree[i]==0) que.push(i);
        }
        int ans=0;
        while(!que.empty()){
            auto cur=que.front();
            que.pop();
            for(auto it:g[cur]){
                indegree[it]--;
                if(indegree[it]==0) que.push(it);
            }
            g[cur].clear();
            ++ans;
        }
        return ans==numCourses;
    }
};
```

相似的还有[课程表2](https://leetcode.cn/problems/course-schedule-ii/description/?envType=study-plan-v2&envId=top-interview-150)

### 区域前缀和

区域前缀和数组中的每个元素 `sum[i][j]` 表示从原点(1,1)到(i,j)所形成的矩形区域内的所有元素之和（包括边界）

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241202172217.png" alt="image.png" style="zoom:60%;" />

```c++
for(int i = 1; i <= n; i++) {
    for(int j = 1; j <= m; j++) {
        sum[i][j] = sum[i-1][j] + sum[i][j-1] - sum[i-1][j-1] + arr[i-1][j-1];
    }
}
```


假设你有一个二维数组 `arr` 和对应的区域前缀和数组 `sum`，并且你想计算从 (x1, y1) 到 (x2, jy2) 的子矩阵之和，其中 (x1, y1) 是左上角坐标，(x2, y2) 是右下角坐标。

```sh
区域和 = prefix[x2+1][y2+1] - prefix[x1][y2+1] - prefix[x2+1][y1] + prefix[x1][y1]
```

```
现在有一片树林,具有n行m列树,对于第i行第j列的树,若砍掉这棵树,小明会花费a[[j]的力气。
小明打算在这片树林中选择一个正方形,对于处于这个正方形边界及内内部的所有树木全部砍掉。但是小明的力气总和为C,他想让他选择的正方形内的树在他力气耗尽前全部砍掉,且正方形的面积尽可能大。请问他能选择的正方形最大面积为多少。
输入描述
第一行三个以空格隔开的正整数n,m和C,表示树林的行数、列数和小明的力气和。
接下来有n行,每行有m个数。表示砍掉第i行第j列这棵树会花费a[j]的力气。
数字间有空格隔开。
对于100%的数据,1≤C,a[i][j]≤300000,1≤n,m≤500
输出描述
	输出可选择的正方形的最大面积
	样例输入
	3 3 4
	3 3 3
	3 1 1
	3 1 1
	样例输出
	4
```


```cpp
#include <iostream>
#include <vector>
using namespace std;

class Solution {
private:
    vector<vector<int>> prefixSum;
    int n, m, C;
    
    // 计算区域和
    inline int getAreaSum(int x1, int y1, int x2, int y2) {
        return prefixSum[x2+1][y2+1] - prefixSum[x1][y2+1] 
               - prefixSum[x2+1][y1] + prefixSum[x1][y1];
    }
    
    // 检查是否存在边长为size的可行正方形
    bool existsValidSquare(int size) {
        // 优化：如果size*size > C，直接返回false
        if (1LL * size * size > C) return false;
        
        for (int i = 0; i <= n - size; i++) {
            for (int j = 0; j <= m - size; j++) {
                if (getAreaSum(i, j, i + size - 1, j + size - 1) <= C) {
                    return true;
                }
            }
        }
        return false;
    }

public:
    int maxSquareArea() {
        // 读取输入
        cin >> n >> m >> C;
        
        // 初始化前缀和数组
        prefixSum.assign(n + 1, vector<int>(m + 1, 0));
        
        // 构建前缀和
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int x;
                cin >> x;
                prefixSum[i+1][j+1] = prefixSum[i+1][j] + prefixSum[i][j+1] 
                                     - prefixSum[i][j] + x;
            }
        }
        
        // 二分查找最大边长
        int left = 1, right = min(n, m);
        int result = 0;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (existsValidSquare(mid)) {
                result = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result * result;
    }
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    Solution solution;
    cout << solution.maxSquareArea() << endl;
    
    return 0;
}
```


## 位运算

### bitset

这里我们可以考虑用c++中的[[C++#bitset类型|bitset]]去进行做。

### Brian Kernighan算法

Brian Kernighan算法的核心思想是：
对于任何非零的无符号整数 n，表达式 `n & (n-1)` 会消除 n 的二进制表示中最右边的 1。
让我们详细解释一下这个算法：

1. 原理：
    - 当我们将一个数减1时，从最右边的1开始，所有的位都会反转。
    - 将原数与减1后的结果进行按位与操作，可以消除最右边的1，而保持其他位不变。
2. 示例：  
    假设 n = 100100 (二进制)  
    那么 n-1 = 100011  
    `n & (n-1) = 100100 & 100011 = 100000`
    可以看到，最右边的1被消除了。
3. 应用：
    - 计算一个数的二进制表示中1的个数（汉明重量）
    - 判断一个数是否是2的幂
    - 在某些情况下可以用来优化位操作算法

[191.位1的个数](https://leetcode.cn/problems/number-of-1-bits/description/?envType=study-plan-v2&envId=top-interview-150)
编写一个函数，获取一个正整数的二进制形式并返回其二进制表达式中 
设置位的个数（也被称为[汉明重量](https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E9%87%8D%E9%87%8F)）。

> **示例 1：**
> **输入：** n = 11
> **输出：** 3
> **解释：** 输入的二进制串 `1011 中，共有 3 个设置位`

```c++
#if 1 // Brian Kernighan算法
        int count = 0;
        while (n) {
            n &= (n - 1);
            count++;
        }
        return count;
#endif
```

[201.数字范围按位与](https://leetcode.cn/problems/bitwise-and-of-numbers-range/description/?envType=study-plan-v2&envId=top-interview-150)

给你两个整数 `left` 和 `right` ，表示区间 `[left, right]` ，返回此区间内所有数字 **按位与** 的结果（包含 `left` 、`right` 端点）。

> **示例 1：**
> **输入：** left = 5, right = 7
> **输出：** 4

```c++
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        while (left < right) {
            right = right & (right - 1);
        }
        return right;
    }
};
```

## 数学问题

### 快速冥

快速幂,二进制取幂(Binary Exponentiation,也称平方法),是一个在 $O(logn)$的时间内计算 a"的小技巧,而暴力的计算需要$O(n)$的时间。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241006180012.png" alt="image.png" style="zoom:60%;" />

首先我们将$n$表示为2进制，举一个例子：

$$3^{(13)}=3^{(1101)_2}=3^8*3^4*3^1$$


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20241006180310.png" alt="image.png" style="zoom:60%;" />
- 迭代版本
	```c++
	long long binpow(long long a, long long b) {
	  long long res = 1;
	  while (b > 0) {
	    if (b & 1) res = res * a;
	    a = a * a;
	    b >>= 1;
	  }
	  return res;
	}
	```


- 递归版本
	```c++
	long long binpow(long long a, long long b) {
	  if (b == 0) return 1;
	  long long res = binpow(a, b / 2);
	  if (b % 2)
	    return res * res * a;
	  else
	    return res * res;
	}
	```


# 算法模板

## 排序

### 选择排序

选择排序（selection sort）的工作原理非常简单：开启一个循环，每轮从**未排序区间**选择最小的元素，将其放到已排序区间的末尾。

设数组的长度为n,选择排序的算法流程
1. 初始状态下,所有元素未排序,即未排序(索引)区间为`[0,n-1]`。
2. 选取区间`[0,n-1]`中的最小元素,将其与索引0处的元素交换。完成后,数组前1个元素已排序。
3. 选取区间`[1,n-1]`中的最小元素,将其与索引1处的元素交换。完完成后,数组前2个元素已排序。
4. 以此类推。经过n-1轮选择与交换后,数组前n-1个元素已排序。
5. 仅剩的一个元素必定是最大元素,无须排序,因此数组排序完成。

```c++
/* 选择排序 */
void selectionSort(vector<int> &nums) {
    int n = nums.size();
    // 外循环：未排序区间为 [i, n-1]
    for (int i = 0; i < n - 1; i++) {
        // 内循环：找到未排序区间内的最小元素
        int k = i;
        for (int j = i + 1; j < n; j++) {
            if (nums[j] < nums[k])
                k = j; // 记录最小元素的索引
        }
        // 将该最小元素与未排序区间的首个元素交换
        swap(nums[i], nums[k]);
    }
}
```

- 时间复杂度为$O(n^2)$、非自适应排序
- 空间复杂度为$O(1)$、原地排序
- 非稳定排序 

### 冒泡排序

冒泡排序（bubble sort）通过连续地比较与交换相邻元素实现排序。这个过程就像气泡从底部升到顶部一样，因此得名冒泡排序


<img src="https://www.hello-algo.com/chapter_sorting/bubble_sort.assets/bubble_sort_overview.png" alt="image.png" style="zoom:60%;" />

1. 首先，对n个元素执行“冒泡”，将数组的最大元素交换至正确位置。
2. 接下来，对剩余$n-1$个元素执行“冒泡”，将第二大元素交换至正确位置。
3. 以此类推，经过$n-1$轮“冒泡”后，前$n-1$大的元素都被交换至正确位置。
4. 仅剩的一个元素必定是最小元素，无须排序，因此数组排序完成

```c++
/* 冒泡排序（标志优化）*/
void bubbleSortWithFlag(vector<int> &nums) {
    // 外循环：未排序区间为 [0, i]
    for (int i = nums.size() - 1; i > 0; i--) {
        bool flag = false; // 初始化标志位
        // 内循环：将未排序区间 [0, i] 中的最大元素交换至该区间的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交换 nums[j] 与 nums[j + 1]
                // 这里使用了 std::swap() 函数
                swap(nums[j], nums[j + 1]);
                flag = true; // 记录交换元素
            }
        }
        if (!flag)
            break; // 此轮“冒泡”未交换任何元素，直接跳出
    }
}
```

- 时间复杂度为$O(n^2)$、非自适应排序
- 空间复杂度为$O(1)$、原地排序
- 稳定排序 

### 插入排序

我们在未排序区间选择一个基准元素，将该元素与其左侧已排序区间的元素逐一比较大小，并将该元素插入到正确的位置。数组插入元素的操作流程。设基准元素为 `base` ，我们需要将从目标索引到 `base` 之间的所有元素向右移动一位，然后将 `base` 赋值给目标索引。

<img src="https://www.hello-algo.com/chapter_sorting/insertion_sort.assets/insertion_operation.png" alt="image.png" style="zoom:60%;" />

插入排序的整体流程如图所示。

1. 初始状态下，数组的第 1 个元素已完成排序。
2. 选取数组的第 2 个元素作为 `base` ，将其插入到正确位置后，**数组的前 2 个元素已排序**。
3. 选取第 3 个元素作为 `base` ，将其插入到正确位置后，**数组的前 3 个元素已排序**。
4. 以此类推，在最后一轮中，选取最后一个元素作为 `base` ，将其插入到正确位置后，**所有元素均已排序**。

<img src="https://www.hello-algo.com/chapter_sorting/insertion_sort.assets/insertion_sort_overview.png" alt="image.png" style="zoom:60%;" />

```c++
/* 插入排序 */
void insertionSort(vector<int> &nums) {
    // 外循环：已排序区间为 [0, i-1]
    for (int i = 1; i < nums.size(); i++) {
        int base = nums[i], j = i - 1;
        // 内循环：将 base 插入到已排序区间 [0, i-1] 中的正确位置
        while (j >= 0 && nums[j] > base) {
            nums[j + 1] = nums[j]; // 将 nums[j] 向右移动一位
            j--;
        }
        nums[j + 1] = base; // 将 base 赋值到正确位置
    }
}
```

- 时间复杂度为$O(n^2)$、非自适应排序
- 空间复杂度为$O(1)$、原地排序
- 非稳定排序 
### 快速排序

快速排序 quick sort 是一种基于**分治策略**的排序算法。
快速排序的核心操作是“哨兵划分”，其目标是：选择数组中的某个元素作为“**基准数**”，**将所有小于基准数的元素移到其左侧，而大于基准数的元素移到其右侧**。

1. 选取数组最左端元素作为基准数，初始化两个指针 `i` 和 `j` 分别指向数组的两端。
2. 设置一个循环，在每轮中使用 `i`（`j`）分别寻找第一个比基准数大（小）的元素，然后交换这两个元素。
3. 循环执行步骤 `2.` ，直到 `i` 和 `j` 相遇时停止，最后将基准数交换至两个子数组的分界线。


<img src="https://pdai.tech/images/alg/alg-sort-fast-1.jpg" alt="image.png" style="zoom:60%;" />


```c++
/* 哨兵划分 */
int partition(vector<int> &nums, int left, int right) {
    // 以 nums[left] 为基准数
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--; // 从右向左找首个小于基准数的元素
        while (i < j && nums[i] <= nums[left])
            i++;          // 从左向右找首个大于基准数的元素
        swap(nums[i], nums[j]); // 交换这两个元素
    }
    swap(nums[i], nums[left]); // 将基准数交换至两子数组的分界线
    return i;            // 返回基准数的索引
}
```


```c++
/* 快速排序 */
void quickSort(vector<int> &nums, int left, int right) {
    // 子数组长度为 1 时终止递归
    if (left >= right)
        return;
    // 哨兵划分
    int pivot = partition(nums, left, right);
    // 递归左子数组、右子数组
    quickSort(nums, left, pivot - 1);
    quickSort(nums, pivot + 1, right);
}
```


- **时间复杂度为 $O(nlogn)$、自适应排序**：在平均情况下，哨兵划分的递归层数为 $log ⁡n$ ，每层中的总循环数为 $n$ ，总体使用 $O(nlog⁡n)$ 时间。在最差情况下，每轮哨兵划分操作都将长度为$n$的数组划分为长度为 $0$ 和 $n−1$ 的两个子数组，此时递归层数达到 $n$，每层中的循环数为 $n$ ，总体使用 $O(n^2)$ 时间。
- **空间复杂度为 $O(n)$、$O(log n)$ 原地排序**：在输入数组完全倒序的情况下，达到最差递归深度$n$，使用 $O(n)$ 栈帧空间。排序操作是在原数组上进行的，未借助额外数组。
- **非稳定排序**：在哨兵划分的最后一步，基准数可能会被交换至相等元素的右侧。
- **快速排序在某些输入下的时间效率可能降低**。举一个极端例子，假设输入数组是完全倒序的，由于我们选择最左端元素作为基准数，那么在哨兵划分完成后，基准数被交换至数组最右端，导致左子数组长度为 −1、右子数组长度为 0 。如此递归下去，每轮哨兵划分后都有一个子数组的长度为 0 ，分治策略失效，快速排序退化为“冒泡排序”的近似形式

非递归方式通常使用栈来模拟递归过程，栈里面存放的是数组的左边界和右边界

```c++
void quicksort_nonrecursive(vector<int>& nums) {
    if (nums.size() <= 1) return;

    stack<pair<int, int>> st;
    st.push({0, nums.size() - 1});

    while (!st.empty()) {
        int beg = st.top().first;
        int end = st.top().second;
        st.pop();

        if (beg >= end) continue;

        int pivot = findpivot(nums, beg, end);

        // 先压入右半部分，再压入左半部分
        // 这样可以保证左半部分先被处理，模拟递归版本的行为
        if (pivot + 1 < end) {
            st.push({pivot + 1, end});
        }
        if (beg < pivot - 1) {
            st.push({beg, pivot - 1});
        }
    }
}
```

### 归并排序

归并排序（merge sort）是一种基于分治策略的排序算法

**划分阶段**：通过递归不断地将数组从中点处分开，将长数组的排序问题转换为短数组的排序问题。
**合并阶段**：当子数组长度为 1 时终止划分，开始合并，持续地将左右两个较短的有序数组合并为一个较长的有序数组，直至结束。

<img src="https://www.hello-algo.com/chapter_sorting/merge_sort.assets/merge_sort_overview.png" alt="image.png" style="zoom:60%;" />


算法流程
如图所示，“划分阶段”从顶至底递归地将数组从中点切分为两个子数组。

1. 计算数组中点 mid ，递归划分左子数组（区间`[left, mid]`）和右子数组（区间`[mid + 1, right]`）。
2. 递归执行步骤 1. ，直至子数组区间长度为 1 时终止。
“合并阶段”从底至顶地将左子数组和右子数组合并为一个有序数组。需要注意的是，从长度为 1 的子数组开始合并，合并阶段中的每个子数组都是有序的。

<img src="https://www.hello-algo.com/chapter_sorting/merge_sort.assets/merge_sort_step10.png" alt="image.png" style="zoom:60%;" />

归并排序的实现如以下代码所示。请注意，nums 的待合并区间为` [left, right]` ，而 tmp 的对应区间为 `[0, right - left] `

```c++
/* 合并左子数组和右子数组 */
void merge(vector<int> &nums, int left, int mid, int right) {
    // 左子数组区间为 [left, mid], 右子数组区间为 [mid+1, right]
    // 创建一个临时数组 tmp ，用于存放合并后的结果
    vector<int> tmp(right - left + 1);
    // 初始化左子数组和右子数组的起始索引
    int i = left, j = mid + 1, k = 0;
    // 当左右子数组都还有元素时，进行比较并将较小的元素复制到临时数组中
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j])
            tmp[k++] = nums[i++];
        else
            tmp[k++] = nums[j++];
    }
    // 将左子数组和右子数组的剩余元素复制到临时数组中
    while (i <= mid) {
        tmp[k++] = nums[i++];
    }
    while (j <= right) {
        tmp[k++] = nums[j++];
    }
    // 将临时数组 tmp 中的元素复制回原数组 nums 的对应区间
    for (k = 0; k < tmp.size(); k++) {
        nums[left + k] = tmp[k];
    }
}

/* 归并排序 */
void mergeSort(vector<int> &nums, int left, int right) {
    // 终止条件
    if (left >= right)
        return; // 当子数组长度为 1 时终止递归
    // 划分阶段
    int mid = (left + right) / 2;    // 计算中点
    mergeSort(nums, left, mid);      // 递归左子数组
    mergeSort(nums, mid + 1, right); // 递归右子数组
    // 合并阶段
    merge(nums, left, mid, right);
}
```

- 时间复杂度为$O(nlogn)$、非自适应排序
- 空间复杂度为$O(n)$、原地排序
- 稳定排序 


### 计数排序

一个序列具有$10^8$个数,数值范围在`[1,1000]`之间 对该序列进行排序

算法流程:
1. 将序列元素作为数组(sta)下标,用数组统计每个元素出现的次数。 `sta[i]`的值表示值为i元素的出现次数 
2. 汇总:从小到大依次输出`sta[i]`个i值

<img src="https://www.runoob.com/wp-content/uploads/2019/03/countingSort.gif" alt="image.png" style="zoom:60%;" />


```c++
void countingSort(vector<int>& arr){
	// 如果数组为空或只有一个元素，直接返回
	if (arr.size() <= 1) return;
	// 找出数组中的最大值和最小值 
	int max_val = *std::max_element(arr.begin(), arr.end()); 
	int min_val = *std::min_element(arr.begin(), arr.end());
	// 计算值域范围
	int range = max_val - min_val + 1;
	// 创建计数数组和输出数组
	std::vector<int> count(range, 0);
	std::vector<int> output(arr.size());
	// 统计每个元素出现的次数
	for (int i = 0; i < arr.size(); i++) 
	{ 
		count[arr[i] - min_val]++; 
	}
	// 计算每个元素在输出数组中的位置  这个累积计数非常重要，因为它告诉我们每个元素在最终排序数组中的位置
	for (int i=1;i<range;i++){
		count[i]+=arr[i-1];
	}
	// 构建输出数组
	for(int i=arr.size()-1;i>=0;i--){
		output[count[arr[i]-min_val]-1]=arr[i];
		count[arr[i]-min_val]--;
	}
	// 将排序结果复制回原数组

	for (int i = 0; i < arr.size(); i++) 
	{ 
		arr[i] = output[i]; 
	}
}
```

- 总时间复杂度为 O(n + k)。
### 桶排序

桶排序（Bucket Sort）是一种**分布式排序**算法，它将待排序的元素分配到**若干个桶**（Bucket）中，然后对每个桶中的元素进行排序，最后将所有桶中的元素按顺序合并。桶排序的核心思想是将数据分到有限数量的桶中，每个桶再分别排序（可以使用其他排序算法或递归地使用桶排序）。

桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个**映射函数**的确定。

**算法步骤：**

1. **初始化桶**：根据数据的范围和分布，创建若干个桶。
2. **分配元素**：遍历待排序的列表，将每个元素分配到对应的桶中。
3. **排序每个桶**：对每个桶中的元素进行排序（可以使用插入排序、快速排序等）。
4. **合并桶**：将所有桶中的元素按顺序合并，得到最终排序结果。

```
1. 什么时候最快
当输入的数据可以均匀的分配到每一个桶中。
2. 什么时候最慢
当输入的数据被分配到了同一个桶中。
```

<img src="https://www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_1.svg_.png" alt="image.png" style="zoom:60%;" />
<img src="https://www.runoob.com/wp-content/uploads/2019/03/Bucket_sort_2.svg_.png" alt="image.png" style="zoom:60%;" />
```c++
// 桶排序函数
void bucketSort(std::vector<float>& arr) {
    // 如果数组为空或只有一个元素，直接返回
    if (arr.size() <= 1) return;
    
    // 找出数组中的最大值和最小值
    float max_val = *std::max_element(arr.begin(), arr.end());
    float min_val = *std::min_element(arr.begin(), arr.end());
    
    // 确定桶的数量，这里使用数组大小作为桶的数量
    int bucket_count = arr.size();
    
    // 创建桶（使用list便于插入）
    std::vector<std::list<float>> buckets(bucket_count);
    
    // 计算每个桶的范围
    float range = (max_val - min_val) / bucket_count;
    
    // 将元素放入对应的桶中
    for (int i = 0; i < arr.size(); i++) {
        // 计算元素应该放入哪个桶
        int bucket_index = (arr[i] - min_val) / range;
        
        // 处理边界情况，最大值应该放在最后一个桶
        if (bucket_index == bucket_count) {
            bucket_index--;
        }
        
        buckets[bucket_index].push_back(arr[i]);
    }
    
    // 对每个桶中的元素进行排序
    for (int i = 0; i < bucket_count; i++) {
        buckets[i].sort(); // list自带的排序方法
    }
    
    // 合并所有桶中的元素
    int index = 0;
    for (int i = 0; i < bucket_count; i++) {
        for (float val : buckets[i]) {
            arr[index++] = val;
        }
    }
}
```

- **最佳情况**：当所有元素均匀分布在各个桶中，且每个桶的元素数量接近相等时，每个桶内排序的时间复杂度接近 O(1)，总体时间复杂度为 O(n)。
- **平均情况**：假设元素均匀分布在 n 个桶中，则每个桶包含的元素数量为 O(1)，对每个桶排序的时间复杂度也为 O(1)，总体时间复杂度为 O(n)。
- **最坏情况**：当所有元素都分配到同一个桶中时，对该桶排序的时间复杂度为 O(n log n)，总体时间复杂度退化为 O(n log n)。

### 基数排序

基数排序（Radix Sort）是一种非比较型的排序算法，它通过逐位比较元素的每一位（从最低位到最高位）来实现排序。基数排序的核心思想是将整数按位数切割成不同的数字，然后按每个位数分别进行排序。基数排序的时间复杂度为 O(n * k)，其中 n 是列表长度，k 是最大数字的位数。

**算法步骤：**

1. **确定最大位数**：找到列表中最大数字的位数，确定需要排序的轮数。
2. **按位排序**：从最低位开始，依次对每一位进行排序（通常使用计数排序或桶排序作为子排序算法）。
3. **合并结果**：每一轮排序后，更新列表的顺序，直到所有位数排序完成。

<img src="https://www.runoob.com/wp-content/uploads/2019/03/radixSort.gif" alt="image.png" style="zoom:60%;" />


```cpp
// 对数组按照指定位数进行计数排序
void countingSortByDigit(std::vector<int>& arr, int exp) {
    int n = arr.size();
    std::vector<int> output(n);
    std::vector<int> count(10, 0); // 10个可能的数字（0-9）
    
    // 统计每个数字出现的次数
    for (int i = 0; i < n; i++) {
        count[(arr[i] / exp) % 10]++;
    }
    
    // 计算累积计数，确定每个数字的位置
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }
    
    // 从后向前遍历，构建输出数组
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }
    
    // 将排序结果复制回原数组
    for (int i = 0; i < n; i++) {
        arr[i] = output[i];
    }
}

// 基数排序函数
void radixSort(std::vector<int>& arr) {
    // 如果数组为空或只有一个元素，直接返回
    if (arr.size() <= 1) return;
    
    // 找出最大值以确定最大位数
    int max_val = getMax(arr);
    
    // 从最低位开始，对每一位进行计数排序
    for (int exp = 1; max_val / exp > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}
```

## 堆

<img src="https://www.runoob.com/wp-content/uploads/2019/03/heapSort.gif" alt="image.png" style="zoom:80%;" />
### 构造堆

假设给定无序序列结构如下

<img src="https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217192038651-934327647.png" alt="image.png" style="zoom:60%;" />


我们从最后一个**非叶子结点开始**（叶结点自然不用调整，第一个非叶子结点 arr.length/2-1=5/2-1=1，也就是下面的6结点），从左至右，从下至上进行调整。

每次调整都是把结点**从上往下的调整**。针对这种向下调整, 调整方法是这样的:总是将当前结点V与它的左右孩子比较(如果有的话),假如孩子中存 在权值比结点V的权值大的,就将其中权值最大的那个孩子结点与结点V交换;交换完毕后 继续让结点V和孩子比较,直到结点V的孩子的权值都比结点V的权值小或是结点V不存 在孩子结点。

```c++
// 建堆
void creatHeap(){
	for(int i=n/2;i>=1;i--){
		downAdjust(i,n);
	}
}
```


```c++
void downAdjust(int low,int high){
	int i=low,j=i*2; // i为欲调整结点,j为其左孩子
	while(j<=high){ // 存在孩子节点
		if(j+1<=high&&heap[j+1]>heap[j]) j=j+1; // 让j存储右孩子下标
		if(heap[j]>heap[i]){
			swap(heap[j],heap[i]);
			i=j;
			j=i*2;
		}
		else {
			break;
		}
	}

}
```


### 删除堆顶
```c++
// 删除堆顶元素
void deleteTop() {
	heap[1]=heap[n--];
	downAdjust(1,n);
}
```

### 插入堆

那么,如果想要往堆里添加一个元素,应当怎么办呢?可以把想要添加的元素放在**数组最后**(也就是完全二叉树的最后一个结点后面),然后进行**向上调整操作**。向上调整总是把欲 调整结点与父亲结点比较,如果权值比父亲结点大,那么就交换其与父亲结点,这样反复比 较,直到达堆顶或是父亲结点的权值较大为止。向上调整的代码如下,时间复杂度为O(logn):

```c++
//添加元素x
void insert(int x) {
	//让元素个数加1,然后将数组末位赋值为x
	heap[++n] = x;
	//向上调整新加入的结点n
	upAdjust(1,n);
}
```


```c++
void upAdjust(int low,int hight)
{
	int i=high,j=i/2;
	while(j>=low){
		if(heap[j]<heap[i]){
			swap(heap[j],heap[i]);
			i=j;
			j=i/2;
		} else{ 
			break;
		}
	}

}
```

### 堆排序

将**堆顶元素与末尾元素进行交换**，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换。
```c++
void heapSort() {
	//建堆
	createHeap();
	//倒着枚举,直到堆中只有一个元素
	for(int i=n;i>1;i--){
		//交换heap[i]与堆顶
		swap(heap[i],heap[1]);
		//调整堆顶
		downAdjust(1,i-1);
	}
}
```


## 判断素数

```c++
1.普通方法：
bool isprime(int n)
{
    if(n<=1) return false;
    int sqr=int(sqrt(n*1.0));
    for(int i=2;i<=sqr;i++) //for(int i=2;i*i<=2;i)
        if(n%i==0) return false;
    return true;
}
2.埃氏筛法：
int prime[101],pnum=0;//prime存放所有的素数，pnum为个数
bool p[maxn]={0};//判断i是否为素数
for(int i=2;i<maxn;i++)
{
    if(p[i]==false)
    {
        prime[pnum++]=i;
        for(int j=i+i;j<maxn;j+=i) p[j]=true;
    }
}
3.求素数数组超级方法
vector<int> prime(50000, 1);
for (int i = 2; i * i < 50000; i++)
	for (int j = 2; j * i < 50000; j++)
		prime[j * i] = 0;
4.求最大公约数
int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
5.求最小公倍数 a和b的最小公倍数=a*b/gcd(a,b);
6.输出小数位数时，算精确数用1.0*（int）/除数
```

## 日期问题

- 基础循环遍历模板
```c++
	for(int year=2000;year<=2022;year++)
	for(int month=1;month<=12;month++)
	for(int day=1;day<=31;day++)
	{
		if(month == 1 || month == 3 || month == 5 || month == 7 ||
			month == 8 || month == 10 || month == 12);
		else if(month == 2)
		{
			if((year%4==0&&year%100!=0) || year % 400 == 0)
		{
			if(day > 29) break;
		}
			else
		{
			if(day > 28) break;
		}
		}
			else
		{
			if(day > 30) break;
		}
	}
```

## 差分数组

差分数组是一种处理数组区间修改查询问题的高效技术。给定一个原始数组，差分数组能够快速对原数组的某个区间内的所有元素进行加减操作，而不需要逐个遍历这些元素。这通过两步实现：构建差分数组和通过差分数组更新原数组。

对于原始数组 `a[N]`，差分数组 `d[N]` 定义如下：

- `d[i] = a[i] - a[i-1]`，对于所有 `1 < i <= N`；
- 特别地，`d[1] = a[1]`，因为没有 `a[0]`

通过差分数组更新原数组

一旦差分数组更新完成，你可以通过求差分数组的前缀和来还原修改后的原数组。即对于所有 `1 <= i <= N`：

- `s[i] = s[i-1] + d[i]`，其中 `s[0] = 0`。


```c++
#include <bits/stdc++.h>
using namespace std;
int a[100005],d[100005];
int main()
{
	int n,m;
	cin>>n>>m;
	for(int i=1;i<=n;i++) cin>>a[i];
	while(m--)
	{
		int l,r,c;
		cin>>l>>r>>c;
		d[l]+=c;
		d[r+1]-=c;
	}
	for(int i=1;i<=n;i++) d[i]+=d[i-1]; //求出当前值更改了多少
	for(int i=1;i<=n;i++) cout<<a[i]+d[i]<<" "; //直接加上更新
}
```

选择素数

```c++
1.普通方法：
bool isprime(int n)
{
    if(n<=1) return false;
    int sqr=int(sqrt(n*1.0));
    for(int i=2;i<=sqr;i++) //for(int i=2;i*i<=2;i)
        if(n%i==0) return false;
    return true;
}
2.埃氏筛法：
int prime[101],pnum=0;//prime存放所有的素数，pnum为个数
bool p[maxn]={0};//判断i是否为素数
for(int i=2;i<maxn;i++)
{
    if(p[i]==false)
    {
        prime[pnum++]=i;
        for(int j=i+i;j<maxn;j+=i) p[j]=true;
    }
}
3.求素数数组超级方法
vector<int> prime(50000, 1);
for (int i = 2; i * i < 50000; i++)
	for (int j = 2; j * i < 50000; j++)
		prime[j * i] = 0;
```

公因数公约数

```c++
4.求最大公约数
int gcd(int a,int b)
{
    return b==0?a:gcd(b,a%b);
}
5.求最小公倍数 a和b的最小公倍数=a*b/gcd(a,b);
```

b进制转换
```c++
// 315 3 1 5
vector<int> digit;
while(n!=0)
{
    digit.push_back(n%b);
    n/=b;
}
do
{
		ans[num++] = n%b;
		n /= b;

} while (n != 0);
```

并查集合并代码 

```c++
int findfather(int x)
{
    if(x==father[x]) return x;
    else//压缩路径
    {
        int f=findfather(father[x]);
        father[x]=f;
        return f;
    }
}
void Union(int a,int b)//并查集合并代码
{
    int faA=findfather(a);
    int faB=findfather(b);
    if(faA!=faB) father[faB]=faA;
}                         //numTrees                          //(numBirds)
for(int i=1;i<maxn;i++)//统计有多少个团体，每个团体多少人(cnt[i]),一共有多少人
{
    if(exist[i]==true)
    {
        int root=findfather(i);
        cnt[root]++;
    }
}
int numTrees=0,numBirds=0;
for(int i=1;i<=maxn;i++)
{
    if(exist[i]==true&&cnt[i]!=0)
    {
        numTrees++;
        numBirds+=cnt[i];
    }
}
```

## BFS


```c++
void BFS(int s){
	queue<int>q;
	q.push(s);
	while(!q.empty()){
		// 取出队首元素
		// 访问队首元素
		// 将队首元素出队
		// 将top的下一层结点中未曾入队的节点全部入队，并设置已入队
	}
}
```

```c++
void bfs(int u)//u为起点
{
	queue<node> q;
	node start;start = { u,0 };
    q.push(start);
	vis[start.v] = true;
	while (!q.empty())
	{
		node topnode = q.front();
		int u = topnode.v;
		q.pop();
		//一些对q的操作
		for(int v=0;v<n;v++)
			if (vis[v] == false && G[u][v] != INF)
			{
				node temp;temp = { v,topnode.layer + 1 };
				q.push(temp); vis[v] = true;
			}
	}
}
void bfstrave()
{
	for (int u = 0; u < n; u++)
		if (vis[u] == false)
			bfs(u);
}
```

[蛇梯棋](https://leetcode.cn/problems/snakes-and-ladders/)


## 树状数组

树状数组主要用于解决单点修改和区间查询

单点修改
```c++
int tree[MAXN];
inline void update(int i,int x){
	for(int pos=i;pos<MAXN;pos+=lowbit(pos))
		tree[pos]+=x;
}
```

求前n项和

```c++
inline int query(int n){
	int ans=0;
	for(int pos=n;pos;pos-=lowbit(pos))
		ans+=tree[pos]
	return ans;
}
```

区间查询
```c++
inline int query(int a,int b){
	return query(b)-query(a-1);
}
```



## 图的模板

1. DFS图的遍历

	```c++
	const int maxn = 1000;
	const int INF = 10000000;
	int n, G[maxn][maxn];
	bool vis[maxn] = { false };
	void dfs(int u, int depth)
	{
		vis[u] == true;
		//对u的操作
		int v;
		for (v = 0; v < n; v++)
		{
			if (vis[u] == false && G[u][v] != INF) dfs(v, depth + 1);
		}
	}
	void dfstrave()
	{
		int u;
		for(u = 0; u < n; u++)
		{
			if (vis[u] == false)
			{
				dfs(u, 1);
			}
		}
	}
	```

2. BFS图的遍历
	
	```c++
	void bfs(int u)//u为起点
	{
		queue<node> q;
		node start;start = { u,0 };
	    q.push(start);
		vis[start.v] = true;
		while (!q.empty())
		{
			node topnode = q.front();
			int u = topnode.v;
			q.pop();
			//一些对q的操作
			for(int v=0;v<n;v++)
				if (vis[v] == false && G[u][v] != INF)
				{
					node temp;temp = { v,topnode.layer + 1 };
					q.push(temp); vis[v] = true;
				}
		}
	}
	void bfstrave()
	{
		for (int u = 0; u < n; u++)
			if (vis[u] == false)
				bfs(u);
	}
	```

3. 最短路径dijkstra

	```c++
	//单Dijkstra
	int n, G[maxn][maxn], d[maxn], pre[maxn];//v的前驱
	int cost[maxn][maxn],c[maxn], num[maxn],weight[maxn],w[maxn],vnum[maxn];//边权，条数，点权,点数
	bool vis[maxn] = { false };
	void dijstra(int s)
	{
		fill(d, d + maxn, INF), fill(c, c + maxn, INF), fill(w, w + maxn, 0),         fill(num, num + maxn, 0);
		d[s] = 0, c[s] = 0, w[s] = weight[s], num[s] = 1,vnum[s]=0;
	    for (int i = 0; i < n; i++) pre[i]=i;
	    int ans=0//存放最小生成树的边权之和
		for (int i = 0; i < n; i++)
		{
			int u = -1, MIN = INF;
			for (int j = 0; j < n; j++)
			{
				if (vis[j] == false && d[j] < MIN) { u = j, MIN = d[j]; }
			}
			if (u == -1) return;
			vis[u] == true;
	        ans+=d[u];//与集合s最近的边加入
			for (int v = 0; v < n; v++)
			{
				if (vis[v] == false && G[u][v] != INF && d[u] + G[u][v] < d[v])
				{
					//d[u] + G[u][v] < d[v]，num[v]=num[u],d[u] + G[u][v]==d[v]，                  num[v]+=num[u];
					d[v] = d[u] + G[u][v];
					pre[v] = u;
				}
			}
		}
	    return ans//返回最小生成边权之和
	}
	//Dijstra+DFS
	//Dijstra区别    if (d[u] + G[u][v] < d[v])
					{
						d[v] = d[u] + G[u][v];
						pre[v].clear();
						pre[v].push_back(u);
					}
					else if (d[u] + G[u][v]==d[v])
					{
						pre[v].push_back(u);
					}
	//dfs
	void  DFS(int v)
	{
	  if(v==s) //递归边界
	  {
	      temppath.push_back(v);
	      int tempcost=0;
	      for(int i=temppath.size()-1;i>0;i--)
	      {
	          int id=temppath[i],idnext=temppath[i-1];
	          tempcost+=cost[id][idnext];//这个容易反
	      }
	      if(tempcost<mincost)
	      {
	          mincost=tempcost;
	          path=temppath;
	      }
	      temppath.pop_back();
	      return;
	
	  }
	  temppath.push_back(v);
	  for(int i=0;i<pre[v].size();i++)//递归式
	  {
	      DFS(pre[v][i]);//易错点
	  }
	  temppath.pop_back();
	}
	```

4. Floyd算法

	```c++
	int dis[maxn][maxn]；//表示i到j的距离
	void floyd()
	{
	    int i,j,v;//左边点，右边点，中介点
	    for(k=0;k<n;k++)
	        for(i=0;i<n;i++)
	            for(j=0;j<n;j++)
	           if(dis[i][k]!=INf&&dis[k][j]!=INF&&dis[i][k]+dis[k][j]<dis[i][j]) 
	               dis[i][j]=dis[i][k]+dis[k][j];
	}
	int main()
	{
	   for(i=0;i<n;i++) dis[i][i]=0;//初始化
	    floyd();
	}
	```

5. SPFA算法

	```c++
	vector<int> v[maxn];
	int n, d[maxn], num[maxn],G[maxn][maxn];//num数组记录顶点的入队次数
	bool vis[maxn];//顶点是否在队列中
	bool spfa(int s)
	{
		memset(d, INF, sizeof(d));
		memset(vis, false, sizeof(vis));
		memset(num, 0, sizeof(num);
		queue<int> q;
		q.push(s);
		vis[s] = true;
		num[s]++;//源点次数加一
		d[s] = 0;
		while (!q.empty())
		{
			int u = q.front();
			q.pop();
			vis[u] = false;//u不在队列中
			for (auto each : v[u])
			{
				if (d[u] + G[u][each] < d[each])
				{
					d[each] = d[u] + G[u][each];
					if (!vis[each])
					{
						q.push(each);
						vis[each] = true;
						num[each]++;
						if (num[each] >= n) return false;//有可达负环
					
					}
				}
			}
		}
		return true;//无可达负环
	}
	```

6. kruskal算法

	```c++
	//将所有边权排序，利用并查集找到边权最小且不在集合的边，边数加一，边数为m时退出
	int kruskal(int n,int m)
	{
	    int ans=0,num_edge=0;//ans为边权之和，num_edge为当前边数
	    //father[i]初始化
	    sort(E,E+M,cmp); //排边权
	    for(int i=0;i<m;i++)
	    {
	        int fau=findfather[];
	        int fav=...;
	        if(fau!=fav)
	        {
	            father[fau]=fav;
	            ans+=边权；
	            num_edge++;
	            if(num_edge==n-1) break;
	        }
	    }
	    if(num_edge!=n-1) return -1;
	    else return ans;
	}
	```

7. 拓扑排序

	```c++
	//1.将所有入度为0的边加入队列。2.取队首，删去所有相连的边，将相关顶点的入度-1，再次将所有入度为0的边加入队列，直到队空；
	vector<int>v[maxn];
	int n,m,indegree[maxn];
	bool topologicalsort()
	{
	    int ans=0;
	    queue<int>q;
	    for(i=0;i<n;i++)
	    {
	        if(indegree[i]==0) q.push(i);
	    }
	    while(!q.empty())
	    {
	        int u=q.front();
	        printf();
	        q.pop();
	        for(auto it:v[u])
	        {
	            indegree[it]--;
	            if(indegree[it]==0) q.push(it);
	            
	        }
	        v[u].clear();
	        ans++;
	    }
	    if(ans==n) return true;
	    else return false;
	}
	```

8. 关键路径

	```c++
	最早发生时间ve[j]=max{ve[i]+length[i->j]},最迟开始vl[i]=min[vl[j]-length[i->j]]
	struct node
	{
	   int ind=0;
	   vector<int> next;
	}node[maxn];//存储每事件的入度和相连的下一事件
	int dis[maxn][maxn];//边权
	vector<int> tporder;//存放拓扑序列
	int ve[maxn]={0},vl[maxn];
	int num=0;
	bool tpsort() //拓扑排序
	{
	      queue<int>q;
	      for(int i=0;i<n;i++)
	      {
	         if(node[i].ind==0) q.push(i);
	      }
	      while(!q.empty())
	      {
	         int top=q.front();
	         tporder.push_back(top);
	         q.pop();
	         for(auto it:Node[top].next)
	         {
	            Node[it].ind--;
	            if(node[it].ind==0) q.push(it);
	            if(ve[top]+dis[top][it]>ve[it])
	            {
	                ve[it]=ve[top]+dis[top][it];
	            }
	         }
	         num++;
	      }
	      if(num==N)
	      return true;
	      else return false;
	}
	int critcalpath()
	{
	   if(tpsort==false) return -1;
	   fill(vl,vl+n,ve[n-1]);
	   //逆拓扑
	   while(tporder.size()!=0)
	   {
	     int u=tporder[tporder.size()-1]
	     tporder.pop_back();
	     for(auto it:Node[u].next)
	     if(vl[it]-dis[u][it]<vl[u])
	     vl[u]=vl[it]-dis[u][it];
	   }
	   for(int i=0;i<n;i++)
	   {
	      for(auto it:Node[i].next)
	      {
	         if(ve[i]==vl[it]-dis[i][it])
	         printf("%d->%d\n",i,it);//i到it为关键路径
	      }
	   }
	   return ve[n-1];//返回关键路径长度，即为终点的最早开始时间
	}
	```

# c++算法技巧

## 输入输出

- 万能头文件 `#include<bits/stdc++.h>`
- cin 与 cout 是 C++ 提供的函数输入输出方便但速度较慢，所以需要 用 指 令 进 行 输入 输出 加 速 ，切记使用 加速命令后不要同时使用`cin/cout`与`scanf/printf`


```c++
include <bits/stdc++.h>
using namespace std;
int main() {
 ios::sync_with_stdio(0);
 cin.tie(0);
 cout.tie(0);
 int x, y;                          // 声明变量
 cin >> x >> y;                     // 读入 x 和 y
 cout << y << endl << x;            // 输出 y，换行，再输出 x
 return 0;                          // 结束主函数
}
```

- 有条件的输入
```c++
  while (cin >> n) { // 遇到空格或者回车进入
    if (n == 0)
      break;

    if (n % 2 == 0) {
      num_even++;
    } else {
      sum_odd += n;
    }
  }
```

- 读取一行数据用`getline`,读取之前可以用几个`cin.ignore()`忽略之前的换行符


```c++
  string s;
  int n;
  cin>>n;
  cin.ignore(); // 清除输入流中的换行符
  getline(cin,s);

  int sum=0;
  for (auto it: s) {
    if(it!=' '&&it!='\n') ++sum;
  }
```

- 字符串转int用`stoi()`，int转字符串用`to_string()`


### stringstreamt用法

1. 字符串分割

```c++
#include <sstream>

// 按空格分割字符串
string str = "hello world cpp";
stringstream ss(str);
string word;
while (ss >> word) {  // 自动按空格分割
    cout << word << endl;
}

// 按特定字符分割
string str2 = "1,2,3,4,5";
stringstream ss2(str2);
string token;
while (getline(ss2, token, ',')) {
    cout << token << endl;
}
```

2. 数字与字符串转换

```c++
// 数字转字符串
int num = 123;
stringstream ss;
ss << num;
string str = ss.str();  // "123"

// 字符串转数字
string str2 = "456";
stringstream ss2(str2);
int num2;
ss2 >> num2;  // 456

// 处理多个数字
string numbers = "123 456 789";
stringstream ss3(numbers);
int n;
vector<int> nums;
while (ss3 >> n) {
    nums.push_back(n);
}
```

3. 格式化输出
```c++
// 拼接不同类型的数据
string name = "John";
int age = 25;
double height = 175.5;

stringstream ss;
ss << "Name: " << name << ", Age: " << age << ", Height: " << height;
string result = ss.str();
```

4. 清空和重用
```c++
stringstream ss;
ss << "First";
cout << ss.str() << endl;

// 清空流
ss.str("");  // 清空内容
ss.clear();  // 清空状态标志

ss << "Second";
cout << ss.str() << endl;
```

5. 数值精度控制
```c++
double num = 3.14159;
stringstream ss;
ss << fixed << setprecision(2);
ss << num;
string str = ss.str();  // "3.14"
```

6. 进制转换
```c++
// 十进制转十六进制
int num = 255;
stringstream ss;
ss << hex << uppercase << num;
string hex_str = ss.str();  // "FF"

// 十六进制转十进制
stringstream ss2;
ss2 << hex << "FF";
int decimal;
ss2 >> decimal;  // 255
```


- sprintf是一个比较冷门的函数，但是作用十分的强大，可以直接把**相应的整数拼接组合成我们规定的格式**，这个在蓝桥杯的日期问题中也非常的好用。

	```c++
	//转换格式
	sprintf(str+1,"%d%02d%02d",year,month,day);
	```


1. 求最大值时

	```c++
	template <class ForwardIterator, class Compare>
	ForwardIterator max_element(ForwardIterator first, ForwardIterator last,
	                            Compare comp);
	```

	```c++
	#include <algorithm> // 包含max_element
	#include <iostream>
	
	int main() {
	    int f[] = {1, 3, 2, 8, 5, 7, 4, 6};
	    int n = sizeof(f) / sizeof(f[0]); // 数组的长度
	
	    // 使用max_element找到数组中的最大元素的迭代器
	    auto max_it = std::max_element(f, f + n);
	
	    // 解引用迭代器获取最大元素的值
	    std::cout << "The maximum element in array is: " << *max_it << std::endl;
	
	    return 0;
	}
	
	```

2. 求和
	```c++
	template<class InputIterator, class T>
	T accumulate(InputIterator first, InputIterator last, T init);
	```

	```c++
	#include <iostream>
	#include <numeric> // 包含accumulate
	
	int main() {
	    int arr[] = {1, 2, 3, 4, 5};
	    int n = sizeof(arr) / sizeof(arr[0]); // 数组的长度
	
	    // 使用accumulate计算数组中所有元素的总和
	    int sum = std::accumulate(arr, arr + n, 0);
	
	    std::cout << "The sum of the array elements is: " << sum << std::endl;
	
	    return 0;
	}
	
	```

3. 去重

	```c++
	#include <iostream>
	#include <vector>
	#include <algorithm>
	
	using namespace std;
	
	int main() {
	    vector<int> nums { 1, 2, 3, 4, 2, 5, 1, 6, 4 };
	    // 使用算法库中的unique函数将重复元素移到容器尾部
	    auto last = unique(nums.begin(), nums.end());
	    // 调用erase函数从容器中删除重复的元素
	    nums.erase(last, nums.end());
	    // 打印去重后的结果
	    for (int num : nums) {
	        cout << num << " ";
	    }
	    cout << endl;
	    return 0;
	}
	```


3. int的最大值和最小值
	- **无限大**：可以使用头文件 `<limits>` 中的 `std::numeric_limits<int>::max()` 来获取 `int` 类型能表示的最大值。
	- **无限小**：使用 `std::numeric_limits<int>::min()` 来获取 `int` 类型能表示的最小值。

	```c++
	/ 整型的“无限大”和“无限小”
	    int intMax = std::numeric_limits<int>::max();
	    int intMin = std::numeric_limits<int>::min();
	
	    std::cout << "Int max (无限大): " << intMax << std::endl;
	    std::cout << "Int min (无限小): " << intMin << std::endl;
	    // 浮点型的无限大 
	    double inf = std::numeric_limits<double>::infinity(); 
	    double negInf = -std::numeric_limits<double>::infinity(); 
	    std::cout << "Double infinity (无限大): " << inf << std::endl; 
	    std::cout << "Double negative infinity (负无限大): " << negInf << std::endl;
	
	```

4. 全排列
	```c++
	#include <algorithm>
	#include <vector>
	#include <iostream>
	
	int main() {
	    std::vector<int> vec = {1, 2, 3};
	    
	    do {
	        for (int num : vec) {
	            std::cout << num << " ";
	        }
	        std::cout << std::endl;
	    } while (std::next_permutation(vec.begin(), vec.end()));
	    
	    return 0;
	}
	```

