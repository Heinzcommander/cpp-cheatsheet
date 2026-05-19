<a href="https://github.com/mortennobel/cpp-cheatsheet"><img align="right" src="https://camo.githubusercontent.com/38ef81f8aca64bb9a64448d0d70f1308ef5341ab/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f6461726b626c75655f3132313632312e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_darkblue_121621.png"></a>

# C++ QUICK REFERENCE / C++ CHEATSHEET
Based on <a href="http://www.pa.msu.edu/~duxbury/courses/phy480/Cpp_refcard.pdf">Phillip M. Duxbury's C++ Cheatsheet</a> and edited by Morten Nobel-Jørgensen.
The cheatsheet focus is both on the language as well as common classes from the standard library.
C++11 additions is inspired by <a href="https://isocpp.org/blog/2012/12/c11-a-cheat-sheet-alex-sinyakov">ISOCPP.org C++11 Cheatsheet</a>).

The goal is to give a concise overview of basic, modern C++ (C++14).

The document is hosted on https://github.com/mortennobel/cpp-cheatsheet. Any comments and feedback are appreciated.

## Preprocessor
```cpp
                            // Comment to end of line
                            /* Multi-line comment */
#include  <stdio.h>         // Insert standard header file
#include "myfile.h"         // Insert file in current directory

#define X some text         // Replace X with some text
#define F(a,b) a+b          // Replace F(1,2) with 1+2
#define X \
 some text                  // Multiline definition
// - don't use #define directives for macros or type aliases, because the debugging of runtime code is worst
// - Modern C++ recommend using for getting similar advantages

#undef X                    // Remove definition
#if defined(X)              // Conditional compilation (#ifdef X)
#else                       // Optional (#ifndef X or #if !defined(X))
#endif                      // Required after #if, #ifdef
```

## Aliases
```cpp
#define int_vec std::vector<int>        // Old-style macro
using int_vec = std::vector<int>;       // Modern C++

#define BUFFER_SIZE 1024                // old
constexpr std::size_t BufferSize = 1024;// Modern

using Friends = std::map<std::string, std::pair<std::int32_t, std::int32_t>>
```

## Literals
```cpp
255, 0377, 0xff             // Integers (decimal, octal, hex)
2147483647L, 0x7fffffffl    // Long (32-bit) integers
123.0, 1.23e2               // double (real) numbers
'a', '\141', '\x61'         // Character (literal, octal, hex)
'\n', '\\', '\'', '\"'      // Newline, backslash, single quote, double quote
"string\n"                  // Array of characters ending with newline and \0
"hello" "world"             // Concatenated strings
true, false                 // bool constants 1 and 0
nullptr                     // Pointer type with the address of 0
```

## Declarations
```cpp
int x;                      // Declare x to be an integer (value undefined)
int x=255;                  // Declare and initialize x to 255
short s; long l;            // Usually 16 or 32 bit integer (int may be either)
char c='a';                 // Usually 8 bit character
unsigned char u=255;
signed char s=-1;           // char might be either
unsigned long x =
  0xffffffffL;              // short, int, long are signed
float f; double d;          // Single or double precision real (never unsigned)
bool b=true;                // true or false, may also use int (1 or 0)
int a, b, c;                // Multiple declarations
int a[10];                  // Array of 10 ints (a[0] through a[9])
int a[]={0,1,2};            // Initialized array (or a[3]={0,1,2}; )
int a[2][2]={{1,2},{4,5}};  // Array of array of ints
char s[]="hello";           // String (6 elements including '\0')
std::string s = "Hello"     // Creates string object with value "Hello"
std::string s = R"(Hello
World)";                    // Creates string object with value "Hello\nWorld"
int* p;                     // p is a pointer to (address of) int
char* s="hello";            // s points to unnamed array containing "hello"
void* p=nullptr;            // Address of untyped memory (nullptr is 0)
int& r=x;                   // r is a reference to (alias of) int x
enum weekend {SAT,SUN};     // weekend is a type with values SAT and SUN
enum weekend day;           // day is a variable of type weekend
enum weekend{SAT=0,SUN=1};  // Explicit representation as int
enum {SAT,SUN} day;         // Anonymous enum
enum class Color {Red,Blue};// Color is a strict type with values Red and Blue
Color x = Color::Red;       // Assign Color x to red
typedef String char*;       // String s; means char* s;
const int c=3;              // Constants must be initialized, cannot assign to
const int* p=a;             // Contents of p (elements of a) are constant
int* const p=a;             // p (but not contents) are constant
const int* const p=a;       // Both p and its contents are constant
const int& cr=x;            // cr cannot be assigned to change x
int8_t,uint8_t,int16_t,
uint16_t,int32_t,uint32_t,
int64_t,uint64_t            // Fixed length standard types
auto it = m.begin();        // Declares it to the result of m.begin()
auto const param = config["param"];
                            // Declares it to the const result
auto& s = singleton::instance();
                            // Declares it to a reference of the result
```

## Initialization with () or {}
- Recommend is in general the using of { }
- only if the call of a specific constructor is desired then is ( ) to use (e.g. std::vector)
```cpp
// { } to use for varibale initialisation
int a{};                        // 0-init (value-initialization)
int b{5};                       // list-initialization

struct S {int a, b;};
S s{ .a = 1, .b = 2};           // struct value init
std::vector<int> v{1, 2, 3};    // init-lists for 1D array

// ( ) to use when: call of std::initializer_list-Constructor should be avoid
std::vector<int> v1(5, 1);     // 5 Elemente, alle 1
std::vector<int> v2{5, 1};     // 2 Elemente: {5, 1}
std::vector<std::vector<uint32_t>> 
    my_matrix(5, std::vector<uint32_t>(4));  // 2D array with 5 rows and 4 columns

// comparable
std::map<std::string, uint32_t> my_map1{};   // empty map
std::map<std::string, uint32_t> my_map2();   // most vaxing parse error - function declaration instead of map object
```

