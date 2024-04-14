# Treap (დეკარტის ხე)

Treap არის მონაცემთა სტრუქტურა, რომელიც აერთიანებს ორობით ხეს და ორობით გროვას (აქედან გამომდინარე, სახელწოდება: ხე + გროვა $\Rightarrow$ Treap).

უფრო კონკრეტულად, treap არის მონაცემთა სტრუქტურა, რომელიც ინახავს $(X, Y)$ წყვილებს ბინარულ ხეში ისე, რომ ეს არის ორობითი საძიებო ხე $X$-ით და ბინარული გროვა $Y$-ით.
თუ ხის ზოგიერთი კვანძი შეიცავს $(X_0, Y_0)$ მნიშვნელობებს, მარცხენა ქვეხის ყველა კვანძს აქვს $X \leq X_0$, მარჯვენა ქვეხის ყველა კვანძს აქვს $X_0 \leq X$ და ყველა კვანძს ორივე მარცხნივ. და მარჯვენა ქვეხეებს აქვთ $Y \leq Y_0$.

ტრიპს ასევე ხშირად მოიხსენიებენ, როგორც "კარტეზიულ ხეს", რადგან ადვილია მისი ჩასმა დეკარტის სიბრტყეში:

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/e/e4/Treap.svg" width="350px"/>
</center>

Treaps შემოგვთავაზეს რაიმუნდ სიდელმა და სესილია არაგონმა 1989 წელს.

## მონაცემთა ასეთი ორგანიზაციის უპირატესობები

ასეთ განხორციელებაში, $X$ მნიშვნელობები არის გასაღებები (და ამავე დროს ფასეულობებში შენახული მნიშვნელობები), ხოლო $Y$ მნიშვნელობებს ეწოდება **პრიორიტეტები**. პრიორიტეტების გარეშე, ტრიპი იქნება ჩვეულებრივი ორობითი საძიებო ხე $X$-ით და $X$-ის მნიშვნელობების ერთი ნაკრები შეიძლება შეესაბამებოდეს უამრავ სხვადასხვა ხეს, ზოგიერთი მათგანი დეგენერირებულია (მაგალითად, დაკავშირებული სიის სახით) , და, შესაბამისად, უკიდურესად ნელი (მთავარი ოპერაციები იქნება $O(N)$ სირთულის).

ამავდროულად, **პრიორიტეტები** (როდესაც ისინი უნიკალურია) საშუალებას იძლევა **უნიკალურად** მიუთითოთ ის ხე, რომელიც აშენდება (რა თქმა უნდა, ეს არ არის დამოკიდებული მნიშვნელობების დამატების თანმიმდევრობაზე), რომელიც შეიძლება დადასტურდეს შესაბამისი თეორემის გამოყენებით. ცხადია, თუ ** პრიორიტეტებს შემთხვევით აირჩევთ**, საშუალოდ მიიღებთ არადეგენერაციულ ხეებს, რაც უზრუნველყოფს $O(\log N)$ სირთულეს ძირითადი ოპერაციებისთვის. აქედან გამომდინარეობს ამ მონაცემთა სტრუქტურის სხვა სახელი - **რანდომიზებული ბინარული საძიებო ხე**.

## ოპერაციები

Treap უზრუნველყოფს შემდეგ ოპერაციებს:

- **ჩასვით (X,Y)** $O(\log N)$-ში.
  ამატებს ახალ კვანძს ხეს. ერთ-ერთი შესაძლო ვარიანტია მხოლოდ $X$-ის გადაცემა და $Y$-ის გენერირება ოპერაციის შიგნით შემთხვევით.
- **მოძებნეთ (X)** $O(\log N)$-ში.
  ეძებს კვანძს მითითებული გასაღების მნიშვნელობით $X$. განხორციელება იგივეა, რაც ჩვეულებრივი ორობითი საძიებო ხისთვის.
- **წაშალე (X)** $O(\log N)$-ში.
  ეძებს კვანძს მითითებული გასაღების მნიშვნელობით $X$ და შლის მას ხიდან.
- **აშენება ($X_1$, ..., $X_N$)** $O(N)$-ში.
  აშენებს ხეს ღირებულებების სიიდან. ეს შეიძლება გაკეთდეს წრფივ დროში (იმ პირობით, რომ $X_1, ..., X_N$ დალაგებულია).
- **კავშირი ($T_1$, $T_2$)** $O(M \log (N/M))$-ში.
  აერთიანებს ორ ხეს, იმ ვარაუდით, რომ ყველა ელემენტი განსხვავებულია. შესაძლებელია იგივე სირთულის მიღწევა, თუ დუბლიკატი ელემენტები უნდა მოიხსნას შერწყმის დროს.
