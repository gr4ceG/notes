*What if we want a module to provide a global variable?*

``` c++
// abc.h
int globalNum; 
```
***Wrong***: `int globalNum;` in `abc.h` is a defintion and declaration:  every file that includes `abc.h` gets it's own copy of this global variable, it is then defined multiple times, and the program won't link  

***Solution***: put the definition of the variable in the `.cc` file so it only gets compiled once, only declare it in the header

``` c++
// abc.cc
int globalNum; // Definition (assign value too if wanted) 

// abc.h
extern int globalNum; 
```

# Linker 
Linker: combines compiled object files into a single executable - in order to do so it not only combines the files but resolves external symbols 

> Symbols that are declared not defined in a `.cc` file must be resolved by the linker (e.g In pur vec header we declared an overload addition operator between two vecs 

``` c++
// vec.h
struct vec {
	int x, y; 
}

vec opertator+(const vec &v1, const vec &v2); // Declaratin only, defintioon n vec.cc
```

And yet, `main.cc` called this function and was compiled, but it only knows about the function declaration - that's because the compiler sees this function has been declared, generates placeholder information object file to say it wants to call it, the linker then finds it in the other object file and updates the location of it in the compiler's placeholder code. 

``` c++
#include "vec.h"
int main() {
	vec v{1,2}
	v = v + v; 
}
```

Suppose we wanted to create a linear algebra module: 

``` c++
// linalg.h
#include "vec.h"
...

// linalg.cc
#include "linalg.h"
#include "vec.h"
```

***This dosent compile***: `main.cc` includes `vec.h` and `linalg.h`, but `linalg.h` includes `vec.h`. So, these two `.cc` files get two copies of `vec.h` and thus two definitons of struct    `vec` - we cannot define something twice, so it is an error.

***Solution***: 

```c++
#include guard 

// vec.h 
#ifndef VEC_H // (if not defined) 
#define VEC_H

... // contents of vec.h
#endif 
``` 

The first time `vec.h` is included, the symbol `VEC_H` is not defined, so the file (including the defintion of `VEC_H`) is uncluded. Subsequent include `VEC_H` ss defined, so the `#ifndef` is false and the file gets supressed. 

***Always***: put `#include guard` in .h files

***DON'T***: put using `namespace ~`; in `.h` files - it forces files that include your header to have that using directive 

==NEVER EVER include .cc files, and NEVER EVER compile .h files==, their code gets compiled as part of the files they're included in 

# Classes 

Can put functions in structs: 

``` c++
// student.h 
struct student {
	int assms, mt, final; 
	float grade(); 
}

// student.cc
float student::grade() {
	return assms * 0.4 + mt * 0.2 + final * 0.4; 
}

// client code 
Student s{60, 70, 80}; 
cout << s.grade(0 << endl; 
```

## Some OOP terminolgy

- A *class* is essentially a structure type that can potentially contain functions. [Student (above) is a class]
- An *object* is an instance of a class. [s (above) is an object]
- The functions inside the class are called member functions (or *methods*) 
- `::` is the *scope resolution operator*. [`c::f` means f in the context of c (a class or namespace) where `::` is like `.`, where the LHS is a class (or namespace) rather than the object]

*Inside the function grade, what do assms, mt, and finals mean?* Since these fields don't exist until an object is instantiated, they refer to the fields of the opbject that the method is called on. 

```c++
Student billy{...}; 
billy.grade(); // method call uses Billy's assms, mt, and final 
```

Formally, ==methods take a hidden extra parameter which is called `this`==. Which is a ptr to the object the method was called on. (e.g in above, `this == &billy`) 

``` c++
float student::grade() {
	return this->assms * 0.4 + this->mt * 0.2 + this->final * 0.4; 
} // analogous to what we wrote before, this-> was implicit
  // Only need to specify this-> field is there;s another variable in scope with that in a name, e,g a parameter
```

# Initializing Objects 

``` c++
Student abdur{60, 70, 80}; // ok, but limited 
```

Better solution: include a method that initializes called a ***constructor***. 

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
}
```