## STORAGE Classes
```cpp
int x;                      // Auto (memory exists only while in scope)
static int x;               // Global lifetime even if local scope
extern int x;               // Information only, declared elsewhere
```

## Statements
```cpp
x=y;                        // Every expression is a statement
int x;                      // Declarations are statements
;                           // Empty statement
{                           // A block is a single statement
    int x;                  // Scope of x is from declaration to end of block
}
if (x) a;                   // If x is true (not 0), evaluate a
else if (y) b;              // If not x and y (optional, may be repeated)
else c;                     // If not x and not y (optional)

while (x) a;                // Repeat 0 or more times while x is true

for (x; y; z) a;            // Equivalent to: x; while(y) {a; z;}

for (x : y) a;              // Range-based for loop e.g.
                            // for (auto& x in someList) x.y();

do a; while (x);            // Equivalent to: a; while(x) a;

switch (x) {                // x must be int
    case X1: a;             // If x == X1 (must be a const), jump here
    case X2: b;             // Else if x == X2, jump here
    default: c;             // Else jump here (optional)
}
break;                      // Jump out of while, do, or for loop, or switch
continue;                   // Jump to bottom of while, do, or for loop
return x;                   // Return x from function to caller
try { a; }
catch (T t) { b; }          // If a throws a T, then jump here
catch (...) { c; }          // If a throws something else, jump here
```

## Functions
```cpp
int f(int x, int y);        // f is a function taking 2 ints and returning int
void f();                   // f is a procedure taking no arguments
void f(int a=0);            // f() is equivalent to f(0)
f();                        // Default return type is int
inline f();                 // Optimize for speed
f() { statements; }         // Function definition (must be global)
T operator+(T x, T y);      // a+b (if type T) calls operator+(a, b)
T operator-(T x);           // -a calls function operator-(a)
T operator++(int);          // postfix ++ or -- (parameter ignored)
extern "C" {void f();}      // f() was compiled in C
```

Function parameters and return values may be of any type. A function must either be declared or defined before
it is used. It may be declared first and defined later. Every program consists of a set of a set of global variable
declarations and a set of function definitions (possibly in separate files), one of which must be:
```cpp
int main()  { statements... }     // or
int main(int argc, char* argv[]) { statements... }
```

`argv` is an array of `argc` strings from the command line.
By convention, `main` returns status `0` if successful, `1` or higher for errors.

Functions with different parameters may have the same name (overloading). Operators except `::` `.` `.*` `?:` may be overloaded.
Precedence order is not affected. New operators may not be created.

## Expressions
Operators are grouped by precedence, highest first. Unary operators and assignment evaluate right to left. All
others are left to right. Precedence does not affect order of evaluation, which is undefined. There are no run time
checks for arrays out of bounds, invalid pointers, etc.

```cpp
T::X                        // Name X defined in class T
N::X                        // Name X defined in namespace N
::X                         // Global name X

t.x                         // Member x of struct or class t
p-> x                       // Member x of struct or class pointed to by p
a[i]                        // i'th element of array a
f(x,y)                      // Call to function f with arguments x and y
T(x,y)                      // Object of class T initialized with x and y
x++                         // Add 1 to x, evaluates to original x (postfix)
x--                         // Subtract 1 from x, evaluates to original x
typeid(x)                   // Type of x
typeid(T)                   // Equals typeid(x) if x is a T
dynamic_cast< T>(x)         // Converts x to a T, checked at run time.
static_cast< T>(x)          // Converts x to a T, not checked
reinterpret_cast< T>(x)     // Interpret bits of x as a T
const_cast< T>(x)           // Converts x to same type T but not const

sizeof x                    // Number of bytes used to represent object x
sizeof(T)                   // Number of bytes to represent type T
++x                         // Add 1 to x, evaluates to new value (prefix)
--x                         // Subtract 1 from x, evaluates to new value
~x                          // Bitwise complement of x
!x                          // true if x is 0, else false (1 or 0 in C)
-x                          // Unary minus
+x                          // Unary plus (default)
&x                          // Address of x
*p                          // Contents of address p (*&x equals x)
new T                       // Address of newly allocated T object
new T(x, y)                 // Address of a T initialized with x, y
new T[x]                    // Address of allocated n-element array of T
delete p                    // Destroy and free object at address p
delete[] p                  // Destroy and free array of objects at p
(T) x                       // Convert x to T (obsolete, use .._cast<T>(x))

x * y                       // Multiply
x / y                       // Divide (integers round toward 0)
x % y                       // Modulo (result has sign of x)

x + y                       // Add, or \&x[y]
x - y                       // Subtract, or number of elements from *x to *y
x << y                      // x shifted y bits to left (x * pow(2, y))
x >> y                      // x shifted y bits to right (x / pow(2, y))

x < y                       // Less than
x <= y                      // Less than or equal to
x > y                       // Greater than
x >= y                      // Greater than or equal to

x & y                       // Bitwise and (3 & 6 is 2)
x ^ y                       // Bitwise exclusive or (3 ^ 6 is 5)
x | y                       // Bitwise or (3 | 6 is 7)
x && y                      // x and then y (evaluates y only if x (not 0))
x || y                      // x or else y (evaluates y only if x is false (0))
x = y                       // Assign y to x, returns new value of x
x += y                      // x = x + y, also -= *= /= <<= >>= &= |= ^=
x ? y : z                   // y if x is true (nonzero), else z
throw x                     // Throw exception, aborts if not caught
x , y                       // evaluates x and y, returns y (seldom used)
```

