# Parameter passing: Call-by-value vs call-by-reference

## Call-by-value 

  ```cpp
  double average(double a, double b) {...}
  ```

  arguments are copied.

## Call-by-reference: 

  ```cpp
  swap(int &x, int &y) {...}
  ```

  `x` and `y` become aliases for the variables in the calling function. 
  Changing them changes the values there. The purpose is use this
  side effect.
 
  **C++11 Note:** [std::swap()](https://en.cppreference.com/w/cpp/algorithm/swap) is already available in the STL. It can be used together with std::move() to avoid copying large objects. std::move() moves rvalues instead of copying the object. rvalues are temporary objects (e.g., the
 result of an evaluation or a return value) and  
 lvalues are permanent storage (e.g., variables). 
 Typical assignment `lval = rval;`. STL containers (vector, string) 
 use move automatically.


## Call-by-constant reference

  ```cpp
  double average(const Object &a, const Object &b) {...}
  ```

  `a` and `b` become aliases for the variables in the calling function.
  The compiler makes sure that they are not changed. The purpose is to avoid copying large objects when passing them to a 
  function.


## Call-by-pointer (used in C)
    
  ```cpp
  strcpy ( char * destination, const char * source ) {...}
  ```

  copies the string in `source` to `destination`. This is typically not 
  used in C++ unless we deal with C-style arrays which we want to avoid.
  

# Return passing

## Return-by-value

This is the typical way of returning values. C++11 makes returning STL data structures efficient using automatically move semantics.

## Return-by (constant) reference

We can return a reference to an object that exists after the function has finished.

```cpp
const LargeObject & chooseRandomItem(const vector<LargeObject> & arr) {
   return arr[ randomInt(0, arr.size() - 1)) ];
}
```

Without the `const` keywords, the caller of `chooseRandomItem()` would
have modifiable access to the returned object.Note that const references can only be assigned to const references or need to be copied.

Be careful, you can only return references to objects that persist after the function finishes!
In this case `arr` is still in memory after `chooseRandomItem()` finishes.

# Use of the keyword `const`


## Constant Values

```cpp
const int i;
```

## Const pointers

```cpp
const char *text = "test"; // the value pointed to cannot be modified.
char* const text = "test"; // the location pointed to cannot be modified.
```

## Const references

```cpp
int a = 10;
const int &b = a;
```

The referenced value cannot be modified.

## Const member functions

Creates a read-only function that cannot modify the calling object. It is technically an 
overloaded function and `const` becomes part of the signature.

non-`const` objects can also be used to call `const` member functions, but not the other 
way round.

```cpp
class ...
{
private:
  int x;
public:
    int getSomeValue() const
    {
      // this results in a compilation error: 
      // x++; 
      return x;
    }  
    ...
}
```

## Some Rules for `const`

* `const` objects cannot be modified. The compiler enforces this at compile time.
* `const` is a safeguard so the programmer does not make a mistake and changes an object inadvertently. 
* With a `const` object, you can only call a `const` member function. Therefore, we often need to provide also a `const` version of some member functions. The STL uses `const` objects a lot, so your classes should be prepared to provide `const` versions. 
