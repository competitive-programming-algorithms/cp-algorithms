# წაშლა მონაცემთა სტრუქტურიდან $O(T(n)\log n)$-ში

დავუშვათ, რომ თქვენ გაქვთ მონაცემთა სტრუქტურა, რომელიც საშუალებას გაძლევთ დაამატოთ ელემენტები **true** $O(T(n))$-ში.
ეს სტატია აღწერს ტექნიკას, რომელიც საშუალებას იძლევა წაშლა $O(T(n)\log n)$ ოფლაინში.

## ალგორითმი

თითოეული ელემენტი ცხოვრობს მონაცემთა სტრუქტურაში დროის გარკვეულ მონაკვეთში დამატებასა და წაშლას შორის.
მოდით ავაშენოთ სეგმენტის ხე შეკითხვებზე.
თითოეული სეგმენტი, როდესაც ზოგიერთი ელემენტი ცოცხალია, იყოფა $O(\log n)$ ხის კვანძებად.
მოდით ჩავდოთ თითოეული შეკითხვა, როდესაც გვინდა ვიცოდეთ რაიმე სტრუქტურის შესახებ შესაბამის ფურცელში.
ახლა ყველა შეკითხვის დასამუშავებლად ჩვენ გავუშვით DFS სეგმენტის ხეზე.
კვანძში შესვლისას დავამატებთ ყველა ელემენტს, რომელიც არის ამ კვანძის შიგნით.
შემდეგ ჩვენ უფრო შორს წავალთ ამ კვანძის შვილებთან ან ვუპასუხებთ შეკითხვებს (თუ კვანძი ფოთოლია).
კვანძიდან გასვლისას უნდა გავაუქმოთ დამატებები.
გაითვალისწინეთ, რომ თუ ჩვენ შევცვლით $O(T(n))$-ის სტრუქტურას, შეგვიძლია დავაბრუნოთ ცვლილებები $O(T(n))$-ში ცვლილებების დატის შენახვით.
გაითვალისწინეთ, რომ უკან დაბრუნება არღვევს ამორტიზებულ სირთულეს.

## შენიშვნები

სეგმენტების ხის შექმნის იდეა სეგმენტებზე, როდესაც რაღაც ცოცხალია, შეიძლება გამოყენებულ იქნას არა მხოლოდ მონაცემთა სტრუქტურის პრობლემებისთვის.
იხილეთ რამდენიმე პრობლემა ქვემოთ.

## განხორციელება

ეს დანერგვა არის [დინამიური კავშირის](https://en.wikipedia.org/wiki/Dynamic_connectivity) პრობლემისთვის.
მას შეუძლია კიდეების დამატება, კიდეების ამოღება და დაკავშირებული კომპონენტების რაოდენობის დათვლა.

```{.cpp file=dynamic-conn}
struct dsu_save {
    int v, rnkv, u, rnku;

    dsu_save() {}

    dsu_save(int _v, int _rnkv, int _u, int _rnku)
        : v(_v), rnkv(_rnkv), u(_u), rnku(_rnku) {}
};

struct dsu_with_rollbacks {
    vector<int> p, rnk;
    int comps;
    stack<dsu_save> op;

    dsu_with_rollbacks() {}

    dsu_with_rollbacks(int n) {
        p.resize(n);
        rnk.resize(n);
        for (int i = 0; i < n; i++) {
            p[i] = i;
            rnk[i] = 0;
        }
        comps = n;
    }

    int find_set(int v) {
        return (v == p[v]) ? v : find_set(p[v]);
    }

    bool unite(int v, int u) {
        v = find_set(v);
        u = find_set(u);
        if (v == u)
            return false;
        comps--;
        if (rnk[v] > rnk[u])
            swap(v, u);
        op.push(dsu_save(v, rnk[v], u, rnk[u]));
        p[v] = u;
        if (rnk[u] == rnk[v])
            rnk[u]++;
        return true;
    }

    void rollback() {
        if (op.empty())
            return;
        dsu_save x = op.top();
        op.pop();
        comps++;
        p[x.v] = x.v;
        rnk[x.v] = x.rnkv;
        p[x.u] = x.u;
        rnk[x.u] = x.rnku;
    }
};

struct query {
    int v, u;
    bool united;
    query(int _v, int _u) : v(_v), u(_u) {
    }
};

struct QueryTree {
    vector<vector<query>> t;
    dsu_with_rollbacks dsu;
    int T;

    QueryTree() {}

    QueryTree(int _T, int n) : T(_T) {
        dsu = dsu_with_rollbacks(n);
        t.resize(4 * T + 4);
    }

    void add_to_tree(int v, int l, int r, int ul, int ur, query& q) {
        if (ul > ur)
            return;
        if (l == ul && r == ur) {
            t[v].push_back(q);
            return;
        }
        int mid = (l + r) / 2;
        add_to_tree(2 * v, l, mid, ul, min(ur, mid), q);
        add_to_tree(2 * v + 1, mid + 1, r, max(ul, mid + 1), ur, q);
    }

    void add_query(query q, int l, int r) {
        add_to_tree(1, 0, T - 1, l, r, q);
    }

    void dfs(int v, int l, int r, vector<int>& ans) {
        for (query& q : t[v]) {
            q.united = dsu.unite(q.v, q.u);
        }
        if (l == r)
            ans[l] = dsu.comps;
        else {
            int mid = (l + r) / 2;
            dfs(2 * v, l, mid, ans);
            dfs(2 * v + 1, mid + 1, r, ans);
        }
        for (query q : t[v]) {
            if (q.united)
                dsu.rollback();
        }
    }

    vector<int> solve() {
        vector<int> ans(T);
        dfs(1, 0, T - 1, ans);
        return ans;
    }
};
```

## სავარჯიშო

- [Codeforces - Connect and Disconnect](https://codeforces.com/gym/100551/problem/A)
- [Codeforces - Addition on Segments](https://codeforces.com/contest/981/problem/E)
- [Codeforces - Extending Set of Points](https://codeforces.com/contest/1140/problem/F)
