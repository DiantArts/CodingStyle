Coding Style
=============

This Coding Style is an attempt to list every programming conventions that I've encounters along my years of programming. Those rules aren't objectively correct and are susceptible to change.

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

### Global or Public member Variables
The Name of global must follow the **PamelCase** convention preceded by **g_**.
```cpp
bool g_IsGameRunning;
```

### Non-member
The Name of Non-member or Public member variables must follow the **camelCase** convention.
```cpp
size_t oldStrSize;
```

### Private/Protected member Variables
The Name of member variables must follow the **PamelCase** convention preceded by **m_** to explicitly separate them from methods' arguments.
```cpp
size_t m_size;
```

### Unused variables
Unused variables must be marked as [[ unused ]], [[ maybe_unused ]] or they shouldn't have a given name.


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
namespace object { // OK
    Table table;
    Chair chair;
} // namespace object


namespace objects { // KO
    Table table;
    Chair chair;
}
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
    explicit Person(const std::string_view name);
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
    explicit ImaginaryNumber(const int reelPart, const int imaginaryPart);
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

### Move as much as possible
Move sementics must be used as much as possible
```cpp
vector.push_back(std::move(str));
```

### NonCopyable and NonCopyableMovable
NonCopyable and NonCopyableMovable classes must be prefered over manual deleted operators and constructors as it improve readability
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

class Cube : public NonCopyable {
...
}

class NonCopyableMovable {
public:
    NonCopyableMovable() = default;
    ~NonCopyableMovable() = default;
    
    NonCopyableMovable(const NonCopyableMovable&) = delete;
    NonCopyableMovable& operator=(const NonCopyableMovable&) = delete;
    
    NonCopyableMovable(NonCopyableMovable&&) = delete;
    NonCopyableMovable& operator=(NonCopyableMovable&&) = delete;
};

class Cube : public NonCopyable {
...
}
```
