``` c++
struct Student {
	int assns, mt, finals; 
	float grade(); 
	Student(int assms, int mt, int final); 
}

Student::student (int assms, int mt, int final) {
	this->assns = assns; 
	this->mt = mt; 
	this->final = final; 
	if (this->assns < 0) this->assns = 0; 
	if (this->assns > 100) this->assns = 100; 
}

Student s{60,70,80}; // Better!
```

If a constuctor has been defined, the numbers above are passed as `args` to that constructor. 

If no constructor has been defined, then these initialize the individual fields of student. Alternatively, 

``` c++
student abdus = Student{60, 70, 80} // does the same thing 
```
# Uniform Initialization 

A note on **uniform initialization**: sometimes variables are initialized with `=`: 

``` c++
int x = 5; 
String s = "hello"; 
```

We may also initailized by `()` especially when invoking constructors: 

``` c++
int x(s); 
String s("hello"); 
Student Michelle(60, 70, 80); 
```

The rules about when we may use `=` vs. `()` are somewhat arbitrary, `c++` now has uniform initalization syntax, using `{}` which is meant to be used in nearly all initialization cases. 

``` c++
int x{s}; 
String s{"hello"}; 
Student Michelle{60, 70, 80}; 
```

==Try to get into the habit of using `{}` for initialization==, it is the preferred modern c++ style. 


For heap allocation, 

``` c++
Student *pjane = new Student{60, 70, 80}; 
```

***Advantages of constructors***: logic checks, ==default parameters==, logic other than simple field initialization 

``` c++
Struct Student {
	...
	Student(int assns = 0, int mt = 0, int final = 0); 
	...
}

Student Mary{70, 80} // 70, 80, 0
Student newKid; // 0 0 0
```

***Note***: every class comes with a default (zero-parameter) constructor (which just default constructs all fields which are objects) 

``` c++
vec v; // default constructor (does nothing) 
```

But, the built in default constructor goes away as soon as you provide any constructor. 

``` c++
Struct vec {
	int x; 
	int y; 
	vec (int x, int y) {
		this->x = x; 
		this->y = y; 
	}
}

vec v{1, 2}; // OK
vec w; // error! -> we wrote the constructor, so now the built in default constructor does not exist 

Struct Basis { 
	vec v1, v2; 
}

Basis b; // ERROR: vec built in default constructor does not exist 
```

``` c++
Struct MyStruct {
	const int myConst; 
	int &myRef; 
} // WRONG
```

==Constants and references must be initialized!==

``` c++
int z;
Struct myStruct {
	const int myConst = 5; 
	int &myRef = z; 
}
```

But should every `myStruct` object need to have the same value of `myConst` and `myRef`?

*Example:*

``` c++
Struct Student {
	const int id; // does not change, different for every student 
}
```

Where to initialize? 
- Constructor body? too late by then, fields must be fully constructed by the time the constructor body runs

What happens when an object is created?

1. Space is allocated
2. Fields ate initialized 
3. Constructor body runs

How do we specify how to initialize fields in step 2? **Member-initialization list (MIL)**

```
Struct Student {
	const int id; 
	int assns, mt, final; 
	Student (int id, int assns, int mt, int final): 
	id{id}, assns{assns}, mt{mt}, final{final}{}
}
```

Names outside the `{}` denote the fields, inside they denote the parameters (if parameters with those names exist)

***Notes:***

- you can (and should) initialize any field in the MIL, not just `const` and `refs`
- Fields listed in the MIL are initialized in the order they're declared in the class regardless of the order they show up in the MIL (you'll get a warning if those differ)
- the MIL is more efficient than assigning values in the body of the constructor, for fields that are objects, if they are not listed in the MIL, then they're default constructed; if you assign them in the body, they are then initialized and then assigned which is wasteful

```c++
struct Bar {
	Foo x; 
	#ifdef FAST
	Bar(int n): x{n}{}
	#else
	Bar(int n)
	x = Foo{n}
	}
	#endif 
}; 
```

FAST (using MIL) is faster 

for SLOW

- `Foo x` is default constructed (60 fib numbers) 
- After doing initialization, you realize you want `Foo{n}` so it is overwritten 
- `x` value is set twice, waste of time 

``` c++
struct Foo {
    int x = 20;
    const int y = 10;
    Foo (int x) : x{x} [};
    Foo(int x, int y) : x{x}, y{y} {}
};

int main() {
    Foo a; // {20, 10}
    Foo b{1}; // {1, 10}
    Foo c{1, 2}; //{1, 2}
}
```

What if a field is initialized both in the class and in the MIL? 

```c++
Struct vec {
	int x = 0; int y = 0; 
	vec(int x): x{x}{}
}
```

In this case, the MIL takes precedence, so `vec{x, y} // 5, 0`

Now consider: 

```c++
Student Mahmoud{60, 70, 80}; 
Student Caroline = Mahmoud; 
```

How does Caroline's initialization occur? via the copy constructor, which is invoked when an object is constructed with another object of the same type. 

***Note:*** every class comes with 

- a default constructor (lost if defined any other constructor)
- a cop constructor (just copy-initializes all fields)
- a copy assignment operator ()just copy-assignment all fields, lost for const)
- a deconstuctor 
- a move constructor 
- a move assignment 