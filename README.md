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
