Coding Style
=============

This Coding Style is an attempt to list every programming conventions that we've encounters along our years of programming. Those rules aren't objectively correct and are susceptible to change.

This Coding style is purely oriented toward readability and explicitness and is therefore strict and painful to put into practice.

Therefore, this coding style isn't written to explain why rules stands like so, but just list them.

Exceptions ...


Variables
----------------------

### Common
Every variable must have their name fully explaning their roles but should **not contain verbs**.
```cpp
size_t oldStrSize;
```

Their utility must **not be ambiguous** but shouldn't damage the code clarity. Therefore, if the name isn't enough to fully explain their role, or if would be too long, they must be preceded by a comment bringing further explainations.
```cpp
// file that is supposed to contain informations about people that the user is connected to
std::ifstream userInformationsFile;
```

### Global variables
The Name of global must follow the **camelCase** convention preceded by **g_**.
```cpp
bool g_isGameRunning;
```

### Non-member and public member Variables
The Name of Non-member or Public member variables must follow the **camelCase** convention.
```cpp
size_t oldStrSize;
```

### Private/Protected member variables
The Name of member variables must follow the **camelCase** convention preceded by **m_** to explicitly separate them from methods' arguments.
```cpp
size_t m_size;
```

### Unused parameters
Unused variables shouldn't have a name.
```cpp
void func(int)
{}
```


Types
----------------------

### Choice
The type choice should be match the variable utility.
```cpp
size_t strLength { str.size() }; // size_t is used as it describe a size
```

### auto
auto should be used as much as possible as it simplify the readability of the code.

### constness
Every const variables must be specified as so. Constexpr should be used as much as possible. When constexpr cannot be applied, const must be used instead.
Remove const qualifier when possible in function declerations.

### References
Order of choice
Const reference: Use to avoid copy or as observer.
Reference: Refers to a not owned thing that will be modified.
Reference wrapper: A copyable and assignable reference.

### Const reference wrapper
Use reference wrapper of const type when possible :
```cpp
std::referencer_wrapper<const TypeName> constReference;
```

### Pointers
smart pointers: Polymorphism, C libraries usage or usage of big objects that need to be on the heap.
raw pointers: Never use.

### Custom deleter of smart pointers
Prefer custom deleter over manual destruction.

### Parameters
When needed, use mutable references. Otherwise, always copy basic types and always pass objects by const references.


Namespaces
----------------------

### Naming convention
Namespaces' names must follow the **camelCase** convention and should mostly be singular. Multiline namespaces must also be closed by a brace followed by a commentary of the closing namespace.
```cpp
namespace space { class Chair; }

namespace space { // OK
    class Table {
    private:
        Chair& chair;
    };
    class Chair {
        ...
    };
} // namespace space
```

Always placing code in namespace.

Never `using namespace` in the global scope and prefer `using` over `using namespace`, always in the smallest scope possible.

Avoid namespace aliases in header files.

Do not include inside namespaces.

Use the namespace detail for internal details that are not visible in the public interface and that should be ignored by external users.

Never declare anything inside namespace `std`.

Unnamed namespaces must be prefered over static functions and variables.

Place variables and functions inside namespaces rather than inside a class they aren't related to or a simple class that only stands for it if it doesn't make sense.

Namespaces shouldn't add indentations.


Structures and Classes
----------------------

### Structures vs Classes
A structure should be use only for **passive objects** that carry public data, which means that access specifiers are not allowed. Furthermore, data fields must not imply relationship between other fields. Finaly, only constructors, destructors and operators should be present.

### Naming convention
Classes' and Structures' names must follow the **PascalCase** convention.

### Single Responsibility Principle (SRP)
The code should be organised as classes and structures represents only **one object** and doesn't mix different levels of abstraction.
Moreover, everyhing that can **should be represented as objects**. For exemple, a fonctionnality shouldn't be managed in multiple files. A specific class should be done.

### Encapsulation
All attributes must be private.
Getters and setters must always be used.

### Access specifier
Access specifiers must be as indented as the class keyword and be declared as the following order: public then protected then private
```cpp
namespace detail { Person }

class Person {
public:
protected:
    std::unique_ptr<detail::Person> m_PersonDetail;
};
```

Access specifiers must be used to seperate variables from methods
```cpp
struct StructName {

    int age;
    
};
```

```cpp
class ClassName {
public:
    explicit ClassName(TypeName variableName);
    ~ClassName() = default;

public:
    static constexpr size_t lifeExpectancy { 79 };



private:
    void privateMethod() const;

private:
    TypeName m_variableName;
};
```


### Constructors
Constructors **must not allow implicit conversions** by default. The usage of explicit keyword is required for constructors and conversion operators.
```cpp
class ImaginaryNumber {
public:
    explicit ImaginaryNumber(int reelPart, int imaginaryPart);
...
    explicit operator int() const;
...
};
```

### Inheritance
Composition is often more appropriate than inheritance. Inheritance must be restricted to an **is a** or **is a kind of** utilisation.

Control structures
----------------------

### If conditional branching
If blocks should contain as less branches as possible as multiple branches harm the code clarity.

### Switch conditional branching
Switches conditional branching should be prefered over multiple if branches when possible. Implicit fallthrough should be avoided. [[ fallthrough ]] attribute stands for that case.

