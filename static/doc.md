# Ključna beseda ```static```

Originalno je beseda pomenila, da je spremenljivka alocirana statično ([link](https://en.wikipedia.org/wiki/Static_variable)). 
Torej je za njo že v naprej pripravljen prostor v podatkovnem segmentu (ni na skladu) ([link](https://en.wikipedia.org/wiki/Data_segment)).
Živlenska doba take spremenljivke je celoten čas zagona programa. ‡

Vse globalne spremenljivke so alocirane statično, za lokalne pa to dosežemo tako, da jih označimo s ```static```. ‡

```cpp
int f(const int n) {
  static int x = 0; // Inicializira se ob prvem zagonu funkcije
  x += n;
  return x;
}

int main() {
  std::cout << f(0) << std::endl;
  std::cout << f(5) << std::endl;
  std::cout << f(1) << std::endl;
}
```

## Vidnost‡

```static``` se prav tako uporablja za določanje vidnosti globalnih elementov ([link](https://en.cppreference.com/w/cpp/language/storage_duration#Linkage)).
Ta uporaba *ni* na noben način povezana s prvo.

```cpp
static const int i = 1;

static void f() { ... }

int main() {
  ...
}
```

## Razredne spremenljivke in metode

```static``` v kontekstu razredov pomeni, da spremenljivke in metode pripadajo razredu samemu in ne kateri od instanc (zato jim rečemo razredne).
Konceptualno so zelo podobne globalnim spremenljivkam in funkcijam le, da so vezane na razred.
Razredne spremenljivke so alocirane statično (skladno s prvo uporabo).

Do njih moramo od zunaj dostopati z uporabo operatorja ```::```.
Enako, kot pri definicji metod zunaj razreda ([link](https://en.cppreference.com/w/cpp/language/scope#Class_scope)) in pri dostopu do elementov v imenskem prostoru (npr. ```std::...```) ([link](https://en.cppreference.com/w/cpp/language/namespace)).
Znotraj razreda lahko dostopamo do njih enako, kot do instančnih spremenljivk in metod (brez ```::```).

### Kdaj jih uporabljamo?
Takrat, ko potrebujemo globalno spremenljivko ali funkcijo in
  * ta logično pripada razredu
  * želimo omejti dostop (npr. s ```private```)
  * funkcija potrebuje dostop do razrednih spremenljivk
  
Razredne spremenljivke z javnim dostopom (```public```), so praktično enake globalnih spremenljivkam, torej zanje veljajo enake pomanjkljivosti, zato se jim poskušamo izogibati.
Če želimo, da so vidne navzven je dobro, da so konstantne (```const```) ([link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#r6-avoid-non-const-global-variables)) oz. jih naredimo privatne in ne implementiramo setterja.

```cpp
class Count {
public:
  Count();
  ~Count();
  static int get_count();
private:
  static int count;
};

int Count::count = 0; // Inicializiramo zunaj razreda

int Count::get_count() {
  return count; // Potrebujemo dostop do razredne spremenljivke, torej static
}

Count::Count() {
  count++;
}

Count::~Count() {
  count--;
}  

int main() {
  std::cout << Count::get_count() << std::endl; // Vedno pokličemo z imenom razreda in ne instance
  Count c1;
  std::cout << Count::get_count() << std::endl;
  Count c2;
  std::cout << Count::get_count() << std::endl;
}
```

```cpp
class Math {
public:
  static int max(int, int);
};

int Math::max(const int x, const int y) { // To bi lahko bila tudi funkcija
  return std::max(x, y);
}

int main() {
  std::cout << Math::max(1, 2) << std::endl; 
}
```

‡: Dodatne informacije