- **იკვეთება ($T_1$, $T_2$)** $O(M \log (N/M))$-ში.
  პოულობს ორი ხის კვეთას (ანუ მათ საერთო ელემენტებს). ჩვენ აქ არ განვიხილავთ ამ ოპერაციის განხორციელებას.

გარდა ამისა, იმის გამო, რომ treap არის ორობითი საძიებო ხე, მას შეუძლია სხვა ოპერაციების განხორციელება, როგორიცაა $K$-th უდიდესი ელემენტის პოვნა ან ელემენტის ინდექსის პოვნა.

## განხორციელების აღწერა

განხორციელების თვალსაზრისით, თითოეული კვანძი შეიცავს $X$, $Y$ და მითითებებს მარცხნივ ($L$) და მარჯვნივ ($R$) შვილებზე.

ჩვენ განვახორციელებთ ყველა საჭირო ოპერაციას მხოლოდ ორი დამხმარე ოპერაციების გამოყენებით: Split და Merge.

### გაყოფა

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/6/69/Treap_split.svg" width="450px"/>
</center>

** გაყოფა ($T$, $X$)** ჰყოფს $T$ ხეს 2 ქვეხეებში $L$ და $R$ ხეებში (რომლებიც გაყოფის დაბრუნების მნიშვნელობებია) ისე, რომ $L$ შეიცავს ყველა ელემენტს გასაღებით $ X_L \le X$ და $R$ შეიცავს ყველა ელემენტს გასაღებით $X_R > X$. ამ ოპერაციას აქვს $O (\log N)$ სირთულე და ხორციელდება სუფთა რეკურსიის გამოყენებით:

1. თუ ძირეული კვანძის (R) მნიშვნელობა არის $\le X$, მაშინ `L` სულ მცირე, შედგებოდა `R->L` და `R`. შემდეგ ჩვენ მოვუწოდებთ split-ს `R->R`-ზე და აღვნიშნავთ მის გაყოფის შედეგს, როგორც `L' და `R'. და ბოლოს, "L" ასევე შეიცავდა "L"-ს, ხოლო "R = R"".
2. თუ ძირეული კვანძის (R) მნიშვნელობა არის $> X$, მაშინ `R` სულ მცირე, შედგებოდა `R` და `R->R`. ჩვენ მოვუწოდებთ split-ს `R->L`-ზე და აღვნიშნავთ მის გაყოფის შედეგს, როგორც `L` და `R`. და ბოლოს, `L=L`, ხოლო `R` ასევე შეიცავს `R`-ს.

ამრიგად, გაყოფის ალგორითმი არის:

1. გადაწყვიტეთ რომელ ქვეხეს მიეკუთვნება ძირეული კვანძი (მარცხნივ ან მარჯვნივ)
2. რეკურსიულად გამოძახება split მის ერთ-ერთ შვილზე
3. შექმენით საბოლოო შედეგი რეკურსიული გაყოფილი ზარის ხელახალი გამოყენებით.

### შერწყმა

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/a/a8/Treap_merge.svg" width="500px"/>
</center>

** შერწყმა ($T_1$, $T_2$)** აერთიანებს ორ ქვეხეს $T_1$ და $T_2$ და აბრუნებს ახალ ხეს. ამ ოპერაციას ასევე აქვს $O (\log N)$ სირთულე. ის მუშაობს იმ ვარაუდით, რომ $T_1$ და $T_2$ შეკვეთილია (ყველა გასაღები $X$$T_1$-ში უფრო პატარაა ვიდრე $T_2$-ის კლავიშები). ამრიგად, ჩვენ უნდა გავაერთიანოთ ეს ხეები $Y$ პრიორიტეტების რიგის დარღვევის გარეშე. ამისათვის ჩვენ ვირჩევთ ძირად ხეს, რომელსაც აქვს $Y$ უფრო მაღალი პრიორიტეტი ძირეულ კვანძში და რეკურსიულად მოვუწოდებთ Merge-ს სხვა ხისთვის და შერჩეული ძირეული კვანძის შესაბამის ქვეხეს.

### ჩასმა

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/3/35/Treap_insert.svg" width="500px"/>
</center>

ახლა **Insert ($X$, $Y$)**-ის განხორციელება აშკარა ხდება. ჯერ ჩავდივართ ხეში (როგორც ჩვეულებრივ ორობითი საძიებო ხეში X-ით) და ვჩერდებით პირველ კვანძზე, რომელშიც პრიორიტეტული მნიშვნელობა $Y$-ზე ნაკლებია. ჩვენ ვიპოვეთ ადგილი, სადაც ჩავსვამთ ახალ ელემენტს. შემდეგი, ჩვენ მოვუწოდებთ **Split (T, X)** ქვეხეს, რომელიც იწყება ნაპოვნი კვანძიდან და ვიყენებთ დაბრუნებულ ქვეხეებს $L$ და $R$ როგორც ახალი კვანძის მარცხენა და მარჯვენა შვილები.

