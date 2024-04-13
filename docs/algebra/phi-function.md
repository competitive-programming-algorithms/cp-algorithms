# ეილერის ტოტიენტის ფუნქცია

ეილერის ტოტიენტ ფუნქცია, რომელიც ასევე ცნობილია როგორც **phi-ფუნქცია** $\phi (n)$, ითვლის მთელ რიცხვებს შორის 1-დან $n$-ის ჩათვლით, რომლებიც არის coprime $n$-მდე. ორი რიცხვი თანაპირდაპირია, თუ მათი უდიდესი საერთო გამყოფი უდრის $1$-ს ($1$ ითვლება ნებისმიერი რიცხვის თანაპრიმად).

აქ არის $\phi(n)$-ის მნიშვნელობები პირველი რამდენიმე დადებითი მთელი რიცხვისთვის:

$$\begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|}
\hline
n & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 & 13 & 14 & 15 & 16 & 17 & 18 & 19 & 20 & 21 \\\\ \hline
\phi(n) & 1 & 1 & 2 & 2 & 4 & 2 & 6 & 4 & 6 & 4 & 10 & 4 & 12 & 6 & 8 & 8 & 16 & 6 & 18 & 8 & 12 \\\\ \hline
\end{array}$$

## თვისებები

ეილერის ტოტიენტის ფუნქციის შემდეგი თვისებები საკმარისია მის გამოსათვლელად ნებისმიერი რიცხვისთვის:

  - თუ $p$ არის მარტივი რიცხვი, მაშინ $\gcd(p, q) = 1$ ყველა $1 \le q < p$. ამიტომ გვაქვს:
  
$$\phi (p) = p - 1.$$

  - თუ $p$ არის მარტივი რიცხვი და $k \ge 1$, მაშინ არის ზუსტად $p^k/p$ რიცხვები $1$-სა და $p^k$-ს შორის, რომლებიც იყოფა $p$-ზე.
    Which gives us:
    
$$\phi(p^k) = p^k - p^{k-1}.$$

  - თუ $a$ და $b$ შედარებით მარტივია, მაშინ:
    
    \[\phi(a b) = \phi(a) \cdot \phi(b).\]
    
    ეს ურთიერთობა არ არის ტრივიალური სანახავი. იგი გამომდინარეობს [Chinese remainder theorem](chinese-remainder-theorem.md). ჩინური ნარჩენების თეორემა იძლევა გარანტიას, რომ თითოეული $0 \le x < a$ და თითოეული $0 \le y < b$, არსებობს უნიკალური $0 \le z < a b$ $z \equiv x \pmod{a}$ და $z \equiv y \pmod{b}$. ძნელი არ არის იმის ჩვენება, რომ $z$ არის coprime $a b$-ზე, თუ და მხოლოდ იმ შემთხვევაში, თუ $x$ არის coprime $a$-ზე და $y$ არის coprime $b$-ზე. ამრიგად, $a b$-ზე თანადამწყები რიცხვების რაოდენობა უდრის $a$ და $b$-ის ოდენობის ნამრავლს.

  - ზოგადად, არა coprime $a$ და $b$, განტოლება

    \[\phi(ab) = \phi(a) \cdot \phi(b) \cdot \dfrac{d}{\phi(d)}\]

    $d = \gcd(a, b)$ ინახება.

ამრიგად, პირველი სამი თვისების გამოყენებით, ჩვენ შეგვიძლია გამოვთვალოთ $\phi(n)$ $n$-ის ფაქტორიზაციით ($n$-ის დაშლა მისი ძირითადი ფაქტორების ნამრავლად).

თუ $n = {p_1}^{a_1} \cdot {p_2}^{a_2} \cdots {p_k}^{a_k}$, სადაც $p_i$ არის $n$-ის ძირითადი ფაქტორები,