## Classes
```cpp
class T {                       // A new type
  public:                       // Accessible to all   
    T(): x(1) {}                // Constructor with initialization list
    explicit T(int a);          // Allow t=T(3) but not t=3
    virtual ~T();               // Destructor (automatic cleanup routine) make it always virtual,
                                // for more flexible and secure code in connection with inheritance
                                // and 'new' operator of these objects 

    T(float x) : T((int)x) {}   // Delegate constructor to T(int)
    T(const T& t): x(t.x) {}    // Copy constructor

    T& operator=(const T& t)    // Assignment operator
      {x=t.x; return *this; }

    operator int() const        // operator int() enables conversion of a class object to int type
      {return x;}               // Allows conversion of this class type to int-type int(t)

    int operator+(int y)            // t+y means t.operator+(y)
      { return x + y;};       
    int operator-()                 // -t means t.operator-() 
      { return x * (-1)};           

    T operator+(const T &rhs) const  // new = this + other -> lhs = this + rhs
      { return this.x += rhs.x };

    T operator+=(const T &rhs)   // new = this + other -> lhs = this + rhs
      {   this.x += rhs.x;
          return *this };

    void f();                   // Member function
    void g() {return;}          // Inline member function
    void h() const;             // Does not modify any data members

    friend void i();            // Global function i() has private access
    friend class U;             // Members of class U have private access

    static int y;               // Data shared by all T objects
    static int l();            // Shared code.  May access y but not x. No access to instance members.

    class Z {};                 // Nested class T::Z
    typedef int V;              // T::V means int

  private:                      // Section accessible only to T's member functions
    int x;                      // Member data
  protected:                    // Also accessible to classes derived from T
};

/*- code area     ---------------------------*/
void T::f() {                 // Code for member function f of class T
    this->x = x;}             // this is address of self (means x=x;)
int T::y = 2;                 // Initialization of static member (required)

int T::l() { ...; }           // Implement static methode outside of class declaration
auto k = T::l();              // Call to static member

T t;                          // Create object t implicit call constructor
t.f();                        // Call method f on object t

struct T {                    // Equivalent to: class T { public:
  virtual void i();           // May be overridden at run time by derived class
  virtual void g()=0; };      // Must be overridden (pure virtual)
                              // --> Abstract class has at least one pure virtual method
                              // --> Abstract class can't be instantiate, they can only be derived

/*- Inheritance  ----------------------------------------*/
class U: public T {           // Derived class U inherits all members of base T
  public:
  void g(int) override; };    // Override method g

class V: private T {};        // Inherited members of T become private
class W: public T, public U {};
                              // Multiple inheritance
class X: public virtual T {};
                              // Classes derived from X have base T directly

/*- Reference as return value ---------------------------*/
class Beispiel {
    int wert;
public:
    int& getWert() { return wert; }           // Non-const: modizierbar
    const int& getWertConst() const { return wert; }  // Const: nur lesbar [web:10]
    
    Beispiel& setWert(int w) {                // Chainable Setter
        wert = w;
        return *this;
    }
};

// Verwenden Sie const& für reine Getter, um versehentliche Modifikationen zu verhindern

/*- Intro polymorphism ---------------------------*/
class Base {
  public: // is at least one methode in the parent class virtual, so it is necessary to define the destructor as virtual
          // that warrant the proper calling of destructor at the using with inheritance and memory managements functions like 'new'
          virtual ~Base(){};                           
          void print_data_no_virtual()
                  { std::cout << "Base" << std::endl };
          virtual void print_data_virtual()             // may be overridden at run time by derived
                  { std::cout << "Base" << std::endl };
};

class Derived : public Base {
  public: void print_Data_no_virtual()
                  { std::cout << "Derived" << std::endl };
          void print_data_virtual() override             // overridden methode of Base
                  { std::cout << "Derived" << std::endl };
};

/*- calling ---------------------------*/
Base oBase{};
Derived oDerived1{}:

// polymorphism is only working on pointer types
const std::vector< Base * > vecClasses{&oBase, &oDerived1};

for(const auto row : vecClasses) {
  row->print_Data_no_virtual();
  row->print_Data_virtual();
}

/*- output ---------------------------*/
Base    // methode of Base class is called
Base    // methode of Base class is called, because the vector is used the type of 'Base *'
Base    // pointer to Base is used
Derived // pointer of Derived is using the internal methode

/*- working with const ---------------------------*/
Class CMatrix
{
public:
    CMatrix() = delete;
    CMatrix(const double &A, const double &B);
    ~CMatrix() = default;
}

=> calling
const CMatrix oMatrix2{1.0, 2.0, 3.0, 4.0};    // make sure that instance of class is also const
const auto oMatrix3 = CMatrix{1.0, 2.0, 3.0, 4.0};

/*- important keywords ---------------------------*/
class UniqueFile {
public:
  UniqueFile() = default;        // compiler use default implementation of constructor
  virtual ~UniqueFile() = default;

  UniqueFile(const UniqueFile&) = delete;             // copy forbidden 
  UniqueFile& operator=(const UniqueFile&) = delete;  // copy forbidden
};

class MyClass {
public:
    // 1. User‑declared default ctor, but use compiler’s default implementation
    MyClass() = default;

    // 2. Explicitly ask compiler to generate default destructor
    ~MyClass() = default;

    // 3. delete copy constructor
    MyClass(const MyClass&) = delete;

    // 4. delete copy assignment
    MyClass& operator=(const MyClass&) = delete;
};

class Base {
public:
    virtual void f1() { /* ... */ }
    virtual void f2() final { /* ... */ }          // cannot be overridden
};

class Derived : public Base {
public:
    void f1() override { /* ... */ }              // OK: overrides Base::f1
    // void f2() override { ... }                 // Error: f2 is final

    // Non‑copyable by default
    Derived(const Derived&) = delete;
    Derived& operator=(const Derived&) = delete;

    Derived() = default;                          // default ctor
    ~Derived() override = default;                // virtual dtor, default body
};

/*
Default-Konstruktor:
Derived() = default; erzeugt den Standardkonstruktor automatisch durch den Compiler. Dieser initialisiert die
Basisklasse und alle Mitglieder mit ihren Default-Werten – ohne benutzerdefinierten Code.

Virtueller Destruktor:
~Derived() override = default; überschreibt den virtuellen Destruktor der Basisklasse explizit und lässt den
Compiler die Standardimplementierung generieren. Das override stellt sicher, dass die Basisklasse tatsächlich
einen virtuellen Destruktor hat, und verhindert Fehler bei fehlender Virtualität.

Warum diese Kombination?
Bei Polymorphie (Zeiger/Referenzen auf Basisklasse) wird so korrekte Zerstörung der Derived-Objekte garantiert,
ohne manuellen Code. Der Compiler kümmert sich um die korrekte Reihenfolge: Derived-Mitglieder → Derived-Basis → Base-Destruktor

Warum virtual nötig sein kann:
Wenn deine Klasse als Basisklasse dient und Polymorphie (z. B. Löschen über Basisklassen-Zeiger) geplant ist, musst du den
Destruktor explizit als virtual ~Base() = default; deklarieren. Ohne virtual wird bei delete basePtr; (wo basePtr auf ein
Derived-Objekt zeigt) nur der Basisklassen-Destruktor aufgerufen – Derived-Teile bleiben unzerstört, was zu Memory Leaks oder
Undefined Behavior führt

virtual ~Base() = default; mit virtual ist perfekt okay und empfohlen – es signalisiert "trivialer Destruktor, aber polymorph".
Bei Klassen ohne Vererbung oder wenn du nie über Basisptr löschst, ist non-virtual fine (spart minimal Overhead). Aber bei jeder
potenziell polymorphen Base: immer virtual ~() = default;
*/
```