ალტერნატიულად, ჩასმა შეიძლება განხორციელდეს საწყისი ტრიპის $X$-ზე გაყოფით და $2$-ის ახალ კვანძთან შერწყმით (იხილეთ სურათი).


### წაშლა

<center>
<img src="https://upload.wikimedia.org/wikipedia/commons/6/62/Treap_erase.svg" width="500px"/>
</center>

**Erase ($X$)**-ის განხორციელება ასევე გასაგებია. ჯერ ჩავდივართ ხეზე (როგორც ჩვეულებრივ ორობითი საძიებო ხეში $X$-ით), ვეძებთ ელემენტს, რომლის წაშლა გვინდა. კვანძის აღმოჩენის შემდეგ, ჩვენ მასზე **Merge**-ს ვუწოდებთ ბავშვებს და ვათავსებთ ოპერაციის დაბრუნების მნიშვნელობას იმ ელემენტის ადგილას, რომელსაც ჩვენ ვშლით.

ალტერნატიულად, ჩვენ შეგვიძლია გამოვყოთ ქვეხე, რომელიც ფლობს $X$-ს $2$-ის გაყოფის ოპერაციებით და გავაერთიანოთ დარჩენილი ტრიპები (იხილეთ სურათი).

### აშენება

ჩვენ ვახორციელებთ **Build** ოპერაციას $O (N \log N)$ სირთულით $N$ **Insert** ზარების გამოყენებით.

### კავშირი

**კავშირს ($T_1$, $T_2$)** აქვს თეორიული სირთულე $O (M \log (N/M))$, მაგრამ პრაქტიკაში ის ძალიან კარგად მუშაობს, ალბათ ძალიან მცირე ფარული მუდმივით. ზოგადობის დაკარგვის გარეშე დავუშვათ, რომ $T_1 \rightarrow Y > T_2 \rightarrow Y$, ე.ი. ე. $T_1$-ის ფესვი იქნება შედეგის ფესვი. შედეგის მისაღებად, ჩვენ უნდა გავაერთიანოთ ხეები $T_1 \rightarrow L$, $T_1 \rightarrow R$ და $T_2$ ორ ხეში, რომლებიც შეიძლება იყოს $T_1$ ფესვის შვილები. ამისათვის ჩვენ ვუწოდებთ Split-ს ($T_2$, $T_1\rightarrow X$), რითაც ვყოფთ $T_2$-ს ორ ნაწილად L და R, რომელსაც შემდეგ რეკურსიულად ვაკავშირებთ $T_1$-ის შვილებთან: კავშირი ($T_1 \rightarrow L$, $L$) და Union ($T_1 \rightarrow R$, $R$), რითაც მიიღება შედეგის მარცხენა და მარჯვენა ქვეხეები.

## განხორციელება

```cpp
struct item {
	int key, prior;
	item *l, *r;
	item () { }
	item (int key) : key(key), prior(rand()), l(NULL), r(NULL) { }
	item (int key, int prior) : key(key), prior(prior), l(NULL), r(NULL) { }
};
typedef item* pitem;
```

ეს არის ჩვენი ნივთის განმარტება. გაითვალისწინეთ, რომ არსებობს ორი შვილობილი მაჩვენებელი და მთელი რიცხვი (BST) და მთელი რიცხვი პრიორიტეტი (გროვისთვის). პრიორიტეტი ენიჭება შემთხვევითი რიცხვების გენერატორის გამოყენებით.

```cpp
void split (pitem t, int key, pitem & l, pitem & r) {
	if (!t)
		l = r = NULL;
	else if (t->key <= key)
        split (t->r, key, t->r, r),  l = t;
	else
        split (t->l, key, l, t->l),  r = t;
}
```

`t` არის ტრიპის გაყოფა, ხოლო `key` არის BST მნიშვნელობა, რომლითაც უნდა გაიყოს. გაითვალისწინეთ, რომ ჩვენ არსად არ `return` შედეგის მნიშვნელობებს, სამაგიეროდ, უბრალოდ ვიყენებთ მათ ასე:


```cpp
pitem l = nullptr, r = nullptr;
split(t, 5, l, r);
if (l) cout << "Left subtree size: " << (l->size) << endl;
if (r) cout << "Right subtree size: " << (r->size) << endl;
```

