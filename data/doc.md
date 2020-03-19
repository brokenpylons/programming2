# Simulacija podatov z razredi

```cpp
struct Color {
  int r, g, b;
}
```

# Vzorec 1

```cpp
class Color {
public:
  Color(int, int, int);
  int get_r() const;
  int get_g() const;
  int get_b() const;
private:
  int r, g, b;
}

Color(const int r, const int g , const int b)
  : r(r), g(g), b(b) {}
  
int get_r() const {
  return r;
}

int get_g() const {
  return g;
}

int get_b() const {
  return b;
}
```

# Vzorec 2

```cpp
class Color {
public:
  Color(int, int, int);
  int get_r() const;
  int get_g() const;
  int get_b() const;
  void set_r(int);
  void set_g(int);
  void set_b(int);
private:
  int r, g, b;
}

Color(const int r, const int g , const int b)
  : r(r), g(g), b(b) {}
  
int get_r() const {
  return r;
}

int get_g() const {
  return g;
}

int get_b() const {
  return b;
}

void set_r(const int r) {
  this->r = r;
}

void set_g(const int g) {
  this->g = g;
}

void set_b(const int b) {
  this->b = b;
}
```

# Vzorec 3 

```cpp
class Color {
public:
  Color(int, int, int);
  int get_r() const;
  int get_g() const;
  int get_b() const;
  Color set_r(int);
  Color set_g(int);
  Color set_b(int);
private:
  int r, g, b;
}

Color(const int r, const int g , const int b)
  : r(r), g(g), b(b) {}
  
int get_r() const {
  return r;
}

int get_g() const {
  return g;
}

int get_b() const {
  return b;
}

Color set_r(const int r) {
  return Color(r, this->g, this->b);
}

void set_g(const int g) {
  return Color(this->r, g, this->b);
}

void set_b(const int b) {
  return Color(this->r, this->g, b);
}
```

Primer iz divjine [link](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html).
