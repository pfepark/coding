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

##### Tuple for-each

```c++
template<size_t Index, typename Tuple, typename Functor>
auto tuple_at(const Tuple& tpl, const Functor& func) -> void
{
	const auto& v = std::get<Index>(tpl);
	func( v );
}

template<typename Tuple, typename Functor, size_t Index = 0>
auto tuple_for_each(const Tuple& tpl, const Functor& f) -> void
{
	constexpr auto tuple_size = std::tuple_size_v<Tuple>;
	if constexpr(Index < tuple_size) {
		tuple_at<Index>(tpl, f);
		tuple_for_each<Tuple, Functor, Index+i>(tpl, f);
	}
}

// example
auto tpl = std::make_tuple(1, true, std::string{"Jedi"});
tuple_for_earch(tpl, [](const auto& v) { std::cout << v << " "; });
```

##### Tuple any_of

```c++
template<typename Tuple, typename Functor, size_t Index = 0>
auto tuple_any_of(const Tuple& tp1, const Functor& f) -> bool
{
    constexpr auto tuple_size = std::tuple_size_v<Tuple>;
    if constexpr(Index < tuple_size)
    {
        bool success = f(std::get<Index>(tp1));
        return success ? true : tuple_any_of<Tuple, Functor, Index+1>(tp1, f);
    }
    else
    {
        return false;
    }
}
```