ამ `split` ფუნქციის გაგება შეიძლება რთული იყოს, რადგან მას აქვს როგორც მაჩვენებლები (`pitem`), ასევე მითითება ამ მაჩვენებლებზე (`pitem &l`). მოდით გავიგოთ სიტყვებით, რას გულისხმობს ფუნქციის გამოძახება `split(t, k, l, r)`: „გაყავით ტრიპი `t` მნიშვნელობით `k` ორ ნაწილად, და შეინახეთ მარცხენა ნაკაწრები `l`-ში და მარჯვენა treap-ში. `r`-ში“. დიდი! ახლა, მოდით გამოვიყენოთ ეს განმარტება ორ რეკურსიულ ზარზე, წინა განყოფილებაში გაანალიზებული შემთხვევის სამუშაოს გამოყენებით: (პირველი, თუ პირობა არის ტრივიალური საბაზისო შემთხვევა ცარიელი ტრიპისთვის)


1. როდესაც ძირეული კვანძის მნიშვნელობა არის $\le$ გასაღები, ჩვენ ვუწოდებთ `split (t->r, key, t->r, r)`, რაც ნიშნავს: „გაყოფა ტრიპ `t->r` (მარჯვენა ქვეხე `t`) მნიშვნელობით `key` და შეინახეთ მარცხენა ქვეხე `t->r`-ში და მარჯვენა ქვეხე `r`-ში“. ამის შემდეგ ჩვენ ვაყენებთ `l = t`. ახლა გაითვალისწინეთ, რომ `l` შედეგის მნიშვნელობა შეიცავს `t->l`, `t` და ასევე `t->r` (რომელიც ჩვენ მიერ განხორციელებული რეკურსიული ზარის შედეგია) ყველა უკვე გაერთიანებულია სწორი თანმიმდევრობით! თქვენ უნდა შეაჩეროთ, რათა დარწმუნდეთ, რომ `l` და `r`-ის ეს შედეგი ზუსტად შეესაბამება იმას, რაც ადრე განვიხილეთ განხორციელების აღწერაში.
2. როდესაც ძირეული კვანძის მნიშვნელობა კლავიშზე მეტია, ჩვენ ვუწოდებთ `split (t->l, key, l, t->l)`, რაც ნიშნავს: „გაყოფა ტრიპ `t->l` (მარცხენა ქვეხე `-ის t`) მნიშვნელობით `key` და შეინახეთ მარცხენა ქვეხე `l`-ში და მარჯვენა ქვეხე `t->l`-ში“. ამის შემდეგ ჩვენ ვაყენებთ `r = t`. გაითვალისწინეთ, რომ `r` შედეგის მნიშვნელობა შეიცავს `t->l` (რომელიც ჩვენ მიერ განხორციელებული რეკურსიული ზარის შედეგია), `t` და ასევე `t->r`, ყველა უკვე გაერთიანებულია სწორი თანმიმდევრობით! თქვენ უნდა შეაჩეროთ, რათა დარწმუნდეთ, რომ `l` და `r`-ის ეს შედეგი ზუსტად შეესაბამება იმას, რაც ადრე განვიხილეთ განხორციელების აღწერაში.

თუ ჯერ კიდევ უჭირთ განხორციელების გაგება, უნდა შეხედოთ მას _ინდუქციურად_, ანუ: *ნუ* ცდილობთ რეკურსიული ზარების განმეორებით დაშლას. დავუშვათ, რომ გაყოფის განხორციელება სწორად მუშაობს ცარიელ ტრიპზე, შემდეგ სცადეთ მისი გაშვება ერთი კვანძისთვის, შემდეგ ორი კვანძისთვის და ა.შ.

```cpp
void insert (pitem & t, pitem it) {
	if (!t)
		t = it;
	else if (it->prior > t->prior)
		split (t, it->key, it->l, it->r),  t = it;
	else
		insert (t->key <= it->key ? t->r : t->l, it);
}

void merge (pitem & t, pitem l, pitem r) {
	if (!l || !r)
		t = l ? l : r;
	else if (l->prior > r->prior)
		merge (l->r, l->r, r),  t = l;
	else
		merge (r->l, l, r->l),  t = r;
}

void erase (pitem & t, int key) {
	if (t->key == key) {
		pitem th = t;
		merge (t, t->l, t->r);
		delete th;
	}
	else
		erase (key < t->key ? t->l : t->r, key);
}

pitem unite (pitem l, pitem r) {
	if (!l || !r)  return l ? l : r;
	if (l->prior < r->prior)  swap (l, r);
	pitem lt, rt;
	split (r, l->key, lt, rt);
	l->l = unite (l->l, lt);
	l->r = unite (l->r, rt);
	return l;
}
```

## ქვეხეების ზომის შენარჩუნება

