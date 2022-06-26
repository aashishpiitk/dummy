#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#define int long long
#define ll long long
#define F(type, i, a, b, incr) for (type i = a; i <= (type)(b); i += (type)(incr))
#define RF(type, i, a, b, decr) for (type i = a; i >= (type)(b); i -= (type)(decr))
#define sz(a) sizeof(a)
#define deb(a) cerr << " [" << #a << "->" << a << "] "
#define next_line cerr << '\n'
#define all(a) a.begin(), a.end()
#define iter(it, s) for (auto it = s.begin(); it != s.end(); it++)
#define setbits(x) __builtin_popcountll(x)
using namespace __gnu_pbds;
using namespace std;

typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> indexed_set;
typedef pair<int, int> pii;
typedef pair<ll,ll> pll;

/*
1. If you cannot approach a problem
directly in the way it is proposed 
then try to think from backwards
2. Have you checked the implementation 
before starting to write the code?
3. Have you read the question carefully ?
4. Keep the names of variable descriptive.
5. Check the types and range of the values properly
6. Erase a iterator in the set or map only after using its value.
7. 1 bitset of 1e9 size only takes 256 mb.
8. Try to use global variables in case of recursion
9. Short and precise exectutes faster
*/


typedef bitset<(int)2e3 + 1> obj;
vector<int> A;

vector<obj> Tree(4e5+ 1LL);

void build(int st, int end, int node){
    if(st == end){
        Tree[node] = obj(0).set(A[st]);
        return;
    }
    int mid = (st + end) >> 1;
    build(st, mid, (node << 1));
    build(mid + 1, end, (node << 1) + 1);
    Tree[node] = Tree[(node << 1)] ^ Tree[(node << 1) + 1];
}


obj query(int st, int end, int node, int l, int r){
    if((st > r) || (end < l)){
        return obj(0);
    }
    if((l <= st) && (end <= r)){
        return Tree[node];
    }
    int mid = (st + end) >> 1;
    return query(st, mid, (node << 1), l, r) ^ query(mid + 1, end, 1 + (node << 1), l, r);
}

void update(int st, int end, int node, int ind, int new_v){
    if((st > ind) || (end < ind)){
        return;
    }
    if(st == end){
        Tree[node].flip(new_v);
        Tree[node].flip(A[ind]);
        return;
    }
    int mid = (st + end) >> 1;
    update(st, mid, node << 1, ind, new_v);
    update(mid + 1, end, (node << 1) + 1, ind, new_v);
    Tree[node ] = (Tree[(node << 1)] ^ Tree[(node << 1) + 1]); 
}

void solve()
{
    int n;
    cin >> n;
    A.resize(n);
    for(int &x: A)
        cin >> x;
    build(0, n - 1, 1);
    int q;
    cin >> q;
    while(q --){
        int x, l, r;
        cin >> x >> l >> r;
        if(x == 0){
            update(0, n - 1, 1, l, A[r]);
            update(0, n - 1, 1, r, A[l]);
            swap(A[l], A[r]);
        }else{
            if(l > r){
                cout << (query(0, n - 1, 1, l, n - 1) ^ query(0, n - 1, 1, 0, r)).count() << '\n';
            }else{
                cout << query(0, n - 1, 1, l, r).count() << '\n';
            }
        }
    }
}

signed main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    #ifndef ONLINE_JUDGE
        // freopen("input.txt", "r", stdin);
        // freopen("output.txt", "w", stdout);
        // freopen("Debug.txt", "w", stderr);
    #else
    #endif
    // cout << fixed << setprecision(10);
    int _t;
    cin>>_t;
    while(_t --)
        solve();
}
