# დინამიური პროგრამირების შესავალი

დინამიური პროგრამირების არსი არის განმეორებითი გაანგარიშების თავიდან აცილება. ხშირად, დინამიური პროგრამირების პრობლემები ბუნებრივად წყდება რეკურსიით. ასეთ შემთხვევებში ყველაზე ადვილია რეკურსიული ამოხსნის დაწერა, შემდეგ განმეორებითი მდგომარეობების შენახვა საძიებო ცხრილში. ეს პროცესი ცნობილია, როგორც ზემოდან ქვევით დინამიური პროგრამირება დამახსოვრებასთან ერთად. ეს იკითხება "memoization" (როგორც ჩვენ ვწერთ memo pad) და არა დამახსოვრება.

ამ პროცესის ერთ-ერთი ყველაზე ძირითადი, კლასიკური მაგალითია ფიბონაჩის მიმდევრობა. მისი რეკურსიული ფორმულირებაა $f(n) = f(n-1) + f(n-2)$ სადაც $n \ge 2$ და $f(0)=0$ და $f(1)=1$. C++-ში ეს გამოიხატება როგორც:

```cpp
int f(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return f(n - 1) + f(n - 2);
}
```

ამ რეკურსიული ფუნქციის გაშვების დრო ექსპონენციალურია - დაახლოებით $O(2^n)$, ვინაიდან ერთი ფუნქციის გამოძახება ( $f(n)$ ) იწვევს 2 მსგავსი ზომის ფუნქციის გამოძახებას ($f(n-1)$ და $f( n-2)$).

## ფიბონაჩის დაჩქარება დინამიური პროგრამირებით (მემოიზაცია)

ჩვენი რეკურსიული ფუნქცია ამჟამად ხსნის ფიბონაჩის ექსპონენციალურ დროში. ეს ნიშნავს, რომ ჩვენ შეგვიძლია მხოლოდ მცირე შეყვანის მნიშვნელობების დამუშავება, სანამ პრობლემა ძალიან გართულდება. მაგალითად, $f(29)$ იწვევს *1 მილიონზე მეტი* ფუნქციის გამოძახებას!

სიჩქარის გასაზრდელად, ჩვენ ვაღიარებთ, რომ ქვეპრობლემების რაოდენობა არის მხოლოდ $O(n)$. ანუ $f(n)$-ის გამოსათვლელად ჩვენ მხოლოდ უნდა ვიცოდეთ $f(n-1),f(n-2), \dots ,f(0)$. ამიტომ, ამ ქვეპრობლემების ხელახალი გაანგარიშების ნაცვლად, ჩვენ ვხსნით მათ ერთხელ და შემდეგ შევინახავთ შედეგს საძიებო ცხრილში. შემდგომი ზარები გამოიყენებს ამ საძიებო ცხრილს და დაუყოვნებლივ დააბრუნებს შედეგს, რაც გამორიცხავს ექსპონენციალურ მუშაობას!

თითოეული რეკურსიული ზარი შეამოწმებს საძიებო ცხრილს, რათა დაინახოს, არის თუ არა მნიშვნელობა გამოთვლილი. ეს კეთდება $O(1)$ დროში. თუ ადრე გამოვთვალეთ, დავაბრუნეთ შედეგი, წინააღმდეგ შემთხვევაში, ნორმალურად ვიანგარიშებთ ფუნქციას. მთლიანი გაშვების დროა $O(n)$! ეს არის უზარმაზარი გაუმჯობესება ჩვენი წინა ექსპონენციალური დროის ალგორითმთან შედარებით!

```cpp
const int MAXN = 100;
bool found[MAXN];
int memo[MAXN];

int f(int n) {
    if (found[n]) return memo[n];
    if (n == 0) return 0;
    if (n == 1) return 1;

    found[n] = true;
    return memo[n] = f(n - 1) + f(n - 2);
}
```

ჩვენი ახალი დამახსოვრებადი რეკურსიული ფუნქციით, $f(29)$, რომელიც იწვევდა *1 მილიონზე მეტ ზარს*, ახლა იძლევა *მხოლოდ 57* ზარს, თითქმის *20000-ჯერ* ნაკლებ ფუნქციის ზარებს! ბედის ირონიით, ახლა ჩვენ შეზღუდული ვართ ჩვენი მონაცემთა ტიპის მიხედვით. $f(46)$ არის ბოლო ფიბონაჩის რიცხვი, რომელიც შეიძლება მოერგოს ხელმოწერილ 32-ბიტიან მთელ რიცხვს.

როგორც წესი, ჩვენ ვცდილობთ შევინახოთ მდგომარეობები მასივებში, თუ ეს შესაძლებელია, რადგან ძიების დრო არის $O(1)$ მინიმალური ზედნადებით. თუმცა, უფრო ზოგადად, ჩვენ შეგვიძლია გადავარჩინოთ სახელმწიფოები, როგორც გვინდა. სხვა მაგალითები მოიცავს რუკებს (ორობითი საძიებო ხეები) ან unordered_maps (ჰეში ცხრილები).

ამის მაგალითი შეიძლება იყოს:

```cpp
unordered_map<int, int> memo;
int f(int n) {
    if (memo.count(n)) return memo[n];
    if (n == 0) return 0;
    if (n == 1) return 1;

    return memo[n] = f(n - 1) + f(n - 2);
}
```

ან ანალოგიურად:

```cpp
map<int, int> memo;
int f(int n) {
    if (memo.count(n)) return memo[n];
    if (n == 0) return 0;
    if (n == 1) return 1;

    return memo[n] = f(n - 1) + f(n - 2);
}
```

