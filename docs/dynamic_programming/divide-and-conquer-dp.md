
# Divide and Conquer DP

Divide and Conquer არის დინამიური პროგრამირების ოპტიმიზაცია.

### წინაპირობები
პროგრამირების ზოგიერთ დინამიურ პრობლემას აქვს ამ ფორმის განმეორება:

$$
dp(i, j) = \min_{0 \leq k \leq j} \\{ dp(i - 1, k - 1) + C(k, j) \\}
$$

სადაც $C(k, j)$ არის ღირებულების ფუნქცია და $dp(i, j) = 0$ როცა $j \lt 0$.

თქვით $0 \leq i \lt m$ და $0 \leq j \lt n$ და $C$-ის შეფასებას დასჭირდება $O(1)$
დრო. მაშინ ზემოაღნიშნული განმეორების პირდაპირი შეფასება არის $O(m n^2)$. იქ
არის $m \ჯერ n$ მდგომარეობები და $n$ გადასვლები თითოეული სახელმწიფოსთვის

მოდით $opt(i, j)$ იყოს $k$-ის მნიშვნელობა, რომელიც მინიმუმამდე აყენებს ზემოთ მოცემულ გამონათქვამს. ვივარაუდოთ, რომ
ხარჯის ფუნქცია აკმაყოფილებს ოთხკუთხედის უტოლობას, შეგვიძლია ვაჩვენოთ ეს
$opt(i, j) \leq opt(i, j + 1)$ ყველა $i, j$. ეს ცნობილია როგორც _ მონოტონურობის მდგომარეობა.
შემდეგ, ჩვენ შეგვიძლია გამოვიყენოთ divide and conquer DP. ოპტიმალური
"გაყოფის წერტილი" ფიქსირებული $i$-ისთვის იზრდება $j$-ის მატებასთან ერთად.