### Nested conditional statements
If a conditional statmement can return, it should do so and no else must be used to use as less depth as possible. Also, Nested conditional braching with a depth of more than 3 should be avoid or splited into several functions.
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
size_t maxStrSize { (str1.size() < str2.size()) ? str1.size() : str2.size() };
```

### Goto
Goto keyword can be used as long as it improve the code clarity. It can especially be usefull when handling errors.
However, labels must not be acceccible without passing through a goto. Labels' name must follow the MACRO_CASE convention.
```cpp
    if (parser.isUnvalidInput) {
        goto ERROR:
    }
...
    return true;
ERROR:
...
    return false;
```

Copy
----------------------

### Move over copy
Move sementics must be used as much as possible.
```cpp
vector.push_back(std::move(str));
```

### NonCopyable and NonMovable
NonCopyable and NonMovable classes must be prefered over manual deleted operators and constructors as it improve readability.
```cpp
class NonCopyable {
public:
    NonCopyable() = default;
    ~NonCopyable() = default;
    
    NonCopyable(const NonCopyable&) = delete;
    NonCopyable& operator=(const NonCopyable&) = delete;
    
    NonCopyable(NonCopyable&&) = default;
    NonCopyable& operator=(NonCopyable&&) = default;
};

class NonMovable {
public:
    NonMovable() = default;
    ~NonMovable() = default;
        
    NonMovable(const NonMovable&) = default;
    NonMovable& operator=(const NonMovable&) = default;
    
    NonMovable(NonMovable&&) = delete;
    NonMovable& operator=(NonMovable&&) = delete;
};
```
```cpp
class Cube : public NonCopyable {
...
}
```

Not classed yet : Headers
----------------------
Every .cpp files should have an associated .hpp file.

Every module files must stand as .cppm.

Every header files should be self-contained and compilable.

Includes guards must be used over #pragma once. The name format must be `___INCLUDE_GUARD_<PATH_FROM_ROOT_DIR>_<EXTENTION>`. Also, the endif must be followed by the name of the define.
```cpp
#ifndef ___INCLUDE_GUARD_SOURCES_HEADER_HPP
#define ___INCLUDE_GUARD_SOURCES_HEADER_HPP
...
#endif // ___INCLUDE_GUARD_SOURCES_HEADER_HPP
```

Includes must be usefull. A useless one must be remove.
An include from a header must not be usefull to another one, but to its self.

In headers, use forward declaration as much as you can.

Inline functions for no reason should be avoid. Small functions like accessors can be defined as inline though.

Includes order: main related file (.hpp for .cpp), C++ standard library headers, C++ externes library headers, C stystem headers, Personal headers. Every categories must be separated by a blank line. Includes must be sort as alphabetical order if possible.

Included filepaths must be relative to the includer.

In cpp, C++ standard library headers must be prefered over C system headers, like `cstdio` over `stdio.h`.


Not classed yet : Variable
----------------------

Place a function's variable in the narrowest scope possible and initialize it in the declaration.

Place a scope if needed ?

Do not declare an unused variable and declare it as close to its first utilisation as possible.

Explicit constructor call must be used to initialize a variable, using brackets :
```cpp
size_t size { 0 };
```

If the variable is an object, do not call its constructor every time it enters the scope
```cpp
Foo f;
for (uint_fast8_t i = 0; i < 100; i++) {
    f.DoSomething(i);
}
```
Avoid static storage duration as much as possible.


Not classed yet : Classes
----------------------

Avoid virtual method calls.

overload operators only if their meaning is obvious and consistent with the built-in operators. For example, `|` operator stands for bitwise- or logical-or, not shell-style pipe.

Data members should be defined as `private` unless they are constants.

Not classed yet : Function
----------------------

Always prefer return values as output parameters except for performance issues.

Prefer throw over error return values.

Prefer small functions, and prefer splitting a long function if it does not harm the program's structure.

Do not overload function if it modifies its logic.

Use trailing return type syntax only if it improves readability.


Not classed yet : C++20
----------------------

Use `std::string_view` as much as possible.

Use 3-way comparison as much as possible

Not classed yet : Others
----------------------


Use friend only when it improves readability or simplifies the code.

Ue noexcept only on usefull cases.

Prefer C++style cast over any other cast formats

Prefer prefix form of ncrement and decrement operators.

Use constexpr as much as possible, and use const if constexpr isn't possible. Do not place const when it doesn't change anything. For example:
```cpp
// Foo.hpp
class Foo {
    void doSomething(int value);
};

// Foo.cpp
void Foo::doSomething(const int value)
{
...
}
```

Use macro only when needed.

Prefer nullptr over NULL and \0 over char(0).

Prefer sizeof(var) over sizeof(type).

prefer typename over class when defining template.

Line should be at most 100 characters long. Indentation must be 4 spaces length and not using tabs.

Floating points must be defined following with `F` :
```cpp
float value { 5.0F };
```

When a boolean expression is longer than the line length, break up the line after the operator and add a level of indentation :
```cpp
if (foo > 5 &&
    bar < 5) {
...
}
```

Do not place parentheses surrounding the return expression except if it improves readability :
```cpp
return true;
```

No extra indentation withing namespaces must be placed.

Avoid trailing spaces.

Avoid recursion

Prefered map for range-based loop syntax :
```cpp
std::map<int, int> map;
for (auto& [key, value] : map) {
}
```

this->functions
m_Varaibles

## OTHER LUUUL

Prefer std::function over function pointers and references

Indentation

I = interface
A = abrastract

copy and move when always construct

allignement, indentation, espacement, headers, include guards, pimpl, namespace (absolu/relatif)

no `= default`