All classes have a default copy constructor, assignment operator, and destructor, which perform the
corresponding operations on each data member and each base class as shown above. There is also a default no-argument
constructor (required to create arrays) if the class has no constructors. Constructors, assignment, and
destructors do not inherit.

## Templates
declaration and definition only in header!
```cpp
template <typename T>
void print_vector(std::vector<T> &vec)
{
    for (const auto &row : vec) { std::cout << row << "\n"; }
} 

template <typename T, std::size_t N>
void print_array(std::array<T, N> &arr)
{
    for (const auto &row : arr) { std::cout << row << "\n"; }
} 

template <typename T>
void print_min_max()
{
    std::cout << "min =" << std::numeric_limits<T>::min() << "\n";
    std::cout << "max =" << std::numeric_limits<T>::max() << "\n";
}
// --> call
print_min_max<uint32_t>();

template <class T> T f(T t);// Overload f for all types
template <class T> class X {// Class with type parameter T
  X(T t); };                // A constructor
template <class T> X<T>::X(T t) {}
                            // Definition of constructor
X<int> x(3);                // An object of type "X of int"
template <class T, class U=T, int n=0>
                            // Template with default parameters
```

## Structured bindings (C++17) 
- for unpack tuples, structs, array or pairs into named variables
```cpp
Point p{3, 4};
auto [px, py] = p;  // Unpacks struct public members
std::cout << "x: " << px << ", y: " << py << std::endl;  // Outputs: x: 3, y: 4

auto myTuple = std::make_tuple(1, "hello", 3.14);
auto [a, b, c] = myTuple;
std::cout << a << " " << b << " " << c << std::endl;  // Outputs: 1 hello 3.14

std::map<std::string, int> scores = {{"Alice", 95}, {"Bob", 87}};
for (auto& [name, score] : scores) {
  std::cout << name << ": " << score << std::endl;
}
```

## Namespaces
```cpp
namespace N {class T {};}   // Hide name T
N::T t;                     // Use name T in namespace N
using namespace N;          // Make T visible without N::
namespace {
  constexpr uint32_t X{5}   // anonymous namespace with variable with local scope 
}
namespase fs = std::filesystem;  // use namespace filesystem with alias fs
```

## Lambda 
- anonymous functions that can be used locally
```cpp
//transform
// applies the given function to the elements of the given input range(s), 
// and stores the result in an output range starting from 
std::transform( vec.begin(), 
                vec.end(), 
                vecResult.begin(),
                // Lambda function for transform task - check even
                [](const int val){return (val % 2) ? true : false; });

// or
auto lambda = [](const int val){return (val % 2) ? true : false; };
std::transform( vec.begin(), 
                vec.end(), 
                vecResult.begin(), 
                lambda);  // Lambda function for transform task

// erase of elements within vector
vec.erase(  std::remove_if( vec.begin(), 
                            vec.end(), 
                            [](const auto val){return (val > 5);}),
            vec.end());
// - std::remove_if: removes all elements from the input range [Itrfirst, Itrlast) that 
//   satisfy the criteria of the unäre function (lambda) to a new range at the end
// - it is return a iterator to the new range that is followed by all values that haven't
//   satisfy the function expression

// erase elements within string with std::remove_if and lambda function
std::string str2 = "Jumped\n Over\tA\vLazy \t  Fox\r\n";
str2.erase( std::remove_if(str2.begin(), 
                           str2.end(),
                           [](unsigned char x) { return std::isspace(x); }),
            str2.end());
--> str2 is now "JumpedOverALazyFox"

// lambda as local function
auto lambda_NoEven = [](std::vector<int> vec)
{
  for (const auto &val : vec)
  {
    if (val % 2 == 0)
      return false;
  }
    return true;
};
bool has_no_even = lambda_NoEven(my_vector2);

/*- Lambda with extern variable access --------------------*/
// the capture clausel enable lambda expression the access to external variables
// in general the lambda expression has no access to variables outside the function
// the acces is explicity to define

int x = 10;
auto by_value  = [x]() { x = 20; };      // x in lambda is only a copy
auto by_ref    = [&x]() { x = 20; };     // x in lambda is a refernce to the real object

int a = 1, b = 2;
auto lamb1 = [=]() { std::cout << a + b; };   // a,b by value
auto lamb2 = [&]() { a = 3; b = 4; };         // a,b by refernce

int a = 10, b = 20, c = 30;

auto lambda = [&, a]() {                 // b,c by reference, a by value
    b = 100;
    c = 200;
    // a ist nur eine Kopie
};
```

