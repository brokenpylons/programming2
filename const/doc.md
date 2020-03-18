# Določilo ```const```

Dobra praksa je, da spremenljivke, argumente funkcij in metode označimo kot ```const```, v kolikor se njihova vrednost tekom programa ne spremeni.
Najlažje dosežemo to tako, da na začetku vedno zapišemo vsepovsod ```const```, nato pa kasneje ```const``` odstranimo, če ugotovimo, da dana vrednost ni konstanta.
Razlog za to je, ker nam prevajalnik manjkajočega določila ```const``` ne javi kot napako, v nasptrotnem primeru, ko poskušamo spremeniti vrednost konstante, pa napako javi.
Na tak način določila ```const``` ne moremo pozabiti.

## Lokalne in globalne spremenljivke

```cpp
const int value = 1;
```

## Argumenti funkcij

```cpp
std::string f(const std::string &s, const int i) {
  ...
}
```

Za vračalni tip ne uporabljamo ```const```, razen če je ta kazalec ali referenca ([link](https://stackoverflow.com/questions/8716330/purpose-of-returning-by-const-value))

```cpp
int f() { ... }

const int &f() { ... }

const int *f() { ... }
```

Za osnovne tipe ```const``` v deklaraciji nima nobenega učinka. Dobra praksa je, da ga uporabljamo izključno v definicijah ([link](https://stackoverflow.com/questions/46292490/is-it-better-to-remove-const-in-front-of-primitive-types-used-as-function-pa/46292715))

```cpp
int f(int arg);

int f(const int arg) {
  ...
}
```

## Instančne spremenljivke

```cpp
class C {
  const int value;
  C(int value);
};

C::C(const int value) : value(value) { // Inicializiramo jih lahko samo v inicializacijski listi
  
}
```

Uporaba konstantnih instančnih spremenljivk pomeni:
 * izbrišejo se privzeti dodelitveni operatorji ([link](https://en.cppreference.com/w/cpp/language/copy_assignment), [link](https://en.cppreference.com/w/cpp/language/move_assignment)) ‡
 * premikalni (move) konstruktor mora kopirati instančne spremenljivke, ki imajo ```const```, torej lahko proži izjemo. Posledično ga določeni tipi, npr. ```str::vector``` ne morejo več uporabiti ([link](https://en.cppreference.com/w/cpp/language/move_constructor)) (vpliva na performanco) ‡

Zaradi zgoraj omenjenih pomankljivosi dobro premislimo preden jih uporabimo!
 
 ## Metode
 


