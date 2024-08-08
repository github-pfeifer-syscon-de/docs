# C++

Some c++ basics:

## C++ 20

from [Cpp talk David Sackstein](https://www.youtube.com/watch?v=9ch7tZN4jeI)

### expected

[expected](https://github.com/TartanLlama/expected)

### generator

[cppcoro](https://gibhub.com/andreasbuhr/cppcoro)

### IOC container (inversion of control)

[hypodermic](https://github.com/ybainier/hypodermic)

no linux :(

## Templates

form [Timur Doumler](https://www.youtube.com/watch?v=xBAduq0RGes)

### Subclass

To instantiate template use in base.cpp

```
// explicit instantiations
template class Base<SubClass>;
```

## Special member functions

from [Kris van Rens](https://www.youtube.com/watch?v=ajRTADPXEko)


| Compiler implicitly declares|
| Userdecl. | Def. ctor | dtor     | copy ctor | copy asign | move ctor | move asign
| --- | --- | --- | --- | --- | --- | --- |
| Nothing   | Deflt     | Deflt    | Deflt     | Deflt      | Deflt     | Deflt |
| Any ctor  |&#129397;<mark>No decl.  </mark>| Deflt    | Deflt     | Deflt      | Deflt     | Deflt |
| Deflt ctor|&#128528;<mark>Userdecl. </mark>| Deflt    | Deflt     | Deflt      | Deflt     | Deflt |
| dtor      | Deflt     |&#128528;<mark>Userdecl.</mark>| Deflt     | Deflt      |&#129397;<mark>No decl.  </mark>|&#129397;<mark>No decl. </mark>|
| Copy ctor |&#129397;<mark>No decl.  </mark>| Deflt    |&#128528;<mark>Userdecl. </mark>| Deflt      |&#129397;<mark>No decl.  </mark>|&#129397;<mark>No decl.</mark>|
| Copy asign| Deflt     | Deflt    | Deflt     |&#128528;<mark>Userdecl.  </mark>|&#129397;<mark>No decl.  </mark>|&#129397;<mark>No decl.</mark>|
| Move ctor |&#129397;<mark>No decl.  </mark>| Deflt    |&#9940;<mark>Deleted   </mark>|&#9940;<mark>Deleted    </mark>|&#128528;<mark>Userdecl. </mark>|&#129397;<mark>No decl.</mark>|
| Move asign| Deflt     | Deflt    |&#9940;<mark>Deleted   </mark>|&#9940;<mark>Deleted    </mark>|&#129397;<mark>No decl.  </mark>|&#128528;<mark>Userdecl.</mark>|

## lvalue, rvalue explaind

from [Ben Saks](https://www.youtube.com/watch?v=XS2JddPq7GQ&t=1683s)

a lvalue is occupying storage (e.g. left side of assignment) 

a rvalue does not need storage (e.g. right side of assignment)

a lvalue references use & operator 

a rvalue references use && operator (used for move, here const is not useful)

## use enable_shared_from_this

from [Arthur O'Dwyer](https://www.youtube.com/watch?v=xGDLkt-jBJ4)


```
#include <memory>
template<class T>
void library_function(std::shared_ptr<T>)
{}
struct A : public std::enable_shared_from_this<A> {
   void operator()()
   {
       library_function(shared_from_this());
   }
};
int main()
{
   auto a = std::make_shared<A>();
   a->operator()();
}
```

# Source location

use std::source_location::current() with:

```
std::ostream& operator<< (std::ostream& os, const std::source_location& location);
```

(for use defined in StringUtils.hpp).

# Minimize constructor overhead

[Nico Josuttis](https://www.youtube.com/watch?v=PNRju6_yn3o)

Against the common expectation pass by value is a good choice:

```
class Cust {
 private:
  std::string first;
  std::string last;
  int id;
 public:
  Cust(std::string f, std::string l ="", int i = 0) 
  : first(std::move(f))
  , last(std::move(l))
  , id(i){
  }
};
```

Or the templated version requires ugly enable_if syntax:

```
class Cust {
 private:
  std::string first;
  std::string last;
  int id;
 public:
  template <typename S1, typename S2, typename = std::enable_if_t<!std::is_convertible_v<S1,Cust>>>
  Cust(S1&& f, S2&&l = "", int i = 0) 
  : first(std::forward<S1>(f))
  , last(std::forward<S2>(l))
  , id(i){
  }
};
```

# Concepts

For C++20

[Andreas Fertig](https://www.youtube.com/watch?v=_FoXWnrGuNU)

# Common knowledge

[Dawid Zalewski](https://www.youtube.com/watch?v=xVgn3VDcqOI&t=272s)

## Uninitalized means indeterminate

```
struct class {
 std::string m_name;
 std::size_t size;
```

Will leave the size at some random value...

Use:
```
struct class {
 std::string m_name{};
 std::size_t size{};
```

## Declaration order means initialization order

```
template <typename T>
struct named_heap {
  named_heap() 
  : capacity{16u}
  , data_min{new T[capacity]}
  , data_max{new T[capacity]}
  {
  }
  T* data_min{};
  T* data_max{};
  capacity;
```

will do something, but not the expected ...

## Assignment is not initialization

```
template <typename T>
struct named_heap : public named_collection_base {
   named_heap(std::string name) 
   {
    named_collection_base::name = std::move(name);
   }
   
```
better call the constructor explicitly, no double work.

## Implicit conversion are almost always evil

```
template <typename T>
struct named_heap : public named_collection_base {
   named_heap(std::string name) 
```

will seen as converting constructor, which allow some stupid conversions.

```
   explicit named_heap(std::string name) 
```

## Everything needs to be defined (just) once

```
 auto kCap = 16zu;
```

placed in header will confuse the linker.

## Only fully constructed object benefit from RAII

```
template <typename T>
struct named_heap {
  named_heap() 
  : capacity{16u}
  , data_min{new T[capacity]}
  , data_max{new T[capacity]}
```

if the first new succeeds and the second fails the first will be freed.

## RAII means: one class one resource

improve the above with C++11:  

```
 , data_min{std::make_unique<T[]>(capacity)}
```

which also saves use to explicitly declare a destructor.

## Member functions have a implicit this parameter

```
class named_object {
  std::string name;
  bool operator!=(const named_object& other) {
     return name != other.name;
  }
};
inline bool operator==(const named_object& a,const named_object& b) {
  return !(a!=b);
} 
```

will cause indefinite recursion, or will not compile, as the instance a is declared as const so the operator!= will not be available if not declared as const.
