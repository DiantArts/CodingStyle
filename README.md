Coding Style
=============

This Coding Style is an attempt to list every programming conventions that we've encounters along our years of programming. Those rules aren't objectively correct and are susceptible to change.

This Coding style is purely oriented toward readability and explicitness and is therefore strict and painful to put into practice.

This coding style isn't written to explain why rules stands like so, but just list them.


Variables
----------------------

### Common
Every variable must have their name fully explaning their roles and should **not contain verbs**.
```cpp
::std::size_t oldStrSize;
```

Their utility must **not be ambiguous** but shouldn't damage the code clarity. Therefore, if the name isn't enough to fully explain their role, or if would be too long, they must be preceded by a comment bringing further explainations.
```cpp
// file that is supposed to contain informations about people that the user is connected to
::std::ifstream userInformationsFile;
```

### Global variables
The Name of global must follow the **camelCase** convention preceded by **g_**.
```cpp
bool g_isGameRunning;
```

### Non-member and public member Variables
The Name of Non-member, Public member or static member variables must follow the **camelCase** convention.
```cpp
::std::size_t oldStrSize;
```

### Private/Protected member variables
The Name of member variables must follow the **camelCase** convention preceded by **m_** to explicitly separate them from methods' arguments.
```cpp
::std::size_t m_size;
```

### Unused parameters
Unused variables should have their name commented.
```cpp
void func(
    const ::std::size_t /* strSize */
)
{}
```


Types
----------------------

### Choice
The type choice should be match the variable utility.
```cpp
::std::size_t strLength{ str.size() }; // size_t is used as it describes a size
```
### auto
auto should be used as much as possible as it simplify the readability of the code.

### constness
Every const variables must be specified as so, except for class members (which are forbiden to be const). Constexpr should be used as much as possible. When constexpr cannot be applied, constinit, consteval and const must be concider instead.
Remove const qualifiers in function declerations when possible, but not in function declerations.

```cpp
class Foo {
    void bar(
        int arg
    );
}

void Foo::bar(
    const int arg
)
{}
```

### Pointers
Use smart pointers for polymorphism, C libraries usage or usage of big objects that need to be on the heap.
Never use raw pointers.
Always prefer custom deleter over manual destruction.

### Parameters
Order of choice
Const reference: Use to avoid copy or as observer.
Mutable Reference: Refers to a not owned thing that will be modified.
Reference wrapper: A copyable and assignable reference.
Use reference wrapper of const type when possible :
```cpp
::std::referencer_wrapper<const TypeName> constReference;
```
When needed, use mutable references. Otherwise, always copy basic types and always pass objects by const references.

### lvale + rvalue overload
When a type is going to get constructed anyway, avoid doing a lvalue + rvalue overload, take the parameter as copy and move it instead.

### Moving over copying
Always prefer moving than copying.



Namespaces
----------------------

### Naming convention
Namespaces' names must follow the **camelCase** convention and should be singular. Multiline namespaces must also be closed by a brace followed by a commentary of the closing namespace. The default regular expression must be `^[a-z][a-z_0-9]*$`
```cpp
namespace space { class Chair; }



namespace space {



    class Table {
    ...        
    };
    
    
    
} // namespace space
```

### Basic rules
Always place code in namespace.
Never use `using namespace` in the global scope and prefer `using` over `using namespace`, always in the smallest scope possible.
Avoid namespace aliases in header files except for common aliases like :
```cpp
using::std::string_literals::operator""s;
```
Never include inside namespaces.
Use the namespace `detail` for internal details that are not visible in the public interface and that should be ignored by external users.
Never declare anything inside namespace `std`.
Unnamed namespaces must be prefered over static functions and variables.
Place variables and functions inside namespaces rather than inside a class they aren't related to or a simple class that only stands for it if it doesn't make sense.
Namespaces shouldn't add indentations.


Structures and Classes
----------------------

### Structures vs Classes
A structure should be use only for **passive objects** that carry public data, which means that access specifiers are not allowed. Furthermore, data fields must not imply relationship between other fields. Therefor, only constructors, destructors and operators should be present.
```cpp
struct StructName {

    ::std::int8_t age;

};
```

### Naming convention
Classes' and Structures' names must follow the **PascalCase** convention.

