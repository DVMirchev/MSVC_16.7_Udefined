# MSVC_16.7_Udefined
error C2065: 't': undeclared identifier with /std:c++latest, works with /std:c++17
```
#include <iostream>
#include <tuple>

template<typename Callable, typename... Args>
decltype(auto)
Boind(Callable&& f, Args&&... args)
{
   return
      [f = std::forward<Callable>(f),
       t = std::make_tuple(std::forward<Args>(args)...)](
         auto&&... args) mutable
      {
         // compile error here with /std:c++latest
         //
         // error C2065: 't': undeclared identifier
         //
         // works with /std:c++17
         std::tuple_size<decltype(t)>::value;

         return std::apply(
            f,
            t); // <- No apparent problem here
      };
}

auto
alabala(int a, int b) {
   return a + b;
}

int
main() {
    auto c = Boind(alabala, 1, 2)();
    std::cout << c;
}
```
