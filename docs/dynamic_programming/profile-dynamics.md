# დინამიური პროგრამირება გატეხილ პროფილზე. პრობლემა "პარკეტი"

გატეხილი პროფილზე დინამიური პროგრამირების(DP)-ის გამოყენებით გადაჭრილი საერთო პრობლემები მოიცავს:

- რამდენიმე ფიგურის (მაგ. დომინოს) ტერიტორიის სრულად შევსების გზების პოვნა (მაგ. ჭადრაკის დაფა/ბადე)
- იპოვნეთ გზა, რომ შეავსოთ ტერიტორია ფიგურების მინიმალური რაოდენობით
- ნაწილობრივი შევსების პოვნა შეუვსებელი სივრცის მინიმალური რაოდენობით (ან უჯრედები, ბადის შემთხვევაში)
- იპოვეთ ნაწილობრივი შევსება ფიგურების მინიმალური რაოდენობით, ისე, რომ მეტი ფიგურების დამატება არ შეიძლება

## პრობლემა "პარკეტი (Parquet)"

**პრობლემის აღწერა.** მოცემულია ბადე $N \ჯერ M$. იპოვეთ ბადის შევსების გზების რაოდენობა $2 \ჯერ 1$ ზომის ფიგურებით (არ უნდა დარჩეს შეუვსებელი უჯრედი და ფიგურები არ უნდა გადაფარონ ერთმანეთს).

მოდით, DP მდგომარეობა იყოს: $dp[i, ნიღაბი]$, სადაც $i = 1, \ldots N$ და $mask = 0, \ldots 2^M - 1$.

$i$ წარმოადგენს მწკრივების რაოდენობას მიმდინარე ბადეში, ხოლო $mask$ არის მიმდინარე ბადის ბოლო მწკრივის მდგომარეობა. თუ $j$-th ბიტი $mask$ არის $0$, მაშინ შესაბამისი უჯრედი ივსება, წინააღმდეგ შემთხვევაში ის შეუვსებელია.

ცხადია, პრობლემაზე პასუხი იქნება $dp[N, 0]$.

ჩვენ ავაშენებთ DP მდგომარეობას თითოეულ $i = 1-ზე, \cdots N$-ზე და ყოველ $mask = 0-ზე, \ldots 2^M - 1$-ზე გამეორებით, და ყოველი $mask$-ისთვის ჩვენ მხოლოდ წინ გადავდივართ, რომ არის, ჩვენ _დავამატებთ_ ფიგურებს მიმდინარე ბადეს.

### განხორციელება

```cpp
int n, m;
vector < vector<long long> > dp;


void calc (int x = 0, int y = 0, int mask = 0, int next_mask = 0)
{
	if (x == n)
		return;
	if (y >= m)
		dp[x+1][next_mask] += dp[x][mask];
	else
	{
		int my_mask = 1 << y;
		if (mask & my_mask)
			calc (x, y+1, mask, next_mask);
		else
		{
			calc (x, y+1, mask, next_mask | my_mask);
			if (y+1 < m && ! (mask & my_mask) && ! (mask & (my_mask << 1)))
				calc (x, y+2, mask, next_mask);
		}
	}
}


int main()
{
	cin >> n >> m;

	dp.resize (n+1, vector<long long> (1<<m));
	dp[0][0] = 1;
	for (int x=0; x<n; ++x)
		for (int mask=0; mask<(1<<m); ++mask)
			calc (x, 0, mask, 0);

	cout << dp[n][0];

}
```

## სავარჯიშო

- [UVA 10359 - Tiling](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1300)
- [UVA 10918 - Tri Tiling](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1859)
- [SPOJ GNY07H (Four Tiling)](https://www.spoj.com/problems/GNY07H/)
- [SPOJ M5TILE (Five Tiling)](https://www.spoj.com/problems/M5TILE/)
- [SPOJ MNTILE (MxN Tiling)](https://www.spoj.com/problems/MNTILE/)
- [SPOJ DOJ1](https://www.spoj.com/problems/DOJ1/)
- [SPOJ DOJ2](https://www.spoj.com/problems/DOJ2/)
- [SPOJ BTCODE_J](https://www.spoj.com/problems/BTCODE_J/)
- [SPOJ PBOARD](https://www.spoj.com/problems/PBOARD/)
- [ACM HDU 4285 - Circuits](http://acm.hdu.edu.cn/showproblem.php?pid=4285)
- [LiveArchive 4608 - Mosaic](https://vjudge.net/problem/UVALive-4608)
- [Timus 1519 - Formula 1](https://acm.timus.ru/problem.aspx?space=1&num=1519)
- [Codeforces Parquet](https://codeforces.com/problemset/problem/26/C)

## ცნობები

- [Blog by EvilBunny](https://web.archive.org/web/20180712171735/https://blog.evilbuggy.com/2018/05/broken-profile-dynamic-programming.html)
- [TopCoder Recipe by "syg96"](https://apps.topcoder.com/forums/?module=Thread&start=0&threadID=697369)
- [Blogpost by sk765](http://sk765.blogspot.com/2012/02/dynamic-programming-with-profile.html)