$$\begin{align}
\phi (n) &= \phi ({p_1}^{a_1}) \cdot \phi ({p_2}^{a_2}) \cdots  \phi ({p_k}^{a_k}) \\\\
&= \left({p_1}^{a_1} - {p_1}^{a_1 - 1}\right) \cdot \left({p_2}^{a_2} - {p_2}^{a_2 - 1}\right) \cdots \left({p_k}^{a_k} - {p_k}^{a_k - 1}\right) \\\\
&= p_1^{a_1} \cdot \left(1 - \frac{1}{p_1}\right) \cdot p_2^{a_2} \cdot \left(1 - \frac{1}{p_2}\right) \cdots p_k^{a_k} \cdot \left(1 - \frac{1}{p_k}\right) \\\\
&= n \cdot \left(1 - \frac{1}{p_1}\right) \cdot \left(1 - \frac{1}{p_2}\right) \cdots \left(1 - \frac{1}{p_k}\right)
\end{align}$$

## განხორციელება

აქ არის განხორციელება $O(\sqrt{n})$-ში ფაქტორიზაციის გამოყენებით:

```cpp
int phi(int n) {
    int result = n;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0)
                n /= i;
            result -= result / i;
        }
    }
    if (n > 1)
        result -= result / n;
    return result;
}
```

## ეილერის ტოტიენტის ფუნქცია $1$-დან $n$-მდე $O(n \log\log{n})$-ში 

თუ ჩვენ გვჭირდება ყველა რიცხვის მთელი ტოტიენტი $1$-დან $n$-მდე, მაშინ $n$-ის ყველა რიცხვის ფაქტორიზაცია არ არის ეფექტური.
ჩვენ შეგვიძლია გამოვიყენოთ იგივე იდეა, რაც [ერატოსთენეს საცერი](sieve-of-eratosthenes.md).
ის მაინც დაფუძნებულია ზემოთ ნაჩვენებ თვისებებზე, მაგრამ იმის ნაცვლად, რომ განაახლოთ დროებითი შედეგი თითოეული მარტივი ფაქტორისთვის თითოეული რიცხვისთვის, ჩვენ ვპოულობთ ყველა მარტივ რიცხვს და თითოეულისთვის ვაახლებთ ყველა რიცხვის დროებით შედეგებს, რომლებიც იყოფა ამ მარტივ რიცხვზე.

ვინაიდან ეს მიდგომა ძირითადად ერატოსთენეს საცრის იდენტურია, სირთულეც იგივე იქნება: $O(n \log \log n)$

```cpp
void phi_1_to_n(int n) {
    vector<int> phi(n + 1);
    for (int i = 0; i <= n; i++)
        phi[i] = i;
    
    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {
            for (int j = i; j <= n; j += i)
                phi[j] -= phi[j] / i;
        }
    }
}
```


## გამყოფი ჯამის თვისება { #divsum}

ეს საინტერესო ქონება დააარსა გაუსმა:

$$ \sum_{d|n} \phi{(d)} = n$$

აქ ჯამი არის ყველა დადებითი გამყოფი $d$ $n$-ზე.

მაგალითად, 10-ის გამყოფებია 1, 2, 5 და 10.
აქედან გამომდინარე $\phi{(1)} + \phi{(2)} + \phi{(5)} + \phi{(10)} = 1 + 1 + 4 + 4 = 10$.

### ტოტიენტის პოვნა 1-დან $n$-მდე გამყოფი ჯამის თვისების გამოყენებით

გამყოფი ჯამის თვისება ასევე გვაძლევს საშუალებას გამოვთვალოთ ყველა რიცხვის ტოტიენტი 1-დან $n$-მდე.
ეს განხორციელება ოდნავ უფრო მარტივია, ვიდრე წინა განხორციელება, რომელიც დაფუძნებულია ერატოსთენეს საცერზე, თუმცა ასევე აქვს ოდნავ უარესი სირთულე: $O(n \log n)$

```cpp
void phi_1_to_n(int n) {
    vector<int> phi(n + 1);
    phi[0] = 0;
    phi[1] = 1;
    for (int i = 2; i <= n; i++)
        phi[i] = i - 1;
    
    for (int i = 2; i <= n; i++)
        for (int j = 2 * i; j <= n; j += i)
              phi[j] -= phi[i];
}
```

## გამოყენება ეილერის თეორემაში { #application }

ეილერის ტოტიენტის ფუნქციის ყველაზე ცნობილი და მნიშვნელოვანი თვისება გამოიხატება **ეილერის თეორემაში**:

$$a^{\phi(m)} \equiv 1 \pmod m \quad \text{if } a \text{ და } m \text{ შედარებით მარტივია.}$$