## Ranges (C++20)
```cpp
std::vector<int> input = {0, 1, 2, 3, 4, 5};
std::vector<int> output;

// Workimg with container and the algorithm library traditional with iteratoren
std::copy_if(input.begin(), input.end(), std::back_inserter(output), 
             [](int n){ return n % 2 == 0; });

// Workimg with container and the algorithm library more modern with ranges and pipe operator |
auto even_squares = input 
  | std::views::filter([](int n){ return n % 2 == 0; })
  | std::views::transform([](int n){ return n * n; });

std::vector<int> vecEvenSquares(even_squares.begine(), even_squares.end());  // even_squars range_view to std::vector<int>

/*
even_squares is a C++20 Range-View, that is filtering from the input only the even numbers
and then square it. The working is from the left to the right and the ouput is always ofer
give to the pipe |. 
*/
```

## `memory` (dynamic memory management)
```cpp
#include <memory>           // Include memory (std namespace)
shared_ptr<int> x;          // Empty shared_ptr to a integer on heap. Uses reference counting for cleaning up objects.
x = make_shared<int>(12);   // Allocate value 12 on heap
shared_ptr<int> y = x;      // Copy shared_ptr, implicit changes reference count to 2.
cout << *y;                 // Dereference y to print '12'
if (y.get() == x.get()) {   // Raw pointers (here x == y)
    cout << "Same";  
}  
y.reset();                  // Eliminate one owner of object
if (y.get() != x.get()) { 
    cout << "Different";  
}  
if (y == nullptr) {         // Can compare against nullptr (here returns true)
    cout << "Empty";  
}  
y = make_shared<int>(15);   // Assign new value
cout << *y;                 // Dereference x to print '15'
cout << *x;                 // Dereference x to print '12'
weak_ptr<int> w;            // Create empty weak pointer
w = y;                      // w has weak reference to y.
if (shared_ptr<int> s = w.lock()) { // Has to be copied into a shared_ptr before usage
    cout << *s;
}
unique_ptr<int> z;          // Create empty unique pointers
unique_ptr<int> q;
z = make_unique<int>(16);   // Allocate int (16) on heap. Only one reference allowed.
q = move(z);                // Move reference from z to q.
if (z == nullptr){
    cout << "Z null";
}
cout << *q;
shared_ptr<B> r;
r = dynamic_pointer_cast<B>(t); // Converts t to a shared_ptr<B>

```

## `math.h`, `cmath` (floating point math)
```cpp
#include <cmath>            // Include cmath (std namespace)
sin(x); cos(x); tan(x);     // Trig functions, x (double) is in radians
asin(x); acos(x); atan(x);  // Inverses
atan2(y, x);                // atan(y/x)
sinh(x); cosh(x); tanh(x);  // Hyperbolic sin, cos, tan functions
exp(x); log(x); log10(x);   // e to the x, log base e, log base 10
pow(x, y); sqrt(x);         // x to the y, square root
ceil(x); floor(x);          // Round up or down (as a double)
fabs(x); fmod(x, y);        // Absolute value, x mod y
```

## `assert.h`, `cassert` (Debugging Aid)
```cpp
#include <cassert>        // Include iostream (std namespace)
assert(e);                // If e is false, print message and abort
#define NDEBUG            // (before #include <assert.h>), turn off assert
```

## `iostream.h`, `iostream` (Replaces `stdio.h`)
```cpp
#include <iostream>         // Include iostream (std namespace)
cin >> x >> y;              // Read words x and y (any type) from stdin
cout << "x=" << 3 << endl;  // Write line to stdout
cerr << x << y << flush;    // Write to stderr and flush
c = cin.get();              // c = getchar();
cin.get(c);                 // Read char
cin.getline(s, n, '\n');    // Read line into char s[n] to '\n' (default)
if (cin)                    // Good state (not EOF)?
                            // To read/write any type T:
istream& operator>>(istream& i, T& x) {i >> ...; x=...; return i;}
ostream& operator<<(ostream& o, const T& x) {return o << ...;}

void printByteAsBinary(std::uint8_t in) 
{
std::cout << static_cast<int>(in) << " as binary: " << std::bitset<8>(in) << std::endl;
}
```

## `fstream.h`, `fstream` (File I/O works like `cin`, `cout` as above)
```cpp
#include <fstream>              // Include filestream (std namespace)
std::string s{}, text{};    
std::ifstream f1("text.txt");   // Open text file for reading
if (f1)                         // Test if open and input available
  f1 >> s;                      // Read object from file
std::cout << s << '\n' << std::endl;    // output first line of text.txt
f1.close();                         // close file handle
s.clear();                          // clear string and release memory

std::ifstream f2("text.txt");       // Open text file for reading
if (f2.is_open()) {
  while (std::getline(f2, s)) {
    text += s + '\n';           // read while getline is true
  }
  std::cout << text << '\n' << std::endl; 
}
f2.close(); s.clear(); text.clear();

// Output in file
std::ofstream file_out{"text_out.txt"};  // Open file for writing
if (file_out)   
  file_out << text;                      // Write to file only one line
file_out.close();

uint32_t append_line_to_file(const std::string &filepath, const std::string &line)
{
    std::ofstream file{};
    // open file for output and append -> other options are 'binary', 'in'
    file.open(filepath, std::ios::out | std::ios::app); 
    if (file.fail())
        return -1;
    file << line;                   // append line to file
    if (file.good())
        return 1;
    return 0;                       // file is local and close will called automatically
}

// Datei im Binärmodus zum Lesen öffnen
std::ifstream file("daten.bin", std::ios::binary);
if (!file) {
  std::cerr << "Datei konnte nicht geöffnet werden\n";
  return 1;
}
// Dateigröße ermitteln
file.seekg(0, std::ios::end);
std::streamsize size = file.tellg();
file.seekg(0, std::ios::beg);
// Puffer anlegen und gesamte Datei einlesen
std::vector<char> buffer(size);
if (!file.read(buffer.data(), size)) {
  std::cerr << "Fehler beim Lesen\n";
  return 1;
}
```

