# coding
coding tips

##### clamp
```c++
int clamp(int x, int low, int high) {
  assert(low <= high);
  return std::min(std::max(x, low), high);
}
```

##### wrap around
```c++
if (x >= 10) {
  x = 0;
}
x = x % 10


if (x >= 10) {
  x = 0;
} else if (x < 0) {
  x = 9;
}
int wrap(int x, int low, int high) {
  assert(low < high);
  const int n = (x - low) % (high - low);
  return (n >= 0) ? (n + low) : (n + high);
}
```

##### Range class
```c++
template<typename T>
class Range {
public:
  Range(T min, T max) : min(_min), max(_max) {
    assert(min_ <= max_);
  }
  
  bool isInside(T value) const {
    return (min_ <= value) && (value <= max_);
  }
  
  T clamp(T value) const {
    return ::clamp(value, min_, max_);
  }
  
  T wrap(T value) const {
    return ::wrap(value, min_, max_);
  }
  
  T min() const {
    return min_;
  }
  
  T max() const {
    return max_;
  }
  
private:
  T min_;
  T max_;
}
```