კონკრეტულ შემთხვევაში, როდესაც $m$ არის მარტივი, ეილერის თეორემა გადაიქცევა **ფერმატის პატარა თეორემაში**:

$$a^{m - 1} \equiv 1 \pmod m$$

ეილერის თეორემა და ეილერის ტოტიენტის ფუნქცია საკმაოდ ხშირად გვხვდება პრაქტიკულ პროგრამებში, მაგალითად, ორივე გამოიყენება [მოდულური გამრავლების ინვერსიის](module-inverse.md) გამოსათვლელად.

როგორც უშუალო შედეგი, ჩვენ ასევე ვიღებთ ეკვივალენტობას:

$$a^n \equiv a^{n \bmod \phi(m)} \pmod m$$

ეს საშუალებას იძლევა გამოვთვალოთ $x^n \bmod m$ ძალიან დიდი $n$-ისთვის, განსაკუთრებით თუ $n$ არის სხვა გამოთვლის შედეგი, რადგან ის იძლევა $n$-ის გამოთვლას მოდულის ქვეშ.

### ჯგუფის თეორია
$\phi(n)$ არის [მამრავლებელი ჯგუფის mod n-ის რიგითობა](https://en.wikipedia.org/wiki/Multiplicative_group_of_integers_modulo_n) $(\mathbb Z / n\mathbb Z)^\ჯერ$, რომ არის ერთეულების ჯგუფი (ელემენტები მრავლობითი შებრუნებით). მრავლობითი ინვერსიის მქონე ელემენტები ზუსტად არის $n$-მდე თანდაყოლილი ელემენტები.

$a$ mod $n$ ელემენტის [გამრავლების თანმიმდევრობა](https://en.wikipedia.org/wiki/Multiplicative_order), რომელიც აღინიშნება $\operatorname{ord}_n(a)$, არის ყველაზე პატარა $k>. 0$ ისეთი, რომ $a^k \equiv 1 \pmod m$. $\operatorname{ord}_n(a)$ არის $a$-ის მიერ გენერირებული ქვეჯგუფის ზომა, ასე რომ, ლაგრანგის თეორემის მიხედვით, ნებისმიერი $a$-ის გამრავლების თანმიმდევრობა უნდა გაყოს $\phi(n)$. თუ $a$-ის გამრავლების რიგი არის $\phi(n)$, ყველაზე დიდი, მაშინ $a$ არის [პრიმიტიული ფესვი](primitive-root.md) და ჯგუფი განსაზღვრებით ციკლურია.

## განზოგადება

არსებობს ბოლო ეკვივალენტობის ნაკლებად ცნობილი ვერსია, რომელიც იძლევა $x^n \bmod m$-ის ეფექტურად გამოთვლას $x$-ისა და $m$-ის გარეშე.
თვითნებური $x, m$ და $n \geq \log_2 m$:

$$x^{n}\equiv x^{\phi(m)+[n \bmod \phi(m)]} \mod m$$

მტკიცებულება:

მოდით $p_1, \dots, p_t$ იყოს $x$-ისა და $m$-ის საერთო მარტივი გამყოფები, ხოლო $k_i$ მათი მაჩვენებლები $m$-ში.
მათთან ერთად ჩვენ განვსაზღვრავთ $a = p_1^{k_1} \dots p_t^{k_t}$, რაც $\frac{m}{a}$-ს აქცევს $x$-მდე.
და მოდით $k$ იყოს უმცირესი რიცხვი ისეთი, რომ $a$ გაყოს $x^k$.
თუ დავუშვებთ $n \ge k$, შეგვიძლია დავწეროთ:

$$\begin{align}x^n \bmod m &= \frac{x^k}{a}ax^{n-k}\bmod m \\
&= \frac{x^k}{a}\left(ax^{n-k}\bmod m\right) \bmod m \\
&= \frac{x^k}{a}\left(ax^{n-k}\bmod a \frac{m}{a}\right) \bmod m \\
&=\frac{x^k}{a} a \left(x^{n-k} \bmod \frac{m}{a}\right)\bmod m \\
&= x^k\left(x^{n-k} \bmod \frac{m}{a}\right)\bmod m
\end{align}$$

ეკვივალენტობა მესამე და მეოთხე ხაზებს შორის გამომდინარეობს იქიდან, რომ $ab \bmod ac = a(b \bmod c)$.
მართლაც, თუ $b = cd + r$ $r <c$-ით, მაშინ $ab = acd + ar$ $ar < ac$-ით.

ვინაიდან $x$ და $\frac{m}{a}$ არის თანაპრიმიტეტები, შეგვიძლია გამოვიყენოთ ეილერის თეორემა და მივიღოთ ეფექტური (რადგან $k$ ძალიან მცირეა; სინამდვილეში $k \le \log_2 m$) ფორმულა:

$$x^n \bmod m = x^k\left(x^{n-k \bmod \phi(\frac{m}{a})} \bmod \frac{m}{a}\right)\bmod m.$$

ამ ფორმულის გამოყენება რთულია, მაგრამ ჩვენ შეგვიძლია გამოვიყენოთ $x^n \bmod m$-ის ქცევის გასაანალიზებლად. ჩვენ ვხედავთ, რომ $(x^1 \bmod m, x^2 \bmod m, x^3 \bmod m, \dots)$ სიგრძის ციკლში შედის $\phi\left(\frac{m) {a}\right)$ პირველი $k$ (ან ნაკლები) ელემენტების შემდეგ.
$\phi\left(\frac{m}{a}\right)$ ყოფს $\phi(m)$-ს (რადგან $a$ და $\frac{m}{a}$ არის coprime, გვაქვს $\phi( ა) \cdot \phi\left(\frac{m}{a}\right) = \phi(m)$), ამიტომ შეგვიძლია ვთქვათ, რომ პერიოდს აქვს სიგრძე $\phi(m)$.
და რადგან $\phi(m) \ge \log_2 m \ge k$, შეგვიძლია დავასკვნათ სასურველი, ბევრად უფრო მარტივი ფორმულა:

$$ x^n \equiv x^{\phi(m)} x^{(n - \phi(m)) \bmod \phi(m)} \bmod m \equiv x^{\phi(m)+[n \bmod \phi(m)]} \mod m.$$

## სავარჯიშო

* [SPOJ #4141 "Euler Totient Function" [Difficulty: CakeWalk]](http://www.spoj.com/problems/ETF/)
* [UVA #10179 "Irreducible Basic Fractions" [Difficulty: Easy]](http://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1120)
* [UVA #10299 "Relatives" [Difficulty: Easy]](http://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1240)
* [UVA #11327 "Enumerating Rational Numbers" [Difficulty: Medium]](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2302)
* [TIMUS #1673 "Admission to Exam" [Difficulty: High]](http://acm.timus.ru/problem.aspx?space=1&num=1673)
* [UVA 10990 - Another New Function](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1931)
* [Codechef - Golu and Sweetness](https://www.codechef.com/problems/COZIE)
* [SPOJ - LCM Sum](http://www.spoj.com/problems/LCMSUM/)
* [GYM - Simple Calculations  (F)](http://codeforces.com/gym/100975)
* [UVA 13132 - Laser Mirrors](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=5043)
* [SPOJ - GCDEX](http://www.spoj.com/problems/GCDEX/)
* [UVA 12995 - Farey Sequence](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4878)
* [SPOJ - Totient in Permutation (easy)](http://www.spoj.com/problems/TIP1/)
* [LOJ - Mathematically Hard](http://lightoj.com/volume_showproblem.php?problem=1007)
* [SPOJ - Totient Extreme](http://www.spoj.com/problems/DCEPCA03/)
* [SPOJ - Playing with GCD](http://www.spoj.com/problems/NAJPWG/)
* [SPOJ - G Force](http://www.spoj.com/problems/DCEPC12G/)
* [SPOJ - Smallest Inverse Euler Totient Function](http://www.spoj.com/problems/INVPHI/)
* [Codeforces - Power Tower](http://codeforces.com/problemset/problem/906/D)
* [Kattis - Exponial](https://open.kattis.com/problems/exponial)
* [LeetCode - 372. Super Pow](https://leetcode.com/problems/super-pow/)
* [Codeforces - The Holmes Children](http://codeforces.com/problemset/problem/776/E)
* [Codeforces - Small GCD](https://codeforces.com/contest/1900/problem/D)