## `std::filesystem` (C++17)
```cpp
#include <filesystem>         
const fs::path workspace_path = "C:/Users/Jan/OneDrive/_Coding/UdemyCpp";
fs::path chapter_path;
chapter_path = workspace_path;
chapter_path /= "06_String";             // --> 'C:/Users/Jan/OneDrive/_Coding/UdemyCpp/06_String'

auto current_file_path = fs::current_path();    
std::cout << "relative_path: " << current_file_path.relative_path() << '\n';
std::cout << "parent_path: " << current_file_path.parent_path() << '\n';
std::cout << "filename: " << current_file_path.filename() << '\n';
std::cout << "stem: " << current_file_path.stem() << '\n';
std::cout << "extension: " << current_file_path.extension() << '\n';
std::cout << "exists: " << fs::exists(current_file_path) << '\n';

for (auto it = fs::directory_iterator(current_path);
         it != fs::directory_iterator{};
         ++it)
{
  std::cout << *it << std::endl;      // output 
}
```

## `std::string` (Variable sized character array)
- Memory allocation on heap - dynamic size
```cpp
#include <string>         // Include string (std namespace)
string s1, s2="hello";    // Create strings
s1.size(), s2.size();     // Number of characters: 0, 5
s1 += s2 + ' ' + "world"; // Concatenation
s1 == "hello world"       // Comparison, also <, >, !=, etc.
s1[0];                    // 'h'
s1.substr(m, n);          // Substring of size n starting at s1[m]
s1.c_str();               // Convert to const char*
s1 = to_string(12.05);    // Converts number to string
std::getline(cin, s);     // Read line ending in '\n'

// Find string in text and replace it
const std::string search_string{"Eins"};
const std::string replace_string{"One"};
const auto idxFind = text.find(search_string);
if (idxFind != std::string::npos)
  text.replace(idxFind, search_string.size(), replace_string);

// string to int 
std::string str = "123";
int num = std::stoi(str);

// std::string_view
// - mainly used for efficient view on string or literal sequences without create copies of it
// - bestehende Zeichensequenzen zu bieten, ohne Kopien zu erstellen.
// - is used as parameter of functions, where strings are readed but not modified or should own by itself
// - for stream output, searchs or parse or compare
// - avoid implicite conversion of std::string, C-Strings oder Literalen
void print(std::string_view sv) {
    std::cout << sv << '\n';
}
std::string str = "Hallo Welt";
print(str);                       // no copy of str
    print("Literal");             // works directly without construct string type
```

## `std::vector` (Variable sized array/stack with built in memory allocation)
- Memory allocation on heap - dynamic size (std::array has fix size, so that the memory is on the stack)
- all elements are side-by-side in the memory
- a push_back or pop of elements can trigger a new memory allocation, which consume many time
```cpp
#include <vector>         // Include vector (std namespace)
vector<int> a(10);        // a[0]..a[9] are int (default size is 0)
vector<int> b{1,2,3};        // Create vector with values 1,2,3
a.size();                 // Number of elements (10)
a.push_back(3);           // Increase size to 11, a[10]=3
a.back()=4;               // a[10]=4;
a.pop_back();             // Decrease size by 1
a.front();                // a[0];
a[20]=1;                  // Crash: not bounds checked
a.at(20)=1;               // Like a[20] but throws out_of_range()

vector<int> b(a.begin(), a.end());  // b is copy of a
vector<T> c(n, x);        // c[0]..c[n-1] init to x
T d[10]; vector<T> e(d, d+10);      // e is initialized from d

// Iterations | loops
std::vector<std::vector<int>> mat;                              // empty 2D vector
std::vector<std::vector<int>> mat(5);                           // 5 rows, each row initially empty
std::vector<std::vector<int>> mat(5, std::vector<int>(2, 0));   // 5 rows and 2 columns with zero

// classic iteration
for (size_t i = 0; i < mat.size(); ++i) {                       // 1. dimension = row
  for (size_t k = 0; k < mat[i].size(); ++k) {                  // 2. dimension = column
    mat[i][k] = 1;                                              // mat[i][k] = mat[row][col]
  }
}
// or iteration with iteratoren
for (auto itr_row = mat.begin(); itr_row != mat.end(); ++itr_row) {                      // 1. dimension = row
  for (auto itr_col = mat[itr_row].begin(); itr_col != mat[itr_row].end(); ++itr_col) {  // 2. dimension = column
    *itr_col = 0;      // iterator is a pointer
  }
}
// or range based for loop
for (auto &row : mat) {                      // 1. dimension = row
  for (auto &col : row) {                    // 2. dimension = column
    ;  // code...
  }
}

// std::erase | on containers (C++20)
std::vector<int> v{1, 2, 3, 2, 4, 2, 5};
auto removed = std::erase(v, 2);           // alle 2en aus dem Vektor entfernen

std::string s = "a_b_c_d_e";
auto removed = std::erase(s, '_');         // alle Unterstriche entfernen

std::vector<std::string> names{"Alice", "Bob", "Alice", "Eve"};
auto removed = std::erase(names, std::string{"Alice"}); // alle "Alice" entfernen

std::vector<int> v{1,2,3,4,5,6,7,8,9,10};
auto removed = std::erase_if(v, [](int x) {
                              return x % 2 == 0; });    // Bedingung: gerade Zahl

// using of std:vector to store 2D Images
int height = 480;  // Bildhöhe
int width = 640;   // Bildbreite
std::vector<std::vector<uint8_t>> image(height, std::vector<uint8_t>(width * 4, 0));  // RGBA: 4 Bytes pro Pixel
image[row][col * channels + channel] = ... ;         // access to pixel -> channels=4 for RGBA:

// resize 2D vector
m_Matrix.resize(height);
for(auto &row : m_Matrix) { row.resize(width); }

if (height > m_height) {
  auto itrRow = m_Matrix.begin();
  itrRow += m_height;
  for (; itrRow != m_Matrix.end(); ++itrRow)
  {
    for(auto itrCol = itrRow->begin(); itrCol != itrRow->end(); ++itrCol ){
      *itrCol = 0;               
    }             
  }
}
```