### Single Responsibility Principle (SRP)
The code should be organised as classes and structures represent only **one object** and should't mix different levels of abstraction.
Moreover, everyhing that can **should be represented as objects**. For exemple, a fonctionnality shouldn't be managed in multiple files. A specific class should be done.

### Encapsulation
All attributes must be private.
Getters and setters must always be used and be named as follow :
```cpp
    [[ nodiscard ]] auto getSize() cont
        -> ::std::size_t
    {
        return m_size;
    }
```

### Methods order
Methods should be ordered as follows: constructors, destructors, copy idiom, move idiom, assignement operators, then grouped by meaning.

### Rule of 0 and rule of 5
Avoid specifying any of the special member functions. If 1 is specified, then specify them all. Destructor and move operators should be noexcept.

### Access specifier
Access specifiers must be as indented like the class keyword and be declared as the following order: public then protected then private
```cpp
namespace detail { Person }



class Person {

public:

...

private:

    ::std::unique_ptr<::detail::Person> m_pimpl;
    
};
```

Access specifiers must be used to seperate variables from methods

```cpp
class ClassName {

public:

    explicit ClassName(
        TypeName variableName
    );



public:

    static inline constexpr TypeName1 publicVariableName{ 0 };



private:
    void privateMethod() const;



private:

    TypeName m_privateMemberName;
    
};
```

### Constructors
Constructors **must not allow implicit conversions** by default. The usage of explicit keyword is required for constructors and conversion operators.
```cpp
class ImaginaryNumber {

public:

    explicit ImaginaryNumber(
        int reelPart,
        int imaginaryPart
    );
    
...

    explicit operator int() const;
    
...

};
```

### Inheritance
Composition is often more appropriate than inheritance. Inheritance must be restricted to an **is a** or **is a kind of** utilisation.

### Unused accessors
Ununsed accessors must not be written.



Control structures
----------------------

### If conditional branching
If blocks should contain as less branches as possible as multiple branches harm the code clarity.

### Switch conditional branching
Switches conditional branching should be prefered over multiple if branches when possible. Implicit fallthrough should be avoided. `[[ fallthrough ]]` attribute stands for that case.

### Early returns
If a conditional statmement can return, it should do so and no else must be used to use as less depth as possible. Also, Nested conditional braching with a depth of more than 3 should be avoid or splited into several functions if possible.
```cpp
    if (...) {
        return true;
    }
...
    return false
```

### Ternary expressions
Ternary expressions must be only used to return values, not to control the program flow, and should not be nested or chained.
```cpp
std::string& longestStr { str1.size() < str2.size() ? str1 : str2 };
```

### Goto
Goto should be avoided, or always use to jump later in the function. It can be use to do multiple breaks but shouldn't be used to jump into blocks.



Not classed yet : Includes
----------------------

Libraries include filepaths should always use the `<>` form.
```cpp
#include <LibName/FileName.hpp>
```

All external functionalities must be explicitly included and not rely on other headers to include them.
An include from a header must not be useful to another one, but to itself.

Includes must be useful. An useless one must be removed.

Includes order: precompiled header, main related file (.hpp for .cpp), C++ standard library headers, C++ personal headers, C system headers, C personal headers. Every category must be separated by a blank line. Includes must be sorted by alphabetical order.



Not classed yet : Headers
----------------------
Every .cpp files should have an associated .hpp file except for main that should contain only the main, and maybe some error managment if needed.

Always use `#pragma once`. For compatibility issues, use `#pragma once` AND Includes guards. The name format must be `INCLUDE_GUARD_<PATH_FROM_ROOT_DIR>_<EXTENTION>`.
Also, the endif must be followed by the name of the define.
```cpp
#ifndef INCLUDE_GUARD_LIGHTLIB_HEADER_HPP
#define INCLUDE_GUARD_LIGHTLIB_HEADER_HPP
...
#endif // INCLUDE_GUARD_LIGHTLIB_HEADER_HPP
```

Every header files should be self-contained and compilable.

In headers, use forward declaration as much as you can.

Inline functions for no reason should be avoided.

`template` and `constexpr` functions should be declared in the `FileName.hpp` and the actual function must be placed in the `FileName.impl.hpp` that is included at the end of the `FileName.hpp` file.

