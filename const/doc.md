# Določilo ```const```

Dobra praksa je, da spremenljivke, argumente funkcij in metode označimo kot ```const```, v kolikor se njihova vrednost tekom programa ne spremeni.
Najlažje dosežemo to tako, da na začetku vedno zapišemo vsepovsod, kjer je to zaželjeno, ```const```, nato pa kasneje ```const``` odstranimo, če ugotovimo, da dana vrednost ni konstanta.
Razlog za to je, ker nam prevajalnik manjkajočega določila ```const``` ne javi kot napako, v nasptrotnem primeru, ko poskušamo spremeniti vrednost konstante, pa napako javi.
Na tak način določila ```const``` ne moremo pozabiti.

## Lokalne in globalne spremenljivke

Vse spremenljivke, ki se ne spremenijo označimo s const ([link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rconst-immutable)).

```cpp
const int value = 1;
```

Številčne spremenljivke s ```const``` lahko uporabimo za določitev velikosti polja (namesto ```#define```).
To je posebna izjema v standardu ([link](https://en.cppreference.com/w/cpp/language/constant_expression#Usable_in_constant_expressions)).

```cpp
const int n = 10;
int array[n];
```

## Argumenti funkcij

```cpp
std::string f(const std::string &s, const int i) {
  ...
}
```

Za vračalni tip ne uporabljamo ```const```, razen če je ta kazalec ali referenca ([link](https://stackoverflow.com/questions/8716330/purpose-of-returning-by-const-value)).

```cpp
int f() { ... }

const int &f() { ... }

const int *f() { ... }
```

Za tipe argumentov ```const``` v deklaraciji nima nobenega učinka, razen če so kazalci ali reference. Dobra praksa je, da ga uporabljamo izključno v definicijah ([link](https://stackoverflow.com/questions/46292490/is-it-better-to-remove-const-in-front-of-primitive-types-used-as-function-pa/46292715)).

```cpp
int f(int);

int f(const int arg) {
  ...
}

int g(const std::string &);

int g(const std::string &) {
  ...
}
```

## Instančne spremenljivke

```cpp
class C {
public:  
  C(int);
private:
  const int value;
};

C::C(const int value) : value(value) { // Inicializiramo jih lahko samo v inicializacijski listi
  
}
```

Uporaba ```const``` instančnih spremenljivk povzroči, da se izbriše:
 * implicitno definiran privzeti konstruktor
 * implicitno definiran dodelitveni operator ([link](https://en.cppreference.com/w/cpp/language/copy_assignment#Deleted_implicitly-declared_copy_assignment_operator))‡
 * implicitno definiran premikalni dodelitveni operator ([link](https://en.cppreference.com/w/cpp/language/move_assignment#Deleted_implicitly-declared_move_assignment_operator))‡
 
 ```cpp
class C {
private:
  const int value;
};

int main() {
  C c; // Ne gre!
}
```

```cpp
class C {
public:
  C(int);
private:
  const int value;
};

C::C(const int value) : value(value) {}

int main() {
  C c1(1);
  C c2(2);
  c1 = c2; // Ne gre!
}
```
 
 ## Metode
 
```cpp
class C {
public:
  std::string f(const std::string &, int);
};

C::f(const std::string &s, const int i) {
  ...
}
```
Vse kar velja za funkcije velja tudi za metode, poleg tega pa imamo še posebno obliko ```const``` metod, ki ne smejo spremeniti stanja objekta (instančnih spremenljivk).
 
```cpp
class C {
public: 
  void set_value(int);
  int get_value() const; // !
private:
  int value;
};
 
void C::set_value(const int value) {
  this->value = value; // Spremeni stanje objekta, ne more biti const
}
 
int C::get_value() const {
  return value; // Ne spremeni stanja, torej const
}
```
 
Pri metodah je ```const``` del signature, torej ga je potrebno napisati pri deklaraciji in definiciji. Prav tako se lahko metode z in brez ```const``` prekrivajo.
 
```cpp
class C {
public:
  void f();
  void f() const;
};
 
void C::f() {}
void C::f() const {}
 
int main() {
 C c1;
 c.f(); // Pokliče se metoda brez const
 
 const C c2;
 c2.f(); // Pokliče se metoda s const
}
```
 
To je uporabno pri operatorjih za dostop do polj ([link](https://en.cppreference.com/w/cpp/language/operators#Array_subscript_operator))‡

## Poglejte še
* [const correctness FAQ](https://isocpp.org/wiki/faq/const-correctness#overview-const)
* [cppreference](https://en.cppreference.com/w/cpp/keyword/const)
* [constexpr](https://en.cppreference.com/w/cpp/language/constexpr)‡

 ‡: Dodatne informacije
