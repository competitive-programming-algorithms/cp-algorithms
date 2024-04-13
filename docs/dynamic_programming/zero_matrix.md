# ყველაზე დიდი ნულოვანი ქვემატრიცის პოვნა

თქვენ გეძლევათ მატრიცა `n` რიგებით და `m` სვეტებით. იპოვეთ ყველაზე დიდი ქვემატრიცა, რომელიც შედგება მხოლოდ ნულებისაგან (ქვემატრიცა არის მატრიცის მართკუთხა ფართობი).

## ალგორითმი

მატრიცის ელემენტები იქნება `a[i][j]`, სადაც `i = 0...n - 1`, `j = 0... m - 1`. სიმარტივისთვის განვიხილავთ ყველა არანულოვან ელემენტს 1-ის ტოლი.

### ნაბიჯი 1: დამხმარე დინამიკა

ირველი, ჩვენ გამოვთვალეთ შემდეგი დამხმარე მატრიცა: `d[i][j]`, უახლოესი მწკრივი, რომელსაც აქვს 1 ზემოთ `a[i][j]`. ფორმალურად რომ ვთქვათ, `d[i][j]` არის მწკრივის უდიდესი რიცხვი (`0-დან `i - 1`-მდე), რომელშიც `j`-ე სვეტში არის ელემენტი, რომელიც ტოლია `1`-ს.
ზემოდან მარცხნიდან ქვემო-მარჯვნივ გამეორებისას, როდესაც ვდგავართ მწკრივში `i`, ვიცით წინა რიგის მნიშვნელობები, ამიტომ საკმარისია მხოლოდ ელემენტების განახლება `1` მნიშვნელობით. ჩვენ შეგვიძლია შევინახოთ მნიშვნელობები მარტივ მასივში `d[i]`, `i = 1...m - 1`, რადგან შემდგომ ალგორითმში ჩვენ დავამუშავებთ მატრიცას თითო მწკრივს და გვჭირდება მხოლოდ მნიშვნელობები. მიმდინარე რიგი.

```cpp
vector<int> d(m, -1);
for (int i = 0; i < n; ++i) {
    for (int j = 0; j < m; ++j) {
        if (a[i][j] == 1) {
            d[j] = i;
        }
    }
}
```

### ნაბიჯი 2: პრობლემის გადაჭრა

ჩვენ შეგვიძლია ამოცანის გადაჭრა $O(n m^2)$-ში მწკრივებში გამეორებით, ქვემატრიცისთვის ყველა შესაძლო მარცხენა და მარჯვენა სვეტის გათვალისწინებით. მართკუთხედის ქვედა იქნება მიმდინარე მწკრივი, ხოლო `d[i][j]`-ის გამოყენებით შეგვიძლია ვიპოვოთ ზედა მწკრივი. თუმცა, შესაძლებელია უფრო შორს წასვლა და გადაწყვეტის სირთულის მნიშვნელოვნად გაუმჯობესება.

ცხადია, რომ სასურველი ნულოვანი ქვემატრიცა ოთხივე მხრიდან შემოსაზღვრულია ზოგიერთით, რაც ხელს უშლის მას ზომის გაზრდას და პასუხის გაუმჯობესებას. მაშასადამე, პასუხს არ გამოვტოვებთ, თუ ასე მოვიქცევით: `i` მწკრივის ყოველი უჯრედისთვის `j` (პოტენციური ნულოვანი ქვემატრიცის ქვედა მწკრივი) ზედა გვექნება `d[i][j]`. მიმდინარე ნულოვანი ქვემატრიცის მწკრივი. ახლა რჩება ნულოვანი ქვემატრიცის მარცხენა და მარჯვენა ოპტიმალური საზღვრების დადგენა, ანუ მაქსიმალურად გადავიტანოთ ეს ქვემატრიცა `j`-ე სვეტის მარცხნივ და მარჯვნივ.

რას ნიშნავს მაქსიმუმის მარცხნივ გადატანა? იგულისხმება `k1` ინდექსის პოვნა, რომლისთვისაც `d[i][k1] > d[i][j]`, და ამავე დროს `k1` - ყველაზე ახლოს ინდექსის მარცხნივ `j`. . გასაგებია, რომ მაშინ `k1 + 1` იძლევა საჭირო ნულოვანი ქვემატრიცის მარცხენა სვეტის რიცხვს. თუ ასეთი ინდექსი საერთოდ არ არის, მაშინ ჩადეთ `k1` = `-1` (ეს ნიშნავს, რომ ჩვენ შევძელით მიმდინარე ნულოვანი ქვემატრიცის გაფართოება მარცხნივ ბოლომდე `a` მატრიცის საზღვრამდე).

სიმეტრიულად, შეგიძლიათ განსაზღვროთ ინდექსი `k2` მარჯვენა საზღვრისთვის: ეს არის ყველაზე ახლო ინდექსი `j`-ის მარჯვნივ, რომ `d[i][k2] > d[i][j]` (ან `m `, თუ ასეთი მაჩვენებელი არ არსებობს).

ასე რომ, ინდექსები `k1` და `k2`, თუ მათ ეფექტურად ძიებას ვისწავლით, მოგვცემს ყველა საჭირო ინფორმაციას მიმდინარე ნულოვანი ქვემატრიცის შესახებ. კერძოდ, მისი ფართობი ტოლი იქნება `(i - d[i][j]) * (k2 - k1 - 1)`.

როგორ მოძებნოთ ეს ინდექსები `k1` და `k2` ეფექტურად ფიქსირებული `i` და `j`-ით? ამის გაკეთება შეგვიძლია საშუალოდ $O(1)$-ში.

ასეთი სირთულის მისაღწევად, შეგიძლიათ გამოიყენოთ სტეკი შემდეგნაირად. მოდით, ჯერ ვისწავლოთ, როგორ მოვიძიოთ ინდექსი `k1` და შევინახოთ მისი მნიშვნელობა თითოეული ინდექსისთვის `j` მიმდინარე მწკრივში `d1[i][j]` მატრიცაში. ამისათვის ჩვენ გადავხედავთ ყველა სვეტს `j` მარცხნიდან მარჯვნივ და დასტაში ვინახავთ მხოლოდ იმ სვეტებს, რომლებსაც `d[][]` მკაცრად აღემატება `d[i][j]`-ზე. . ნათელია, რომ სვეტიდან `j` შემდეგ სვეტზე გადასვლისას საჭიროა სტეკის შინაარსის განახლება. როდესაც სტეკის ზედა ნაწილში არის შეუსაბამო ელემენტი (მაგ. `d[][] <= d[i][j]`) ამოიღეთ იგი. ადვილი გასაგებია, რომ საკმარისია დასტადან ამოღება მხოლოდ მისი ზემოდან და არცერთი სხვა ადგილიდან (რადგან დასტა შეიცავს სვეტების მზარდი `d` თანმიმდევრობას).

მნიშვნელობა `d1[i][j]` თითოეული `j`-ისთვის ტოლი იქნება იმ მომენტში დასტაზე მდებარე მნიშვნელობის.

`d2[i][j]` დინამიკა `k2` ინდექსების მოსაძებნად განიხილება მსგავსი, მხოლოდ თქვენ გჭირდებათ სვეტების ნახვა მარჯვნიდან მარცხნივ.

გასაგებია, რომ რადგან თითოეულ სტრიქონზე დასტას ემატება ზუსტად `m` ცალი, არ შეიძლება მეტი წაშლაც, სირთულის ჯამი იქნება წრფივი, ამიტომ ალგორითმის საბოლოო სირთულე არის $O(nm)$.

ასევე უნდა აღინიშნოს, რომ ეს ალგორითმი მოიხმარს $O(m)$ მეხსიერებას (შეყვანის მონაცემების გარეშე - მატრიცა `a[][]`).

### განხორციელება

```cpp
int zero_matrix(vector<vector<int>> a) {
    int n = a.size();
    int m = a[0].size();

    int ans = 0;
    vector<int> d(m, -1), d1(m), d2(m);
    stack<int> st;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            if (a[i][j] == 1)
                d[j] = i;
        }

        for (int j = 0; j < m; ++j) {
            while (!st.empty() && d[st.top()] <= d[j])
                st.pop();
            d1[j] = st.empty() ? -1 : st.top();
            st.push(j);
        }
        while (!st.empty())
            st.pop();

        for (int j = m - 1; j >= 0; --j) {
            while (!st.empty() && d[st.top()] <= d[j])
                st.pop();
            d2[j] = st.empty() ? m : st.top();
            st.push(j);
        }
        while (!st.empty())
            st.pop();

        for (int j = 0; j < m; ++j)
            ans = max(ans, (i - d[j]) * (d2[j] - d1[j] - 1));
    }
    return ans;
}
```