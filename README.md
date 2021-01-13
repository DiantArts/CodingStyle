Coding Style
=============

This Coding Style is an attempt to list every programming conventions that we've encounters along our years of programming. Those rules aren't objectively correct and are susceptible to change.

This Coding style is purely oriented toward readability and explicitness and is therefore strict and painfull to put into practice.

Therefore, this conding style isn't written to explain why rules stands like so, but just list them.


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
The Name of global must follow the **PamelCase** convention preceded by **g_**.
```cpp
bool g_IsGameRunning;
```

### Non-member and public member Variables
The Name of Non-member or Public member variables must follow the **camelCase** convention.
```cpp
size_t oldStrSize;
```

### Private/Protected member variables
The Name of member variables must follow the **PamelCase** convention preceded by **m_** to explicitly separate them from methods' arguments.
```cpp
size_t m_size;
```

### Unused variables
Unused variables shouldn't have a name.


Types
----------------------

### Choice
The type choice should be match the variable utility.
```cpp
size_t strLength { str.size() }; // size_t is used as it describe a size
for (uint_fast8_t i { 0 }; i < 10; i++) { ... } // as i is in range of [0, 10], uint8 is the best match.
```

### cstdint
the usage of cstdint must be prioritized over a simple int as it allow the user to define clearly the variable range.
(u)int_fastX_t and (u)int_leastX_t must be prioritized over (u)intX_t as it let the compiler chose the actual used type.

### auto
auto should be used as much as possible as it simplify the readability of the code.

Namespaces
----------------------

### Naming convention
Namespaces' names must follow the **camelCase** convention. Furthermore, its name should mostly be singular as it should allow the user to access the class as he is declaring a name, not accessing a group of name. Namespaces must also be closed by a brace followed by a commentary of the closing namespace
```cpp
namespace space { // OK
    class Table {
        ...
    };
    class Chair {
        ...
    };
} // namespace space
```


Structures and Classes
----------------------

### Structures vs Classes
A structure should be use only for **passive objects** that carry public data. Furthermore, data fields must not imply relationship between other fields. Finaly, only constructors, destructors and helper methods may be present


### Naming convention
Classes' and Structures' names must follow the **PamelCase** convention.


### Single Responsibility Principle (SRP)
The code should be organised as classes and structures represents only **one object** and doesn't mix different levels of abstraction.
Moreover, everyhing that can **should be represented as objects**. For exemple, a fonctionnality shouldn't be managed in multiple files. A specific class should be done.


### Access specifier
Access specifiers must be **as indented as the class keyword** and be declared as the following order: public then protected then private
```cpp
class Person {
public:
protected:
private:
};
```

Access specifiers must be used to seperate variables from methods
```cpp
class Person {
public:
    explicit Person(std::string_view name);
    ~Person() = default;

public:
    static constexpr size_t lifeExpectancy { 79 };

private:
    std::string name;
};
```
Access specifiers for methods must be placed before variables ones
```cpp
class Person {
// methods
public:
protected:
private:

// variables
public:
protected:
private:
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

Copy and Move
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

Not classed yet : Namespaces
----------------------

Always placing code in namespace.

Avoid `using namespace`.

Avoid namespace aliases, at least in header files.

Terminate multiline namespaces with comment:
```
namespace foo {
...
} // namespace foo
```

Do not include inside namespaces.

Use the namespace detail for internal details that are not visible in the public interface and that should be ignored by external users.

Never declare anything inside namespace `std`.

Unnamed namespaces must be prefered over static functions and variables

Place variables and function inside namespaces rather than inside a class they aren't related to or a simple class that only stands for it if it doesn't make sense.

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

Prefer smart pointers over raw pointers.

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
