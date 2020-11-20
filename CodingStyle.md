Coding Style
=============

This Coding Style is an attempt to list every programming conventions that I've encounters along my
years of programming. Those rules aren't objectively correct and are susceptible to change.

This Coding style is purely oriented toward readability and explicitness and is therefore strict and
painfull to put into practice.


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



Structures and Classes
----------------------

### Structures vs Classes
A structure should be use only for **passive objects** that carry public data.
Furthermore, data fields must not imply relationship between other fields.
Finaly, only constructors, destructors and helper methods may be present

### Naming convention
Classes' and Structures' names must follow the **PamelCase** convention.

### Access specifier
Access specifiers must be **as indented as the class keyword** and be declared as the following
order: public then protected then private
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


### Constructors
Constructors **must not allow implicit conversions** by default. The usage of explicit keyword is required
for constructors and conversion operators.
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

API Specific
----------------------

### Copyable and Movable types
Every types must have a clear definition whether the the type is copyable,
movable or neither copyable nor movable.
```cpp
class Copyable {
public:
  Copyable(const Copyable& other) = default;
  Copyable& operator=(const Copyable& other) = default;

  // The implicit move operations are suppressed by the declarations above.
};

class MoveOnly {
public:
  MoveOnly(MoveOnly&& other) = default;
  MoveOnly& operator=(MoveOnly&& other) = default;

  // The copy operations are implicitly deleted
  MoveOnly(const MoveOnly&) = delete;
  MoveOnly& operator=(const MoveOnly&) = delete;
};

class NotCopyableNorMovable {
public:
  NotCopyableNorMovable(const NotCopyableNorMovable&) = delete;
  NotCopyableNorMovable& operator=(const NotCopyableNorMovable&) = delete;

  // The move operations are implicitly disabled, but you can
  // spell that out explicitly if you want:
  NotCopyableNorMovable(NotCopyableNorMovable&&) = delete;
  NotCopyableNorMovable& operator=(NotCopyableNorMovable&&) = delete;
};
```