## `std::tuple` (container for store fix number of elements with different data types)
```cpp
// 1. Definition eines Tupels: Name (string), Alter (int), Gehalt (double)
std::tuple<std::string, int, double> person = std::make_tuple("Alice", 30, 55000.5);

// 2. Zugriff auf die Elemente mit std::get<Index>(tuple_name)
// Die Indizes sind 0-basiert.
std::string name = std::get<0>(person);
int alter = std::get<1>(person);
double gehalt = std::get<2>(person);
std::cout << name << " ist " << alter << " Jahre alt." << std::endl;

// 3. Werte ändern
std::get<1>(person) = 31; 
std::cout << "Neues Alter: " << std::get<1>(person) << std::endl;

// Definition and insert elements 
std::map<OTCID, std::tuple< std::unique_ptr<TcWMc::CAxis>,
                            STcWMcAxisCommand,
                            STcWMcAxisMotionDatas,
                            STcWMcAxisMotionDatas,
                            TcW::SStatusEx>> m_mapAxis{};

m_mapAxis.emplace(
    someId,
    std::make_tuple(
        std::make_unique<TcWMc::CAxis>(/* args */),
        STcWMcAxisCommand{},        // init an instance of struct/class and set all members to default 
        STcWMcAxisMotionDatas{},    // init an instance of struct/class and set all members to default
        STcWMcAxisMotionDatas{},    // init an instance of struct/class and set all members to default
        TcW::SStatusEx{}            // init an instance of struct/class and set all members to default
    )
);

// Access
for (auto& [id, tuple] : m_mapAxs) {
  auto& [xisPtr, command, motion1, motion2, status] = tuple;
}
// or
auto& tuple = m_mapAxis[axisId];
auto& axisPtr = std::get<0>(tuple);
auto& command = std::get<1>(tuple);
// or
auto& [axisPtr, command, motion1, motion2, status] = m_mapAxis.at(axisId);
```

## Iteratoren (similar to pointer)
- forward_iterator: iterate from begin to end
- bidirectional_iterator: iteration in both directions
- random_access_iterator: allow directly access to an index
```cpp
auto begin = my_vector.begin(); 
auto end = my_vector.end();     // bool vector_is_empty = begin == end ? true : false
                                // end = past-the-end element and shall not be dereferenced
std::cout << *begin << '\n';
std::cout << *end << '\n' << '\n';

for (auto it = my_vector.begin(); it != my_vector.end(); ++it) {
  std::cout << *it << '\n';
}

std::vector<uint32_t> my_vec{1, 2, 3, 4, 5};
// forward iterator
for (auto it = my_vec.begin(); it != my_vec.end(); ++it) { 
  *it += 1; // change my_vec entries with dereferncing
}
print_vec(my_vec);      // output: 2,3,4,5,6

// bidirectional iterator
for (auto it = my_vec.rbegin(); it != my_vec.rend(); ++it) { 
  *it -= 1; // change my_vec entries with dereferncing
}
print_vec(my_vec);      // output: 1,2,3,4,5
```

## Range based for loop
- works in background with iteratoren
```cpp
// with structured binding C++17
struct Data{ float x; float y; };
auto vec = std::vector<Data>{Data{1.0F, 2.0F}, Data{4.0F, 6.0F}};

for (auto &[x_, y_] : vec) {
  x_ = -1.0F;
  std::cout << x_ << " " << y_ << '\n';
}

// 2 dimensional
for (const auto &row : my_vector) {
  for (const auto &val : row)
  {
    std::cout << val << '\n';
  }
}
```

## `deque` (Array stack queue)
`deque<T>` is like `vector<T>`, but also supports:
```cpp
#include <deque>          // Include deque (std namespace)
a.push_front(x);          // Puts x at a[0], shifts elements toward back
a.pop_front();            // Removes a[0], shifts toward front
```

## `std::pair'
```cpp
#include <utility>        // Include utility (std namespace)
std::pair<string, int> a{"hello", 3};  // A 2-element struct
a.first;                  // "hello"
a.second;                 // 3

using Friends = std::map<std::string, std::pair<std::int32_t, std::int32_t>>;
Friends my_map2{};
my_map2["Tobias"] = std::make_pair(43, 83);
my_map2["Juliane"] = std::make_pair(40, 57);
my_map2["Kurt"] = std::make_pair(5, 22);
```

## `std::map` 
- associative array - usually implemented as binary search trees - avg. time complexity: O(log n)
```cpp
#include <map>            // Include map (std namespace)
map<string, int> a;       // Map from string to int
a["hello"] = 3;           // Add or replace element a["hello"]
for (auto& p:a)
    cout << p.first << p.second;  // Prints hello, 3
a.size();                 // 1

using FriendsParams = std::pair<std::int32_t, std::int32_t>;
using Friends = std::map<std::string, FriendsParams>;

// Default contructor
Friends my_map1{};
my_map1["Tobias"] = FriendsParams{43, 83};
my_map1["Juliane"] = FriendsParams{40, 57};
my_map1["Kurt"] = FriendsParams{5, 22};

// Initializer list contructor
Friends my_map3{{"Tobias", {43, 83}}, {"Juliane", {40, 57}}};

// Output
for(const auto &row : my_map1) {
  std::cout   << "Name = " << row.first 
              << " | Alter = " << row.second.first
              << " | Gewicht = " << row.second.second << std::endl;
}
```

## `unordered_map` 
- associative array - usually implemented as hash table - avg. time complexity: O(1))
```cpp
#include <unordered_map>  // Include map (std namespace)
unordered_map<string, int> a; // Map from string to int
a["hello"] = 3;           // Add or replace element a["hello"]
for (auto& p:a)
    cout << p.first << p.second;  // Prints hello, 3
a.size();                 // 1
```