ორივე მათგანი თითქმის ყოველთვის უფრო ნელი იქნება ვიდრე მასივზე დაფუძნებული ვერსია ზოგადი მემოიზირებული რეკურსიული ფუნქციისთვის.
მდგომარეობის შენახვის ეს ალტერნატიული გზები ძირითადად გამოსადეგია ვექტორების ან სტრიქონების მდგომარეობის სივრცის ნაწილად შენახვისას.

დამახსოვრებული რეკურსიული ფუნქციის მუშაობის დროის ანალიზის ხალხური მეთოდია:

$$\text{მუშაობა ქვეპრობლემზე} * \text{ქვეპრობლემების რაოდენობა}$$

ორობითი საძიებო ხის გამოყენება (რუკა C++-ში) მდგომარეობების შესანახად ტექნიკურად გამოიწვევს $O(n \log n)$, რადგან ყოველი ძიება და ჩასმა დასჭირდება $O(\log n)$ სამუშაო და $O(n)$ უნიკალური ქვეპრობლემები გვაქვს $O(n \log n)$ დრო.

ამ მიდგომას ეწოდება ზემოდან ქვევით, რადგან ჩვენ შეგვიძლია ვუწოდოთ ფუნქციას მოთხოვნის მნიშვნელობით და გაანგარიშება იწყება ზემოდან (მოთხოვნილი მნიშვნელობა) ქვევით (რეკურსიის ძირითადი შემთხვევები) და აკეთებს მალსახმობებს დამახსოვრების გზით. .

## ქვემოდან ზევით დინამიური პროგრამირება

აქამდე თქვენ მხოლოდ ზემოდან ქვემოდან დინამიურ პროგრამირებას გინახავთ დამახსოვრების საშუალებით. თუმცა, ჩვენ შეგვიძლია ასევე გადავჭრათ პრობლემები ქვემოდან ზევით დინამიური პროგრამირებით.
ქვემოდან ზემოთ არის ზუსტად ზემოდან ქვევით საპირისპირო, თქვენ იწყებთ ქვემოდან (რეკურსიის საბაზისო შემთხვევები) და აფართოებთ მას უფრო და უფრო მეტ მნიშვნელობამდე.

ფიბონაჩის რიცხვებისთვის ქვემოდან ზევით მიდგომის შესაქმნელად, ჩვენ ვიწყებთ ბაზის შემთხვევებს მასივში. შემდეგ, ჩვენ უბრალოდ ვიყენებთ რეკურსიულ განმარტებას მასივზე:

```cpp
const int MAXN = 100;
int fib[MAXN];

int f(int n) {
    fib[0] = 0;
    fib[1] = 1;
    for (int i = 2; i <= n; i++) fib[i] = fib[i - 1] + fib[i - 2];

    return fib[n];
}
```

რა თქმა უნდა, როგორც წერია, ეს ცოტა სულელურია ორი მიზეზის გამო:
პირველ რიგში, ჩვენ ვაკეთებთ განმეორებით მუშაობას, თუ ფუნქციას არაერთხელ გამოვიძახებთ.
მეორეც, ჩვენ მხოლოდ უნდა გამოვიყენოთ ორი წინა მნიშვნელობა მიმდინარე ელემენტის გამოსათვლელად. ამიტომ, ჩვენ შეგვიძლია შევამციროთ ჩვენი მეხსიერება $O(n)$-დან $O(1)$-მდე.

ქვემოდან ზემოთ დინამიური პროგრამირების გადაწყვეტის მაგალითი ფიბონაჩისთვის, რომელიც იყენებს $O(1)$-ს, შეიძლება იყოს:

```cpp
const int MAX_SAVE = 3;
int fib[MAX_SAVE];

int f(int n) {
    fib[0] = 0;
    fib[1] = 1;
    for (int i = 2; i <= n; i++)
        fib[i % MAX_SAVE] = fib[(i - 1) % MAX_SAVE] + fib[(i - 2) % MAX_SAVE];

    return fib[n % MAX_SAVE];
}
```

გაითვალისწინეთ, რომ ჩვენ შევცვალეთ მუდმივი `MAXN`-დან `MAX_SAVE`-მდე. ეს იმიტომ ხდება, რომ ელემენტების საერთო რაოდენობა, რომლებზეც უნდა გვქონდეს წვდომა, არის მხოლოდ 3. ის აღარ სკალდება შეყვანის ზომით და, განსაზღვრებით, არის $O(1)$ მეხსიერება. გარდა ამისა, ჩვენ ვიყენებთ საერთო ხრიკს (მოდულის ოპერატორის გამოყენებით) მხოლოდ ჩვენთვის საჭირო მნიშვნელობების შესანარჩუნებლად.

Ის არის. ეს არის დინამიური პროგრამირების საფუძვლები: არ გაიმეოროთ ადრე შესრულებული სამუშაო.

დინამიურ პროგრამირებაში უკეთესი გახდომის ერთ-ერთი ხრიკი არის ზოგიერთი კლასიკური მაგალითის შესწავლა.

## კლასიკური დინამიური პროგრამირების პრობლემები
- 0-1 Knapsack
- Subset Sum
- Longest Increasing Subsequence
- Counting all possible paths from top left to bottom right corner of a matrix
- Longest Common Subsequence 
- Longest Path in a Directed Acyclic Graph (DAG)
- Coin Change
- Longest Palindromic Subsequence
- Rod Cutting
- Edit Distance
- Bitmask Dynamic Programming
- Digit Dynamic Programming
- Dynamic Programming on Trees

რა თქმა უნდა, ყველაზე მნიშვნელოვანი ხრიკი ვარჯიშია.

## სავარჯიშო
* [LeetCode - 1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/description/)
* [LeetCode - 118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/description/)
* [LeetCode - 1025. Divisor Game](https://leetcode.com/problems/divisor-game/description/)