ეს საშუალებას გვაძლევს გადავჭრათ ყველა სახელმწიფოს უფრო ეფექტურად. ვთქვათ, ჩვენ გამოვთვალეთ $opt(i, j)$
ზოგიერთი ფიქსირებული $i$ და $j$. მაშინ ნებისმიერი $j' < j$-ისთვის ვიცით, რომ $opt(i, j') \leq opt(i, j)$.
ეს ნიშნავს, რომ $opt(i, j')$-ის გამოთვლისას არ უნდა გავითვალისწინოთ ამდენი
ქულების გაყოფა!

მუშაობის დროის მინიმუმამდე შესამცირებლად, ჩვენ ვიყენებთ იდეას გაყოფა და დაპყრობა. Პირველი,
გამოთვალეთ $opt(i, n / 2)$. შემდეგ, გამოთვალეთ $opt(i, n / 4)$, იმის ცოდნა, რომ ეს ნაკლებია
ვიდრე ან უდრის $opt(i, n / 2)$ და $opt(i, 3 n / 4)$ იმის ცოდნა, რომ ეს არის
$opt(i, n / 2)$-ზე მეტი ან ტოლი. რეკურსიულად თვალყურის დევნით
ქვედა და ზედა საზღვრები $opt$-ზე, ჩვენ მივაღწევთ $O(m n \log n)$ გაშვების დროს. თითოეული
$opt(i, j)$-ის შესაძლო მნიშვნელობა მხოლოდ $\log n$ სხვადასხვა კვანძებში ჩანს.

გაითვალისწინეთ, რომ არ აქვს მნიშვნელობა რამდენად "დაბალანსებულია" $opt(i, j)$. ფიქსირებულის გასწვრივ
დონეზე, $k$-ის თითოეული მნიშვნელობა გამოიყენება მაქსიმუმ ორჯერ და არის მაქსიმუმ $\log n$
დონეები.

## ზოგადი განხორციელება

მიუხედავად იმისა, რომ დანერგვა განსხვავდება პრობლემის მიხედვით, აქ არის საკმაოდ ზოგადი

ფუნქცია `compute` ითვლის $i$ მდგომარეობებს `dp_cur`, მოცემული $i-1$ მდგომარეობების წინა მწკრივი `dp_before`.
მას უნდა ეწოდოს `compute(0, n-1, 0, n-1)`. ფუნქცია `solve` ითვლის `m` რიგებს და აბრუნებს შედეგს.


```{.cpp file=divide_and_conquer_dp}
int m, n;
vector<long long> dp_before, dp_cur;

long long C(int i, int j);

// compute dp_cur[l], ... dp_cur[r] (inclusive)
void compute(int l, int r, int optl, int optr) {
    if (l > r)
        return;

    int mid = (l + r) >> 1;
    pair<long long, int> best = {LLONG_MAX, -1};

    for (int k = optl; k <= min(mid, optr); k++) {
        best = min(best, {(k ? dp_before[k - 1] : 0) + C(k, mid), k});
    }

    dp_cur[mid] = best.first;
    int opt = best.second;

    compute(l, mid - 1, optl, opt);
    compute(mid + 1, r, opt, optr);
}

long long solve() {
    dp_before.assign(n,0);
    dp_cur.assign(n,0);

    for (int i = 0; i < n; i++)
        dp_before[i] = C(0, i);

    for (int i = 1; i < m; i++) {
        compute(0, n - 1, 0, n - 1);
        dp_before = dp_cur;
    }

    return dp_before[n - 1];
}
```

### რას უნდა მიაქციოს ყურადღება

ყველაზე დიდი სირთულე Divide and Conquer DP პრობლემების დამტკიცებაა
$opt$-ის ერთფეროვნება. ერთ-ერთი განსაკუთრებული შემთხვევა, როდესაც ეს ასეა, არის, როდესაც დანახარჯების ფუნქცია აკმაყოფილებს ოთხკუთხედის უტოლობას, ანუ $C(a, c) + C(b, d) \leq C(a, d) + C(b, c)$ for ყველა $a \leq b \leq c \leq d$.
ბევრი Divide and Conquer DP პრობლემა ასევე შეიძლება მოგვარდეს ამოზნექილი ჰალი ხრიკით ან პირიქით. სასარგებლოა ცოდნა და გაგება
ორივე!

## სავარჯიშო
- [AtCoder - Yakiniku Restaurants](https://atcoder.jp/contests/arc067/tasks/arc067_d)
- [CodeForces - Ciel and Gondolas](https://codeforces.com/contest/321/problem/E) (Be careful with I/O!)
- [CodeForces - Levels And Regions](https://codeforces.com/problemset/problem/673/E)
- [CodeForces - Partition Game](https://codeforces.com/contest/1527/problem/E)
- [CodeForces - The Bakery](https://codeforces.com/problemset/problem/834/D)
- [CodeForces - Yet Another Minimization Problem](https://codeforces.com/contest/868/problem/F)
- [Codechef - CHEFAOR](https://www.codechef.com/problems/CHEFAOR)
- [CodeForces - GUARDS](https://codeforces.com/gym/103536/problem/A) (This is the exact problem in this article.)
- [Hackerrank - Guardians of the Lunatics](https://www.hackerrank.com/contests/ioi-2014-practice-contest-2/challenges/guardians-lunatics-ioi14)
- [Hackerrank - Mining](https://www.hackerrank.com/contests/world-codesprint-5/challenges/mining)
- [Kattis - Money (ACM ICPC World Finals 2017)](https://open.kattis.com/problems/money)
- [SPOJ - ADAMOLD](https://www.spoj.com/problems/ADAMOLD/)
- [SPOJ - LARMY](https://www.spoj.com/problems/LARMY/)
- [SPOJ - NKLEAVES](https://www.spoj.com/problems/NKLEAVES/)
- [Timus - Bicolored Horses](https://acm.timus.ru/problem.aspx?space=1&num=1167)
- [USACO - Circular Barn](http://www.usaco.org/index.php?page=viewproblem2&cpid=616)
- [UVA - Arranging Heaps](https://onlinejudge.org/external/125/12524.pdf)
- [UVA - Naming Babies](https://onlinejudge.org/external/125/12594.pdf)



## ცნობები
- [Quora Answer by Michael Levin](https://www.quora.com/What-is-divide-and-conquer-optimization-in-dynamic-programming)
- [Video Tutorial by "Sothe" the Algorithm Wolf](https://www.youtube.com/watch?v=wLXEWuDWnzI)