C++ standard library headers must be prefered over C system headers, like `cstring` over `string.h`.


Not classed yet : Modules
----------------------

Every module files must stand as `.mpp`.


Not classed yet : Variable
----------------------

Place a function's variable in the narrowest scope possible and initialize it in the declaration. Create a scope if needed.

Do not declare an unused variable and declare it as close to its first utilisation as possible.

Explicit constructor call must be used to initialize a variable, using brackets :
```cpp
::std::size_t size{ 0 };
```

If the variable is an object, do not call its constructor every time it enters the scope
```cpp
Foo f;
for (auto i{ 0 }; i < 100; ++i) {
    f.doSomething(i);
}
```

Avoid static storage duration as much as possible.


Not classed yet : Classes
----------------------

Avoid virtual methods when no reasons.

overload operators only if their meaning is obvious and consistent with the built-in operators. For example, `|` operator stands for bitwise- or logical-or, not shell-style pipe.

Data members must be defined as `private` unless they are constants.


Not classed yet : Function
----------------------

Always prefer return values to output parameters.

Prefer throw over error return values.

Always prefer small functions, and split long functions if it does not harm the program's structure.

Do not overload functions if it modifies its logic.

Always use the trailing return time as it is way more readable when got use to it


Not classed yet : C++20
----------------------

Use `::std::string_view` and `::std::span` over const references.

Use 3-way comparison as much as possible.


Not classed yet : Format
----------------------

Prototypes of functions always break lines for everything :
```cpp
class Foo {

public:

    struct Bar {
    ...
    };



public:

    // ------------------------------------------------------------------ section1Name

    template <
        typename TypeA
    > [[ nodiscard ]] static constexpr auto func1Name(
        const ::std::string& key,
        int value = 5 // const in the decleration
    ) const
        && -> constTypeA&;

    // func description commentary
    virtual auto func2Name(
        const ::std::string& key,
        int value // const in the decleration
    ) & -> int
        override;
        
    // func description commentary
    virtual auto func3Name(
        concept auto& var,
        const ::std::string& key,
        int value // const in the decleration
    ) const
        -> int
        = 0;

    // func description commentary
    auto func4Name(
        const ::std::string& key,
        auto&&... varArgs
    ) -> const Foo::Bar&;



    // ------------------------------------------------------------------ section2Name

    void setValue(
        int value
    );

    [[ nodiscard ]] auto getValue() const
        -> int;
    


private:

    // ------------------------------------------------------------------ section2Name (same section but seperated by an access specifier)
    
    void printValue() const;
    
    
    
private:

    ::namespace::VarType m_varName{ 0 };
    
    Foo::Bar m_bar;
    
};

template <
    TypeA
> auto Foo::func(
    const std::string& key,
    const TypeA& value
) -> int
{
    return 1;
}
```

< insert explanation > (Always place brackets)
```cpp
if (auto i{ 0 };
    rect1.top <= rect2.top &&
    (rect1.top <= rect2.top &&
        rect1.width >= rect2Middle) &&
    rect2.width >= rect1Middle
) else (
    rect1.top > rect2.top &&
    (rect1.top > rect2.top &&
        rect1.width < rect2Middle) &&
    rect2.width < rect1Middle
) {
    ...
}

if (rect1.top <= rect2.top) {
    ...
}

while (
    rect1.top <= rect2.top &&
    (rect1.top <= rect2.top &&
        rect1.width >= rect2Middle) &&
    rect2.width >= rect1Middle
) {
    ...
}

for (auto i{ 0 };
    rect1.top <= rect2.top &&
    (rect1.top <= rect2.top &&
        rect1.width >= rect2Middle) &&
    rect2.width >= rect1Middle &&
    i < 10;
++i) {
    ...
}

    const auto tmpvar{
        anotherWtfLongFunc(str1.size()) -
        anotherWtfLongFunc(str2.size())
    };
    auto var2{
        func(1),
        func(2) + tmpvar,
        func(3)
    };
    auto var3{ func(1), func(3) };
```

```cpp
static inline const typeName* const
static constexpr/constinit auto var{ 5 };
```

Always use lower case literal suffixes for all literals when directly possible :
```cpp
auto float1{ 5.0f };
auto double1{ 5.0 };
::std::size_t size{ 0 };
auto str{ "hello i'm a str"s };
auto strView{ "hello i'm a strView"sv };
```