Treap-ის ფუნქციონირების გასაგრძელებლად, ხშირად საჭიროა კვანძების რაოდენობის შენახვა თითოეული კვანძის ქვეხეში - ველი `int cnt` `item` სტრუქტურაში. მაგალითად, ის შეიძლება გამოყენებულ იქნას ხის K-ე უდიდესი ელემენტის მოსაძებნად $O (\log N)$-ში, ან ელემენტის ინდექსის საპოვნელად დახარისხებულ სიაში იგივე სირთულით. ამ ოპერაციების განხორციელება იგივე იქნება, რაც ჩვეულებრივი ორობითი საძიებო ხისთვის.

როდესაც ხე იცვლება (კვანძები ემატება ან წაიშლება და ა.შ.), ზოგიერთი კვანძის `cnt` უნდა განახლდეს შესაბამისად. ჩვენ შევქმნით ორ ფუნქციას: `cnt()` დააბრუნებს `cnt`-ის მიმდინარე მნიშვნელობას ან 0-ს, თუ კვანძი არ არსებობს, და `upd_cnt()` განაახლებს `cnt`-ის მნიშვნელობას ამ კვანძისთვის, თუ ვივარაუდებთ, რომ მისი შვილები L და R `cnt`-ის მნიშვნელობები უკვე განახლებულია. აშკარად საკმარისია `upd_cnt()`-ის ზარების დამატება `insert`, `erase`, `split` და `merge` ბოლოს, რათა `cnt` მნიშვნელობები განახლებული იყოს.

```cpp
int cnt (pitem t) {
	return t ? t->cnt : 0;
}

void upd_cnt (pitem t) {
	if (t)
		t->cnt = 1 + cnt(t->l) + cnt (t->r);
}
```

## Treap-ის შექმნა $O (N)$-ში ოფლაინ რეჟიმში {data-toc-label="Treap-ის აშენება O(N)-ში ოფლაინ რეჟიმში"}