## `std::set` 
- std::set hold unique (no duplicated) entries automatically sorted in ascending ordered
- store unique elements - usually implemented as binary search trees - avg. time complexity: O(log n)
```cpp
#include <set>            // Include set (std namespace)
set<int> s;               // Set of integers
s.insert(123);            // Add element to set
if (s.find(123) != s.end()) // Search for an element
    s.erase(123);
cout << s.size();         // Number of elements in set

std::set<std::string> namen = {"Hans", "Anna", "Peter", "Anna"};
for (const std::string& name : namen) {
  std::cout << name << " ";
}
std::cout << "\n";        // Ausgabe: Anna Hans Peter | the double of ANNA will automatically deleted

std::set<int> s = {1, 2, 3};
if (s.find(2) != s.end()) {
  std::cout << "2 ist im Set enthalten.\n";
}
if (s.find(4) == s.end()) {
  std::cout << "4 ist nicht im Set.\n";
}
```

## `unordered_set` 
- store unique elements - usually implemented as a hash set - avg. time complexity: O(1)
```cpp
#include <unordered_set>  // Include set (std namespace)
unordered_set<int> s;     // Set of integers
s.insert(123);            // Add element to set
if (s.find(123) != s.end()) // Search for an element
    s.erase(123);
cout << s.size();         // Number of elements in set
```
## `random` 
```cpp
#include <random>

std::vector<int32_t> my_vec(NUM_ELEMENTS, 0U);
// random numbers
std::mt19937 random_generator{42};
// distribution
std::uniform_int_distribution<std::int32_t> distribution{-10, 10};
// init vector with random numbers within distribution
for (auto &row : my_vec) {
  row = distribution(random_generator);
}
```

## `algorithm` (A collection of 60 algorithms on sequences with iterators)
```cpp
#include <algorithm>      // Include algorithm (std namespace)
min(x, y); max(x, y);     // Smaller/larger of x, y (any type defining <)
swap(x, y);               // Exchange values of variables x and y
sort(a, a+n);             // Sort array a[0]..a[n-1] by <
sort(a.begin(), a.end()); // Sort vector or deque
reverse(a.begin(), a.end()); // Reverse vector or deque
```

## `limits` 
```cpp
#include <limits>      // Include 

template <typename T>
void print_type_properties()
{
  std::cout << "min=" << std::numeric_limits<T>::min() << '\n'
            << "max=" << std::numeric_limits<T>::max() << '\n'
            << "bits=" << std::numeric_limits<T>::digits << '\n'
            << "decdigits=" << std::numeric_limits<T>::digits10 << '\n'
            << "integral=" << std::boolalpha
            << std::numeric_limits<T>::is_integer << '\n'
            << "signed=" << std::boolalpha
            << std::numeric_limits<T>::is_signed << '\n'
            << "exact=" << std::boolalpha << std::numeric_limits<T>::is_exact << '\n'
            << "infinity=" << std::boolalpha
            << std::numeric_limits<T>::has_infinity << '\n';
}
// --> call
print_type_properties<std::uint16_t>();

template <typename T>
bool almost_equal(const T x, const T y)
{
    return std::abs(x - y) <= std::numeric_limits<T>::epsilon();
}
```

## `chrono` (Time related library)
```cpp
#include <chrono>                // Include chrono
//steady_clock is suitable for interfall measurment
const auto start_time = std::chrono::steady_clock::now();   // static member function will called by the class name
for (int i = 0; i < 10'000'000; ++i) { fhelper *= i; }
const auto end_time = std::chrono::steady_clock::now();     // static member function will called by the class name
const auto elapsed_time 
       = std::chrono::duration_cast<std::chrono::milliseconds>(end_time-start_time);   // resolution ms
std::cout << "Time " << elapsed_time.count() << std::endl;  // output in ms

// high_resolution_clock may be an alias for stead_clock or system_clock
using namespace std::chrono;     // Use namespace
auto from =                      // Get current time_point
  high_resolution_clock::now();
// ... do some work       
auto to =                        // Get current time_point
  high_resolution_clock::now();
using ms =                       // Define ms as floating point duration
  duration<float, milliseconds::period>;
                                 // Compute duration in milliseconds
cout << duration_cast<ms>(to - from)
  .count() << "ms";
```

## `thread` (Multi-threading library)
```cpp
#include <thread>         // Include thread
unsigned c = 
  hardware_concurrency(); // Hardware threads (or 0 for unknown)
auto lambdaFn = [](){     // Lambda function used for thread body
    cout << "Hello multithreading";
};
thread t(lambdaFn);       // Create and run thread with lambda
t.join();                 // Wait for t finishes

// --- shared resource example ---
mutex mut;                         // Mutex for synchronization
condition_variable cond;           // Shared condition variable
const char* sharedMes              // Shared resource
  = nullptr;
auto pingPongFn =                  // thread body (lambda). Print someone else's message
  [&](const char* mes){
    while (true){
      unique_lock<mutex> lock(mut);// locks the mutex 
      do {                
        cond.wait(lock, [&](){     // wait for condition to be true (unlocks while waiting which allows other threads to modify)        
          return sharedMes != mes; // statement for when to continue
        });
      } while (sharedMes == mes);  // prevents spurious wakeup
      cout << sharedMes << endl;
      sharedMes = mes;       
      lock.unlock();               // no need to have lock on notify 
      cond.notify_all();           // notify all condition has changed
    }
  };
sharedMes = "ping";
thread t1(pingPongFn, sharedMes);  // start example with 3 concurrent threads
thread t2(pingPongFn, "pong");
thread t3(pingPongFn, "boing");
```

## `NLohman json` (GitHub Library for json)
```cpp
https://github.com/nlohmann/json/blob/develop/README.md

```

## `future` (thread support library)
```cpp
#include <future>         // Include future
function<int(int)> fib =  // Create lambda function
  [&](int i){
    if (i <= 1){
      return 1;
    }
    return fib(i-1) 
         + fib(i-2);
  };
future<int> fut =         // result of async function
  async(launch::async, fib, 4); // start async function in other thread
// do some other work 
cout << fut.get();        // get result of async function. Wait if needed.
```