Hexadecimal letters must be upper case.
```cpp
int hexNum{ 0xAF3B };

Do not place parentheses surrounding the return expression :
```cpp
return true;
```

Avoid trailing white characters.

Indentation must done using tabs while allignement must be done using spaces.

With a tab width of 4 spaces, a line should be at most 110 columns long.


Not classed yet : Others
----------------------

Always use `override` when overriding. Moreover, always redeclare pure virtual methods of parent classes. Classes destructor must be pure virtual if the class has a
member pure virtual.

Use friend only when it improves readability or simplifies the code.

Use noexcept only on useful cases (like destructors and copy/move idioms.

Prefer C++ style cast over any other cast formats. Always prefer `static_cast`. When using `dynamic_cast`, just think again. Never use `const_cast` and avoid reinterpret_cast.

Prefer prefix form of increment and decrement operators.

Use macro only when absolutely needed.

Always avoid unbounded recursions. Prefer iteration over recursion.

Never use `NULL` and `char(0)`, use nullptr instead.

Always prefer `sizeof var` over `sizeof(type)`.
Place parenthesis around `sizeof(type)` but not around `sizeof var`.

Prefer `typename` over `class` when defining template.

Prefer constraint auto over templates.

Prefer binding structure syntax for range-based loop :
```cpp
std::map<int, int> map;
for (auto& [key, value] : map) {
}
```

Use `this->` for everything that does not start with `m_`.

Avoid ::std::function when possible.

Filename and classname for interfaces and abstracts classes must be as follows :
```cpp
class IClassName; // interfaces
class AClassName; // abstracts
```

Always init variables inside the class decleration instead of inside the constructor member initializer list.

Never use `= default` in header files, use it in the cpp file.

Use `final` where it makes sense.

Always use std types when possible. (`::std::size_t` over `::size_t`)

Namespaces must always be relative to the current class and absolute if not possible

Always use `[[ nodiscard ]]` in the function decleration when it makes sense but not in the expression when seperated.

Avoid using run-time type information.

Use the most accurate type of `int` like `::std::int8_t`

Prefer inline functions, enums, and const variables over marocs.

Use nullptr for pointers, and '\0'.

Use return type deduction (for both functions and lambdas) only for small functions.

Dont use class template arguement deduction.

Use designated initializers only in their C++20-compliant form.
```cpp
    struct Point {
        float x = 0.0f;
        float y = 0.0f;
        float z = 0.0f;
    };

    Point p = {
        .x = 1.0f,
        .y = 2.0f
        // z will be 0.0
    };
```

Use only approved libraries from the Boost library collection.

Only use public aliases if it improves the code readability, and should be clearly documented.

prefer build2 over other build systems, and use build2's guide lines

dont use obvious comments.

Comment non obvious function arguments :
```cpp
message.send(/*isImportant =*/ false);
```

use `TODO` comments.

Dont use blank line when not necessary but seperate things to make it easy on the eyes to catch what goes with what.

OTHERS
----------------------

### Avoid `memset` to delete sensitive data
use memset_s instead.

### ::std::scoped_lock
Should be created with constructor arguments.

### Object slice
Objects should not be sliced, instead, use polymorphism and smart pointers.

### Mutexes
Should be unlocked in the reverse order they were locked

### case ranges
Should cover multiple values
```cpp
switch (i) {
  case 0:
    //...
    break;
  case 1 ... 2:
    //...
    break;
  case 3:
    //...
    break;
}
```

Comparison and assignment operators should not be virtual.

Child class fields should not shadow parent class fields

`::std::source_location` should be used instead of `__FILE__`, `__LINE__`, and `__func__` macros

`::std::jthread` should be used instead of `::std::thread`

`auto` should be used for non-type template parameters
```cpp
template <
    auto func
> call()
{
    func();
}
```

`as_const`  should be used instead of `const_cast`

`::std::move` should be used on rvalue references.

use `::std::bit_cast` instead of `reinterpret_cast`.

Digit seperators should be used
```cpp
long decimal_int_value{ 5'543'124 };
```

Constants should come first in equality tests.
```cpp
if (constant == var)
```
