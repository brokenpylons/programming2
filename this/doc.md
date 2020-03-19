# Kazalec ```this```

C++ je nastal kot predprocesor za C.
Torej se je razred prevedel v nekaj takega (```this``` je rezerviran, zato je namesto tega self):

```cpp
// class Color {

// private
struct Data {
  int r;
  int g;
  int b;
};

// public
Data constructor(const int, const int, const int);
int get_r(Data *);
int get_g(Data *);
int get_b(Data *);

void set_r(Data *, int);
void set_g(Data *, int);
void set_b(Data *, int);

// }

Data constructor(const int r, const int g, const int b) {
  return Data{r, g, b};
}

int get_r(Data *const self) {
  return self->r;
}

int get_g(Data *const self) {
  return self->g;
}

int get_b(Data *const self) {
  return self->b;
}

void set_r(Data *const self, const int r) {
  self->r = r;
}

void set_g(Data *const self, const int g) {
  self->g = g;
}

void set_b(Data *const self, const int b) {
  self->b = b;
}

int main() {
  data color = constructor(0, 0, 0);
  get_r(&color);
  set_r(&color, 255);
}
```



