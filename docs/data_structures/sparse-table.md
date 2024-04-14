# იშვიათი მაგიდა

Sparse Table არის მონაცემთა სტრუქტურა, რომელიც საშუალებას იძლევა უპასუხოს დიაპაზონის შეკითხვებს.
მას შეუძლია უპასუხოს დიაპაზონის კითხვებს $O(\log n)$-ში, მაგრამ მისი ნამდვილი ძალა არის უპასუხოს დიაპაზონის მინიმალურ შეკითხვებს (ან ექვივალენტური დიაპაზონის მაქსიმალურ შეკითხვებს).
ამ მოთხოვნებისთვის მას შეუძლია პასუხის გამოთვლა $O(1)$ დროში.

მონაცემთა ამ სტრუქტურის ერთადერთი ნაკლი ის არის, რომ მისი გამოყენება შესაძლებელია მხოლოდ _immutable_ მასივებზე.
ეს ნიშნავს, რომ მასივი არ შეიძლება შეიცვალოს ორ მოთხოვნას შორის.
თუ მასივის რომელიმე ელემენტი იცვლება, მონაცემთა სრული სტრუქტურა ხელახლა უნდა გამოითვალოს.

## ინტუიცია

ნებისმიერი არაუარყოფითი რიცხვი შეიძლება ცალსახად იყოს წარმოდგენილი, როგორც ორის კლებადი ხარისხების ჯამი.
ეს მხოლოდ რიცხვის ორობითი წარმოდგენის ვარიანტია.
Მაგალითად. $13 = (1101)_2 = 8 + 4 + 1$.
$x$ რიცხვისთვის შეიძლება იყოს მაქსიმუმ $\lceil \log_2 x \rceil$ ჯამი.

ამავე მსჯელობით ნებისმიერი ინტერვალი შეიძლება ცალსახად იყოს წარმოდგენილი, როგორც ინტერვალების გაერთიანება სიგრძეებით, რომლებიც მცირდება ორის სიმძლავრეებით.
Მაგალითად. $[2, 14] = [2, 9] \ჭიქა [10, 13] \ჭიქა [14, 14]$, სადაც სრულ ინტერვალს აქვს სიგრძე 13 და ცალკეულ ინტერვალებს აქვთ სიგრძეები 8, 4 და 1 შესაბამისად.
და ასევე აქ კავშირი შედგება მაქსიმუმ $\lceil \log_2(\text{ინტერვალის სიგრძე}) \rceil$ მრავალი ინტერვალისგან.

Sparse Tables-ის მთავარი იდეა არის ყველა პასუხის წინასწარ გამოთვლა დიაპაზონის მოთხოვნებისთვის ორი სიგრძის სიმძლავრით.
შემდეგ დიაპაზონის სხვა შეკითხვაზე პასუხის გაცემა შესაძლებელია დიაპაზონის ორი სიგრძის დიაპაზონებად დაყოფით, წინასწარ გამოთვლილი პასუხების მოძიებით და მათი გაერთიანებით სრული პასუხის მისაღებად.

## წინასწარი გამოთვლა

წინასწარ გამოთვლილ კითხვებზე პასუხების შესანახად გამოვიყენებთ ორგანზომილებიან მასივს.
$\text{st}[i][j]$ შეინახავს პასუხს $[j, j + 2^i - 1]$ დიაპაზონისთვის $2^i$.
2-განზომილებიანი მასივის ზომა იქნება $(K + 1) \times \text{MAXN}$, სადაც $\text{MAXN}$ არის ყველაზე დიდი შესაძლო მასივის სიგრძე.
$\text{K}$ უნდა დააკმაყოფილოს $\text{K} \ge \lfloor \log_2 \text{MAXN} \rfloor$, რადგან $2^{\lfloor \log_2 \text{MAXN} \rfloor}$ არის ორი დიაპაზონის უდიდესი ძალა, რომელსაც ჩვენ უნდა დავუჭიროთ მხარი.
გონივრული სიგრძის მქონე მასივებისთვის ($\le 10^7$ ელემენტები), $K = 25$ კარგი მნიშვნელობაა.

$\text{MAXN}$ განზომილება მეორეა, რომელიც საშუალებას აძლევს (ქეშის მეგობრული) თანმიმდევრული წვდომის მეხსიერებას.

```{.cpp file=sparsetable_definition}
int st[K + 1][MAXN];
```

Because the range $[j, j + 2^i - 1]$ of length $2^i$ splits nicely into the ranges $[j, j + 2^{i - 1} - 1]$ and $[j + 2^{i - 1}, j + 2^i - 1]$, both of length $2^{i - 1}$, we can generate the table efficiently using dynamic programming:

