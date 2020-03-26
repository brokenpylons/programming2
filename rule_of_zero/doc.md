# Pravilo nič

Prevajalnik naslednje operacije generira avtomatsko:
* kopirni konstruktor
* dodelitveni operator ‡
* destruktor
* premikalni konstruktor ‡
* premikalni dodelitveni konstruktor ‡

Lahko jih implementiramo sami, vendar moramo v tem primeru implementirati vseh pet, oz. vsaj prve tri (pravilo petih, treh) ([link](https://en.cppreference.com/w/cpp/language/rule_of_three)) ([link](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c21-if-you-define-or-delete-any-default-operation-define-or-delete-them-all)).
V primeru, da implementiramo samo nekatere, nam preostalih prevajalnik ne generira več (oz. so zastarele).
Te operacije se uporabljajo izključno za razrede, ki so lastniki nekega zunanjega vira (vzorec RAII) ([link](https://en.cppreference.com/w/cpp/language/raii)). ‡
Vsi ostali razredi naj ne implementirajo **nobene** (pravilo nič) ([link](https://en.cppreference.com/w/cpp/language/rule_of_three#rule_of_zero)) ([link](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rc-zero)).

```cpp
class Narobe {
  public:
  ~Narobe() {}
};

int main() {
  Narobe n1;
  Narobe n2(n1); // Zastarelo
  Narobe n3;
  n3 = n1; // Zastarelo
  Narobe n4(Narobe{}); // Kopija namesto premika
  Narobe n5;
  n5 = Narobe(); // Kopija namesto premika
}
````

## CLion Warning

Če želimo, da nas na to CLion opozori, pod:

```
File > Settings > Editor > Inspections > General > Clang-Tidy > Options
```

Dodamo:
```
cppcoreguidelines-*
```

‡: Dodatne informacije