გასაღებების დალაგებული სიის გათვალისწინებით, შესაძლებელია ტრიპის აწყობა უფრო სწრაფად, ვიდრე კლავიშების ერთ ჯერზე ჩასმა, რაც მოითხოვს $O(N \log N)$. ვინაიდან გასაღებები დალაგებულია, დაბალანსებული ორობითი საძიებო ხე შეიძლება ადვილად აშენდეს წრფივ დროში. $Y$ გროვის მნიშვნელობები ინიციალიზებულია შემთხვევით და შემდეგ შეიძლება დაგროვდეს ღილაკებისგან დამოუკიდებლად $X$, რათა [გროვის აშენება](https://en.wikipedia.org/wiki/Binary_heap#Building_a_heap) $O(N)-ში. $.

```cpp
void heapify (pitem t) {
	if (!t) return;
	pitem max = t;
	if (t->l != NULL && t->l->prior > max->prior)
		max = t->l;
	if (t->r != NULL && t->r->prior > max->prior)
		max = t->r;
	if (max != t) {
		swap (t->prior, max->prior);
		heapify (max);
	}
}

pitem build (int * a, int n) {
	// Construct a treap on values {a[0], a[1], ..., a[n - 1]}
	if (n == 0) return NULL;
	int mid = n / 2;
	pitem t = new item (a[mid], rand ());
	t->l = build (a, mid);
	t->r = build (a + mid + 1, n - mid - 1);
	heapify (t);
	upd_cnt(t)
	return t;
}
```

შენიშვნა: `upd_cnt(t)`-ის გამოძახება საჭიროა მხოლოდ იმ შემთხვევაში, თუ გჭირდებათ ქვეხის ზომები.

ზემოთ მოყვანილი მიდგომა ყოველთვის იძლევა იდეალურად დაბალანსებულ ხეს, რომელიც ზოგადად კარგია პრაქტიკული მიზნებისთვის, მაგრამ იმ პრიორიტეტების არ შენარჩუნების ხარჯზე, რომლებიც თავდაპირველად მინიჭებული იყო თითოეულ კვანძზე. ამრიგად, ეს მიდგომა შეუძლებელია შემდეგი პრობლემის გადასაჭრელად:

!!! მაგალითი "[acmsguru - Cartesian Tree](https://codeforces.com/problemsets/acmsguru/problem/99999/155)"
	მოცემული $(x_i, y_i)$ წყვილების თანმიმდევრობა, ააგეთ მათზე კარტეზიული ხე. ყველა $x_i$ და ყველა $y_i$ უნიკალურია.

გაითვალისწინეთ, რომ ამ პრობლემაში პრიორიტეტები შემთხვევითი არ არის, ამიტომ მხოლოდ წვეროების სათითაოდ ჩასმა შეიძლება უზრუნველყოს კვადრატული გადაწყვეტა.

აქ ერთ-ერთი შესაძლო გამოსავალი არის თითოეული ელემენტისთვის მარცხნივ და მარჯვნივ უახლოესი ელემენტების პოვნა, რომლებსაც ამ ელემენტზე ნაკლები პრიორიტეტი აქვთ. ამ ორ ელემენტს შორის უფრო დიდი პრიორიტეტი უნდა იყოს მიმდინარე ელემენტის მშობელი.

ეს პრობლემა მოგვარებულია [მინიმალური დასტა](./stack_queue_modification.md) მოდიფიკაცია წრფივ დროში:

```cpp
void connect(auto from, auto to) {
    vector<pitem> st;
    for(auto it: ranges::subrange(from, to)) {
        while(!st.empty() && st.back()->prior > it->prior) {
            st.pop_back();
        }
        if(!st.empty()) {
            if(!it->p || it->p->prior < st.back()->prior) {
                it->p = st.back();
            }
        }
        st.push_back(it);
    }
}

pitem build(int *x, int *y, int n) {
    vector<pitem> nodes(n);
    for(int i = 0; i < n; i++) {
        nodes[i] = new item(x[i], y[i]);
    }
    connect(nodes.begin(), nodes.end());
    connect(nodes.rbegin(), nodes.rend());
    for(int i = 0; i < n; i++) {
        if(nodes[i]->p) {
            if(nodes[i]->p->key < nodes[i]->key) {
                nodes[i]->p->r = nodes[i];
            } else {
                nodes[i]->p->l = nodes[i];
            }
        }
    }
    return nodes[min_element(y, y + n) - y];
}
```

## იმპლიციტური მარშრუტები

Implicit Treap არის ჩვეულებრივი მოდიფიკაციის მარტივი მოდიფიკაცია, რომელიც არის მონაცემთა ძალიან ძლიერი სტრუქტურა. ფაქტობრივად, იმპლიციტური treap შეიძლება ჩაითვალოს მასივად შემდეგი პროცედურების განხორციელებით (ყველა $O (\log N)$ ონლაინ რეჟიმში):

- ელემენტის ჩასმა მასივში ნებისმიერ ადგილას
- თვითნებური ელემენტის მოხსნა
- თვითნებურ ინტერვალზე ჯამის, მინიმალური/მაქსიმალური ელემენტის და ა.შ
- დამატება, მოხატვა თვითნებურ ინტერვალზე
- ელემენტების შებრუნება თვითნებურ ინტერვალზე

იდეა არის ის, რომ გასაღებები უნდა იყოს მასივის ელემენტების ნულზე დაფუძნებული **ინდექსები**. მაგრამ ჩვენ არ ვინახავთ ამ მნიშვნელობებს ცალსახად (წინააღმდეგ შემთხვევაში, მაგალითად, ელემენტის ჩასმა გამოიწვევს გასაღების ცვლილებას ხის $O (N)$ კვანძებში).

გაითვალისწინეთ, რომ კვანძის გასაღები არის მასზე ნაკლები კვანძების რაოდენობა (ასეთი კვანძები შეიძლება იყოს არა მხოლოდ მის მარცხენა ქვეხეში, არამედ მისი წინაპრების მარცხენა ქვეხეებშიც).
უფრო კონკრეტულად, **იმპლიციტური გასაღები** ზოგიერთი კვანძისთვის T არის $cnt (T \rightarrow L)$ წვეროების რაოდენობა ამ კვანძის მარცხენა ქვეხეში პლუს მსგავსი მნიშვნელობები $cnt (P \rightarrow L) + 1$ for T კვანძის P თითოეული წინაპარი, თუ T არის P-ის მარჯვენა ქვეხეში.

ახლა გასაგებია, როგორ სწრაფად გამოვთვალოთ მიმდინარე კვანძის იმპლიციტური გასაღები. მას შემდეგ, რაც ყველა ოპერაციაში ჩვენ მივდივართ ნებისმიერ კვანძში ხეზე დაშვებით, შეგვიძლია უბრალოდ დავაგროვოთ ეს ჯამი და გადავიტანოთ ფუნქციას. თუ მარცხენა ქვეხეზე მივდივართ, დაგროვილი ჯამი არ იცვლება, თუ მარჯვენა ქვეხეზე მივდივართ ის იზრდება $cnt (T \rightarrow L) +1$-ით.

აქ არის **Split** და **Merge**-ის ახალი დანერგვები:

```cpp
void merge (pitem & t, pitem l, pitem r) {
	if (!l || !r)
		t = l ? l : r;
	else if (l->prior > r->prior)
		merge (l->r, l->r, r),  t = l;
	else
		merge (r->l, l, r->l),  t = r;
	upd_cnt (t);
}

void split (pitem t, pitem & l, pitem & r, int key, int add = 0) {
	if (!t)
		return void( l = r = 0 );
	int cur_key = add + cnt(t->l); //implicit key
	if (key <= cur_key)
		split (t->l, l, t->l, key, add),  r = t;
	else
		split (t->r, t->r, r, key, add + 1 + cnt(t->l)),  l = t;
	upd_cnt (t);
}
```

ზემოთ განხორციელებისას, $split(T, T_1, T_2, k)$-ის გამოძახების შემდეგ, ხე $T_1$ შედგება $T$-ის პირველი $k$ ელემენტებისაგან (ანუ ელემენტებისაგან, რომლებსაც აქვთ მათი იმპლიციტური გასაღები ნაკლები. ვიდრე $k$) და $T_2$ შედგება ყველა დანარჩენისგან.

ახლა მოდით განვიხილოთ სხვადასხვა ოპერაციების განხორციელება იმპლიციტური ტრიპებზე:

- **ელემენტის ჩასმა**.
  დავუშვათ, ჩვენ უნდა ჩავსვათ ელემენტი $pos$ პოზიციაზე. ჩვენ ვყოფთ ტრიპს ორ ნაწილად, რომლებიც შეესაბამება $[0..pos-1]$ და $[pos..sz]$ მასივებს; ამისათვის ჩვენ ვუწოდებთ $split(T, T_1, T_2, pos)$. შემდეგ შეგვიძლია გავაერთიანოთ ხე $T_1$ ახალ წვეროსთან $merge(T_1, T_1, \text{new item})$-ის გამოძახებით (მარტივი ჩანს, რომ ყველა წინაპირობა დაკმაყოფილებულია). და ბოლოს, ჩვენ ვაკავშირებთ ხეებს $T_1$ და $T_2$ ისევ $T$-ში $merge(T, T_1, T_2)$-ის გამოძახებით.
- **ელემენტის წაშლა**.
 ეს ოპერაცია კიდევ უფრო მარტივია: იპოვნეთ წასაშლელი ელემენტი $T$, შეასრულეთ მისი შვილების $L$ და $R$ შერწყმა და შეცვალეთ ელემენტი $T$ შერწყმის შედეგით. ფაქტობრივად, ელემენტის წაშლა იმპლიციტურ თრეპში ზუსტად იგივეა, რაც ჩვეულებრივ ტრიპში.
- ინტერვალზე იპოვეთ **ჯამი / მინიმალური** და ა.შ.
 პირველი, შექმენით დამატებითი $F$ ველი `item` სტრუქტურაში ამ კვანძის ქვეხის სამიზნე ფუნქციის მნიშვნელობის შესანახად. ამ ველის შენარჩუნება მარტივია ქვეხეების ზომის შენარჩუნების მსგავსად: შექმენით ფუნქცია, რომელიც ითვლის ამ მნიშვნელობას კვანძისთვის მისი შვილების მნიშვნელობებზე დაყრდნობით და დაამატეთ ამ ფუნქციის გამოძახებები ყველა ფუნქციის ბოლოს, რომელიც ცვლის ხეს.
 მეორე, ჩვენ უნდა ვიცოდეთ როგორ დავამუშავოთ მოთხოვნა თვითნებური ინტერვალისთვის $[A; B]$.
 ხის ნაწილის მისაღებად, რომელიც შეესაბამება $[A-ს ინტერვალს; B]$, ჩვენ უნდა გამოვიძახოთ $split(T, T_2, T_3, B+1)$ და შემდეგ $split(T_2, T_1, T_2, A)$: ამის შემდეგ $T_2$ შედგება ყველა ელემენტისგან ინტერვალი $[A; B]$ და მხოლოდ მათგან. ამიტომ, შეკითხვაზე პასუხი შეინახება $T_2$-ის ფესვის $F$ ველში. კითხვაზე პასუხის გაცემის შემდეგ, ხე უნდა აღდგეს $merge(T, T_1, T_2)$ და $merge(T, T, T_3)$-ის გამოძახებით.
 - **დამატება / ფერწერა** ინტერვალზე.
 ჩვენ ვმოქმედებთ წინა აბზაცის მსგავსად, მაგრამ F ველის ნაცვლად ვინახავთ ველს `add` რომელიც შეიცავს ქვეხის დამატებულ მნიშვნელობას (ან იმ მნიშვნელობას, რომლითაც არის დახატული ქვეხე). ნებისმიერი ოპერაციის შესრულებამდე ჩვენ უნდა "დავძლიოთ" ეს მნიშვნელობა სწორად - ანუ შევცვალოთ $T \rightarrow L \rightarrow add$ და $T \rightarrow R \rightarrow add$ და გავასუფთავოთ `add` მშობელ კვანძში. ამ გზით ხეზე რაიმე ცვლილების შემდეგ ინფორმაცია არ დაიკარგება.
- **უკუ** ინტერვალზე.
 ეს ისევ წინა ოპერაციის მსგავსია: ჩვენ უნდა დავამატოთ ლოგიკური დროშა `rev` და დავაყენოთ ის true-ზე, როდესაც მიმდინარე კვანძის ქვეხე უნდა შეიცვალოს. ამ მნიშვნელობის „დაძაბვა“ ცოტა რთულია - ჩვენ ვცვლით ამ კვანძის შვილებს და ვაყენებთ ამ დროშას მათთვის true.

აქ არის იმპლიციტური ტრიპის განხორციელების მაგალითი ინტერვალზე შებრუნებით. თითოეული კვანძისთვის ჩვენ ვინახავთ ველს სახელწოდებით `value` რომელიც არის მასივის ელემენტის რეალური მნიშვნელობა მიმდინარე პოზიციაზე. ჩვენ ასევე გთავაზობთ `output()` ფუნქციის განხორციელებას, რომელიც გამოსცემს მასივს, რომელიც შეესაბამება იმპლიციტური ტრიპის მიმდინარე მდგომარეობას.

```cpp
typedef struct item * pitem;
struct item {
	int prior, value, cnt;
	bool rev;
	pitem l, r;
};

int cnt (pitem it) {
	return it ? it->cnt : 0;
}

void upd_cnt (pitem it) {
	if (it)
		it->cnt = cnt(it->l) + cnt(it->r) + 1;
}

void push (pitem it) {
	if (it && it->rev) {
		it->rev = false;
		swap (it->l, it->r);
		if (it->l)  it->l->rev ^= true;
		if (it->r)  it->r->rev ^= true;
	}
}

void merge (pitem & t, pitem l, pitem r) {
	push (l);
	push (r);
	if (!l || !r)
		t = l ? l : r;
	else if (l->prior > r->prior)
		merge (l->r, l->r, r),  t = l;
	else
		merge (r->l, l, r->l),  t = r;
	upd_cnt (t);
}

void split (pitem t, pitem & l, pitem & r, int key, int add = 0) {
	if (!t)
		return void( l = r = 0 );
	push (t);
	int cur_key = add + cnt(t->l);
	if (key <= cur_key)
		split (t->l, l, t->l, key, add),  r = t;
	else
		split (t->r, t->r, r, key, add + 1 + cnt(t->l)),  l = t;
	upd_cnt (t);
}

void reverse (pitem t, int l, int r) {
	pitem t1, t2, t3;
	split (t, t1, t2, l);
	split (t2, t2, t3, r-l+1);
	t2->rev ^= true;
	merge (t, t1, t2);
	merge (t, t, t3);
}

void output (pitem t) {
	if (!t)  return;
	push (t);
	output (t->l);
	printf ("%d ", t->value);
	output (t->r);
}
```

## ლიტერატურა

* [Blelloch, Reid-Miller "Fast Set Operations Using Treaps"](https://www.cs.cmu.edu/~scandal/papers/treaps-spaa98.pdf)

## პრაქტიკის პრობლემები

* [SPOJ - Ada and Aphids](http://www.spoj.com/problems/ADAAPHID/)
* [SPOJ - Ada and Harvest](http://www.spoj.com/problems/ADACROP/)
* [Codeforces - Radio Stations](http://codeforces.com/contest/762/problem/E)
* [SPOJ - Ghost Town](http://www.spoj.com/problems/COUNT1IT/)
* [SPOJ - Arrangement Validity](http://www.spoj.com/problems/IITWPC4D/)
* [SPOJ - All in One](http://www.spoj.com/problems/ALLIN1/)
* [Codeforces - Dog Show](http://codeforces.com/contest/847/problem/D)
* [Codeforces - Yet Another Array Queries Problem](http://codeforces.com/contest/863/problem/D)
* [SPOJ - Mean of Array](http://www.spoj.com/problems/MEANARR/)
* [SPOJ - TWIST](http://www.spoj.com/problems/TWIST/)
* [SPOJ - KOILINE](http://www.spoj.com/problems/KOILINE/)
* [CodeChef - The Prestige](https://www.codechef.com/problems/PRESTIGE)
* [Codeforces - T-Shirts](https://codeforces.com/contest/702/problem/F)
* [Codeforces - Wizards and Roads](https://codeforces.com/problemset/problem/167/D)
* [Codeforces - Yaroslav and Points](https://codeforces.com/contest/295/problem/E)