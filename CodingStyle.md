Coding Style
=============

This Coding Style is an attempt to list every programming conventions that I've encounters along my
years of programming. Those rules aren't objectively correct and are susceptible to change.

This Coding style is purely oriented toward readability and explicitness and is therefore strict and
painfull to put into practice.

Design
----------------------

### Files

- 

-

-

Variables
----------------------

### Common
Every variable must have their name fully explaning their roles but should **not contain verbs**.
```cpp
size_t oldStrSize;
```

Their utility must **not be ambiguous** but shouldn't damage the code clarity. Therefore, if the name
isn't enough to fully explain their role, or if would be too long, they must be preceded by a comment
bringing further explainations.
```cpp
// file that is supposed to contain informations about people that the user is connected to
std::ifstream informationFile;
```


### Non-member or Public member Variables
The Name of Non-member or Public member variables must follow the **camelCase** convention.
```cpp
size_t oldStrSize;
```



### Global or Public member Variables
The Name of global must follow the **PamelCase** convention preceded by **g_**.
```cpp
bool g_IsGameRunning;
```



### Private/Protected member Variables
The Name of member variables must follow the **PamelCase** convention preceded by **m_** to
explicitly separate them from methods' arguments.
```cpp
size_t m_size;
```



Classes
----------------------
### Access specifier
Classes Access specifiers must be **as indented as the class keyword** and be declared as the following order:
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
    Person(const std::string_view name);
    ~Person() = default;

public:
    static constexpr size_t lifeExpectancy { 79 };

private:
    std::string name;
};
```


Design
----------------------