```{.cpp file=sparsetable_generation}
std::copy(array.begin(), array.end(), st[0]);

for (int i = 1; i <= K; i++)
    for (int j = 0; j + (1 << i) <= N; j++)
        st[i][j] = f(st[i - 1][j], st[i - 1][j + (1 << (i - 1))]);
```

ფუნქცია $f$ დამოკიდებული იქნება მოთხოვნის ტიპზე.
დიაპაზონის ჯამის მოთხოვნებისთვის ის გამოთვლის ჯამს, დიაპაზონის მინიმალური მოთხოვნებისთვის ის გამოთვლის მინიმუმს.

წინასწარი გამოთვლის დროის სირთულეა $O(\text{N} \log \text{N})$.

## დიაპაზონის ჯამის მოთხოვნები

ამ ტიპის მოთხოვნებისთვის ჩვენ გვინდა ვიპოვოთ დიაპაზონის ყველა მნიშვნელობის ჯამი.
ამიტომ $f$ ფუნქციის ბუნებრივი განმარტება არის $f(x, y) = x + y$.
ჩვენ შეგვიძლია ავაშენოთ მონაცემთა სტრუქტურა:

```{.cpp file=sparsetable_sum_generation}
long long st[K + 1][MAXN];

std::copy(array.begin(), array.end(), st[0]);

for (int i = 1; i <= K; i++)
    for (int j = 0; j + (1 << i) <= N; j++)
        st[i][j] = st[i - 1][j] + st[i - 1][j + (1 << (i - 1))];
```

$[L, R]$ დიაპაზონის ჯამის შეკითხვაზე პასუხის გასაცემად, ჩვენ ვიმეორებთ ორის ყველა ძალას, დაწყებული უდიდესიდან.
როგორც კი ორი $2^i$ სიმძლავრე უფრო მცირეა ან ტოლია დიაპაზონის სიგრძეზე ($= R - L + 1$), ჩვენ ვამუშავებთ $[L, L + 2^i - 1 დიაპაზონის პირველ ნაწილს. ]$ და გააგრძელეთ დარჩენილი დიაპაზონი $[L + 2^i, R]$.

```{.cpp file=sparsetable_sum_query}
long long sum = 0;
for (int i = K; i >= 0; i--) {
    if ((1 << i) <= R - L + 1) {
        sum += st[i][L];
        L += 1 << i;
    }
}
```

დიაპაზონის ჯამის მოთხოვნის დროის სირთულე არის $O(K) = O(\log \text{MAXN})$.

## დიაპაზონის მინიმალური მოთხოვნები (RMQ)

ეს არის კითხვები, სადაც Sparse Table ანათებს.
დიაპაზონის მინიმუმის გამოთვლისას არ აქვს მნიშვნელობა დიაპაზონში მნიშვნელობას დავამუშავებთ ერთხელ თუ ორჯერ.
ამიტომ, იმის ნაცვლად, რომ დიაპაზონი გავყოთ მრავალ დიაპაზონში, ჩვენ ასევე შეგვიძლია გავყოთ დიაპაზონი მხოლოდ ორ გადახურულ დიაპაზონად ორი სიგრძის სიმძლავრით.
Მაგალითად. ჩვენ შეგვიძლია გავყოთ $[1, 6]$ დიაპაზონი $[1, 4]$ და $[3, 6]$ დიაპაზონებად.
მინიმალური დიაპაზონი $[1, 6]$ აშკარად იგივეა, რაც $[1, 4]$ დიაპაზონის მინიმალური და $[3, 6]$ დიაპაზონის მინიმალური.
ასე რომ, ჩვენ შეგვიძლია გამოვთვალოთ $[L, R]$ დიაპაზონის მინიმალური რაოდენობა:

$$\min(\text{st}[i][L], \text{st}[i][R - 2^i + 1]) \quad \text{ where } i = \log_2(R - L + 1)$$

ეს მოითხოვს, რომ ჩვენ შეგვიძლია სწრაფად გამოვთვალოთ $\log_2(R - L + 1)$.
ამის მიღწევა შეგიძლიათ ყველა ლოგარითმის წინასწარ გამოთვლით:

