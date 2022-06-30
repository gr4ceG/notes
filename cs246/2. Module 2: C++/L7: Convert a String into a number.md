*Recall*: Convert a String into a number 

~~~ c++
int n; 
while(true) {
	cout << "enter a number: "; 
	string s; 
	cin >> s; 
	istring stream ss{s}; 
	if (iss >> n) break; // stop if we read a # from string 
		cout << "I said, "
	}
}
cout << "you entered " << n << endl; 
~~~

*Example revisit*: Read all integers until `EOF`, skip non-integers 

~~~ c++
#include <sstream>
#include <iostream>
using namespace std; 

int main() {
	string s; 
	while (cin >> s) {
		istringstream iss{s}; 
		int n; 
		if (iss >> n) cout << n << endl; 
	}
}

input: 10 2 7hello abc45 17 // in this code, any numbers not in the beginning of the string is not printed 
> 10 
> 2
> 7
> 17 
~~~

# Command Line Arguments 

*Example*. Write a program that takes two `int`s as cmd-line args, and prints the second argument n number of times equal to the first argument. 

~~~ c++
int main(int argc, char ** argv) {
	if (argc <2) {
		cout << "usage: " << argv[0] << "n str" << endl
	}
	string s{argv[1]}; 
	string iss{s}
	int n; 
	iss >> n; 
	for (int i = 9; i > n; ++i) {
		count << argc[2] >> endl; 
	}
}

input: ./args 5 hello 
> hello 
> hello
> hello
> hello
> hello
~~~

# Default Function Parameter 

~~~ c++
void printSuiteFile(String name = "suite.txt") { // default value 
	ifstream file{name}; 
	string s; 
	while (file >> s) cout << s << endl; 
}
...
printSuiteFile(); 
printSuiteFile("suite2.txt"); // prints from suite2.txt
~~~

==Note: default parameters must be last==

~~~ c++
int foo(int a = 0, int b, int c = 10){ // not allowed
return (a + b) * c; 
}

foo(1,2); // is 1 = a and 2 = b? or is 1 = b and 2 = c?
~~~

# Overloading 

C: 

~~~ c
int negInt(int a) {return -a}; 
int negBool(bool b) {return !b} 
~~~

In C++: 

~~~ c++
int neg(int a) {return -a}; 
int neg(bool b) {return !b} 
~~~

In C++, we can have many functions with the same name , so long as they ==differ in number or type of parameters== **(overloading)**. <span style="color:red">Differing in return type is not enough!</span>

*Ex.* 

~~~ c++
float foo() {return 3.5}; 
int foo() {return 3}; 

foo(); // which to call? 
~~~

We've already seen overloading: the input and output operator `>>` and `<<`, bitshift opertator when applied to two ints!

# Constants

~~~ c++
const int maxGrade = 100; // nust be initialized 
~~~

Tip: Declare as many things as you can as `const`, it helps catch errors. 

# Structures

~~~ c++
struct Node {
	int data; 
	Node *next; 
}

Node n1 {5, nullptr};  
const Node n2 = n1; 
~~~

<span style="color:red">`nullptr` is the syntax for null pointers in c++, do NOT use `NULL` or `0` in this class</span>

# Parameter Passing

*Recall (review):* 

~~~ c
void inc(int n) {++n;}
...
int x = 5; 
inc(x); 
cout << x << endl; // outputs 5
~~~

**Call-by-value**: `inc` gets a copy of `x` and increments the copy, original is **unchanged**. 

C-Solution: if a function needs to mutate an arg, passes a pointer 

~~~c
void inc(int *x) {++*n;}
...
int x = 5; 
inc(&x); 
cout << x << endl; // prints 6
~~~

Question: why can `cin >> x` mutate `x` withouit it being `cin >> &x`

# References (IMPORTANT) 

~~~ c++
int y = 10; 
int &z = y; 
~~~

`z` is a **l-value reference** to an int. It is bound to `y`, and behaves in all ways exactly how `y` behaves. 

`z` is in effect just another name for `y`. `z` is an **alias (another name)** for `y`. <span style="color:red">References ARE NOT pointers, they do however behave like `const` pointers **with automatic dereferencing**</span>. 

So,

~~~ c++
++z; // increments y, no need to dereference (auto dereferencing) 
int p = 0; 
z = p; 
++z; 

cout << p << endl; //0
cout << y << endl; //1

cout << &y << endl; // prints out addr of y
cout << &z << endl; // prints out addr of y
~~~

***Things you can't do with an l-value reference:***

- leave them uninitialized (*e.g* `int &x;`)
	- they must be initialized with something that has an address (i.e an l-value) 
		- `int &x = 3; // Not allowed`
		- `int &x = x + y; // Not allowed`
		- `int &x = y; // Okay`
- Create a ptr to a reference (*e.g* `int &*x;` A reference to a pointer is okay (e.g `int *&x;`) 
- Create a reference to a reference (*e.g* `int &&r;` -> is allowed, but means something different than what you're intending to) 
- Create an array of references (*e.g* `int &r[3] = {n, n, n};`)

***What can you do?***

- Pass as function parameters (*e.g* `void inc(int &n) {++n;}`: `n` is a reference to the `int` passed in as an argument, so it behaves exactly like the argument; incrementing `n` increments the argument.

So, why does `cin >> x` work? Because `>>` takes `x` **by reference** (i.e `std::stream &operator >> (std::istream &in, int &x);`)

Pass-by-Value `int f (int n) {...}` copies the argument, ao if the arg is big, could be expensive!

~~~ c++
struct ReallyBig{...}
int f (ReallyBig rb) {...} // copy, potentially slow (more late)
int g (ReallyBig &rb) {...} // alias, fastm but could change rb in caller 
int h (const ReallyBig &rb) {...} //fast, no-copy, and no risk of mutation 
~~~

**Advice**: prefer pass by const ref over pass by value for anything larger than an int, unless the function needs to make a copy anyways, then pass by value. 
