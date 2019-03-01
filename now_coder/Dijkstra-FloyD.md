
# 题目：牛牛取快递
## 题目描述
牛牛的快递到了，他迫不及待地想去取快递，但是天气太热了，以至于牛牛不想在烈日下多走一步。他找来了你，请你帮他规划一下，他最少要走多
少距离才能取回快递。

**每个输入包含一个测试用例:**

输入的第一行包括四个正整数，表示位置个数N(2<=N<=10000)，道路条数M(1<=M<=100000)，起点位置编号S(1<=S<=N)和快递位置编号T(1<=T<=N)。位置编
号从1到N，道路是单向的。数据保证S≠T，保证至少存在一个方案可以从起点位置出发到达快递位置再返回起点位置。

接下来M行，每行包含三个正整数，表示当前道路的起始位置的编号U(1<=U<=N)，当前道路通往的位置的编号V(1<=V<=N)和当前道路的距离D(1<=D<=1000)。

**对于每个用例的输出为：** 
在单独的一行中输出从起点出发抵达快递位置再返回起点的最短。

## 解题思路：
**Dijkstra**
```Dijkstra
int Dijkstra(int Start , int End , int **arc , int len)
{
    vector<int> dis(len + 1 , INT_MAX) ;
    vector<int> sta(len + 1 , true) ;
    dis[Start] = 0 ;
    int c_i = 1 , c_j , minNum , local ;

    while (c_i ++ < len)
    {
        minNum = INT_MAX ;
        local = c_i ;
        //选择起点
        for (c_j = 1 ; c_j <= len ; c_j ++)
        {
            if (sta[c_j] && minNum > dis[c_j])
            {
                minNum = dis[c_j] ;
                local = c_j ;
            }
        }
        sta[local] = false ;
        //更新dis
        /*
        dis[local] != INT_MAX ;
        arc[local][c_j] != INT_MAX ;
        INT_MAX + INT_MAX 超过 int 表示的范围
        */
        for (c_j = 1 ; c_j <= len ; c_j ++)
        {
            if (sta[c_j] && dis[local] != INT_MAX && arc[local][c_j] != INT_MAX && dis[c_j] > dis[local] + arc[local][c_j])
                dis[c_j] = dis[local] + arc[local][c_j] ;
        }
    }//while

    return dis[End] ;
}

int TestDijkstra()
{
    int N , M , S , T ;
    int c_i , c_j , c_k ;
    int U , V , D ;
    while (cin >> N >> M >> S >> T)
    {
        int **Value = new int *[N + 1] ;
        c_i = 0 ;
        while (c_i <= N)
        {
            Value[c_i] = new int[N + 1] ;
            c_i ++ ;
        }
        for (c_i = 1 ; c_i <= N ; c_i ++)
            for (c_j = 1 ; c_j <= N ; c_j ++)
                Value[c_i][c_j] = c_i == c_j ? 0 : INT_MAX ;
        c_k = 0 ;
        while (c_k ++ < M)
        {
            cin >> U >> V >> D ;
            Value[U][V] = D ;
        }
        for (c_i = 1 ; c_i <= N ; c_i ++)
        {
            for (c_j = 1 ; c_j <= N ; c_j ++)
                cout << Value[c_i][c_j] << '\t' ;
            cout << endl ;
        }

        cout << Dijkstra(S , T , Value , N) + Dijkstra(T , S , Value , N) << endl ;

    }
}
```
## 解题思路：
**FloyD**
```FloyD
coding：
int FloyD()
{
    int N , M , S , T ;
    int c_i , c_j , c_k ;
    int U , V , D ;
    while (cin >> N >> M >> S >> T)
    {
        int Value[N + 1][N + 1] ;
        for (c_i = 1 ; c_i <= N ; c_i ++)
            for (c_j = 1 ; c_j <= N ; c_j ++)
                Value[c_i][c_j] = c_i == c_j ? 0 : INT_MAX ;
        c_k = 0 ;
        while (c_k ++ < M)
        {
            cin >> U >> V >> D ;
            Value[U][V] = D ;
        }
        for (c_k = 1 ; c_k <= N ; c_k ++)
            for (c_i = 1 ; c_i <= N ; c_i ++)
                for (c_j = 1 ; c_j <= N ; c_j ++)
                    if (Value[c_i][c_k] != INT_MAX && Value[c_k][c_j] != INT_MAX && Value[c_i][c_k] + Value[c_k][c_j] < Value[c_i][c_j] )
                        Value[c_i][c_j] = Value[c_i][c_k] + Value[c_k][c_j] ;
        cout << Value[S][T] + Value[T][S] << endl ;
    }
    return 0 ;
}
```

## 解题思路：
**正确的答案**
```
#include<iostream>
#include<vector>
#include<algorithm>
#include<climits>
#include<map>
using namespace std ;
struct ThriData
{
    int first ;
    int second ;
    int Three ;
    ThriData(int x , int y , int z): first(x) , second(y) , Three(z) {}
    bool operator < (const ThriData& cmp)const
    {
        return first < cmp.first ;
    }
};
int newDijkstraChange(int Start , int End , vector<ThriData>& arc , int len)
{
    vector<int> dis(len + 1 , INT_MAX) ;
    vector<int> sta(len + 1 , true) ;
    dis[Start] = 0 ;
    int c_i , c_j , minNum , local ;
    local = Start ;
    int tmplen = arc.size() ;
    map<int , int> connectLocal ;
    c_i = 0 ;
    while (c_i < len)
    {
        connectLocal[c_i] = tmplen ;
        c_i ++ ;
    }
    c_i = tmplen - 1 ;
    while (c_i > -1)
    {
        connectLocal[arc[c_i].first] = c_i ;
        c_i -- ;
    }
    c_i = 1;
    while (c_i ++ < len)
    {
        minNum = INT_MAX ;
        //选择起点
        for (c_j = 1 ; c_j <= len ; c_j ++)
        {
            if (sta[c_j] && minNum >= dis[c_j])
            {
                minNum = dis[c_j] ;
                local = c_j ;
            }
        }
        if (dis[local] == INT_MAX)
            break ;
        sta[local] = false ;
        //更新dis
        for (c_j = connectLocal[local] ; c_j < tmplen ; c_j ++)
        {
            if (arc[c_j].first > local)
                break ;
            if (arc[c_j].first == local && dis[arc[c_j].second] > dis[local] + arc[c_j].Three)
                dis[arc[c_j].second] = dis[local] + arc[c_j].Three ;
        }
    }//while
    return dis[End] ;
}
int main()
{
    int N , M , S , T ;
    int c_k , c_i ;
    int U , V , D ;
    vector<ThriData> Value ;
    while (cin >> N >> M >> S >> T)
    {
        c_k = 0 ;
        while (c_k ++ < M)
        {
            cin >> U >> V >> D ;
            Value.push_back(ThriData{U , V , D}) ;
        }
        sort(Value.begin() , Value.end()) ;
        int ret = 0 ;
        ret += newDijkstraChange(S , T , Value , N) ;
        ret += newDijkstraChange(T , S , Value , N) ;
        cout << ret << endl ;
        for (c_i = 0 ; c_i < Value.size() ; c_i ++)
            cout << Value[c_i].first << '\t' << Value[c_i].second << '\t' << Value[c_i].Three << endl;
        Value.clear() ;
    }
    return 0 ;
}

```