```{.cpp file=sparse_table_log_table}
int lg[MAXN+1];
lg[1] = 0;
for (int i = 2; i <= MAXN; i++)
    lg[i] = lg[i/2] + 1;
```
ალტერნატიულად, ჟურნალი შეიძლება გამოითვალოს მუდმივ სივრცეში და დროში:
```c++
// C++20
#include <bit>
int log2_floor(unsigned long i) {
    return std::bit_width(i) - 1;
}

// pre C++20
int log2_floor(unsigned long long i) {
    return i ? __builtin_clzll(1) - __builtin_clzll(i) : -1;
}
```
[ეს საორიენტაციო ნიშანი](https://quick-bench.com/q/Zghbdj_TEkmw4XG2nqOpD3tsJ8U) აჩვენებს, რომ `lg` მასივის გამოყენება უფრო ნელია ქეშის გამოტოვების გამო.


ამის შემდეგ ჩვენ უნდა გამოვთვალოთ Sparse Table სტრუქტურა. ამჯერად ჩვენ განვსაზღვრავთ $f$-ით $f(x, y) = \min(x, y)$.

```{.cpp file=sparse_table_minimum_generation}
int st[K + 1][MAXN];

std::copy(array.begin(), array.end(), st[0]);

for (int i = 1; i <= K; i++)
    for (int j = 0; j + (1 << i) <= N; j++)
        st[i][j] = min(st[i - 1][j], st[i - 1][j + (1 << (i - 1))]);
```

და $[L, R]$ დიაპაზონის მინიმალური გამოთვლა შესაძლებელია:

```{.cpp file=sparse_table_minimum_query}
int i = lg[R - L + 1];
int minimum = min(st[i][L], st[i][R - (1 << i) + 1]);
```

დიაპაზონის მინიმალური მოთხოვნის დროის სირთულე არის $O(1)$.

## მსგავსი მონაცემთა სტრუქტურები, რომლებიც მხარს უჭერენ მეტი ტიპის შეკითხვებს


წინა ნაწილში განხილული $O(1)$ მიდგომის ერთ-ერთი მთავარი სისუსტე არის ის, რომ ეს მიდგომა მხარს უჭერს მხოლოდ [უძლური ფუნქციების](https://en.wikipedia.org/wiki/Idempotence) შეკითხვებს. 
ე.ი. ის მშვენივრად მუშაობს დიაპაზონის მინიმალური მოთხოვნებისთვის, მაგრამ ამ მიდგომის გამოყენებით შეუძლებელია დიაპაზონის ჯამის შეკითხვებზე პასუხის გაცემა.

რსებობს მონაცემთა მსგავსი სტრუქტურები, რომლებსაც შეუძლიათ ნებისმიერი ტიპის ასოციაციური ფუნქციების მართვა და დიაპაზონის შეკითხვებზე პასუხის გაცემა $O(1)$-ში.
ერთ-ერთ მათგანს ჰქვია [Disjoint Sparse Table](https://discuss.codechef.com/questions/117696/tutorial-disjoint-sparse-table).
კიდევ ერთი იქნება [Sqrt Tree](sqrt-tree.md).

## სავარჯიშო

* [SPOJ - RMQSQ](http://www.spoj.com/problems/RMQSQ/)
* [SPOJ - THRBL](http://www.spoj.com/problems/THRBL/)
* [Codechef - MSTICK](https://www.codechef.com/problems/MSTICK)
* [Codechef - SEAD](https://www.codechef.com/problems/SEAD)
* [Codeforces - CGCDSSQ](http://codeforces.com/contest/475/problem/D)
* [Codeforces - R2D2 and Droid Army](http://codeforces.com/problemset/problem/514/D)
* [Codeforces - Maximum of Maximums of Minimums](http://codeforces.com/problemset/problem/872/B)
* [SPOJ - Miraculous](http://www.spoj.com/problems/TNVFC1M/)
* [DevSkill - Multiplication Interval (archived)](http://web.archive.org/web/20200922003506/https://devskill.com/CodingProblems/ViewProblem/19)
* [Codeforces - Animals and Puzzles](http://codeforces.com/contest/713/problem/D)
* [Codeforces - Trains and Statistics](http://codeforces.com/contest/675/problem/E)
* [SPOJ - Postering](http://www.spoj.com/problems/POSTERIN/)
* [SPOJ - Negative Score](http://www.spoj.com/problems/RPLN/)
* [SPOJ - A Famous City](http://www.spoj.com/problems/CITY2/)
* [SPOJ - Diferencija](http://www.spoj.com/problems/DIFERENC/)
* [Codeforces - Turn off the TV](http://codeforces.com/contest/863/problem/E)
* [Codeforces - Map](http://codeforces.com/contest/15/problem/D)
* [Codeforces - Awards for Contestants](http://codeforces.com/contest/873/problem/E)
* [Codeforces - Longest Regular Bracket Sequence](http://codeforces.com/contest/5/problem/C)
* [Codeforces - Array Stabilization (GCD version)](http://codeforces.com/problemset/problem/1547/F)
