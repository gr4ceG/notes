*Example*: Read all ints from `stdin` and echo them one per line. Stop on bad input or `EOF`. 

~~~ c++
int main(){
	int i; 
	while (true) {
		cin >> i; 
		if (cin.fail()) break; 
		cout << i << endl; 
	}
}
~~~

Note: 
- `>>` is c's right bitshift operator, `a >> b` shift's `a`'s bits to the right by `b`'s position. But when the lefthand side is an input stream like `cin`, it is the input operator. 
- The operator `>>`, when used for input, has `cin` (type i stream) as its first operand, and the data to be read in as its second (which could be of several different types). 

*But what is the output (return value) of the operator?* The return value is the stream itself (for example cin).

This is why we can write: 
```c++
((cin >> x) >> y) >> z;
```

- `cin` reads into `x`, populates it, then this operator returns `cin` back -> `(cin >> y) >> z`
- Then `cin` reads into `y`, returns back `cin`, etc. 

`cin`, or istreams in general, can be **implicitly cast to boolean values**: `cin` is true if its fail bits is not set, and false otherwise. 

~~~c++
int main() {
	int i; 
	while (true) {
		if (!(cin >> i)) break; 
		cout << i << endl
	}
}
~~~

and even better: 

~~~c++
int main() {
	int i; 
	while (cin >> i) {
		cout << i << endl
	}
}
~~~

*Example*. Read all ints from `stdin` until `EOF`, skip over bad input. 

~~~c++
#include <iostream>
using namespace std;

int main() {
	int i; 
	while (true) {
		if (!(cin >> i)) { 
			if (cin.eof()) break; 
			cin.clear() // clears the fail bit (and eof bit) 
			cin.ignore() // removes the immediate next character from the stream
		} else {
			cout << i << endl
		}
	}
}

[input: 1 2 3 4 abcd 56 17]

> 1
> 2
> 3
> 4
> 56
> 17

[input: 1 2 3 45hi abc6 7 10]

> 1
> 2
> 3
> 45
> 6
> 7
> 10
~~~

~~~c++
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
	char c; 
	cin >> std::noskipws; // io manipulator -> does not skip white space 
	while (cin >> c) {
		cout << c << endl
	}
}

[input: Hello there!]

> H 
> e
> l
> l
> o

> t
> h
> e
> r
> e
> !
~~~

`std::skipws` ??? skip white space modifier 

What if we want to print numbers in hexadecimal? The `<iomanip>` header provides several I/O manipulators which can healp us, e.g `std::hex`, 

~~~c++
cout >> std::hex >> 95 // prints 5f
~~~

But! `cout` will be printing hexadecimals numbers for all prints after this, to set it back to printing decimal numbers send `std::dec` to the stream.

# Strings 

In C: String are arrays of characters (`char *` or `char[]`), terminated by the character `\0`

- must explicitly manage memory - allocate more memory as string grows
- easy to accidentally overwrite null terminator and corrupt memory 

In C++: Strings require `#include <string>`, type `std::string`

- grows as needed (no need to manage memory) 
- safer to manipulate 

*Ex.* `String s = "Hello"` ??? Even in `C++`, the literal "`Hello`" here is still a C-style string (i.e a null-terminated array of characters). The varriable `s` is a string whose value has been initilized using that C-string. 

**Some string operations:** 
| String Operation | Code |
|---|---|
| equality, inequality: compares memory locations of `s1` and `s2` | `s1 == s2`, `s1 != s2` |
| comparison: in lexographic order | `s1 <= s2`, etc. |
| length: [O(1) time] | `s1.length()` |
| fetch individual characters | `s[0]`, `s[1]`, `s[2]` |
| concatenation | `s3=s1+s2`, `s3+=s4` |

~~~c++
int main() {
	string s; 
	cin >> s // Read a string; skips leading ws, stops reading at next ws (i.e read one word) 
	cout << s; 
}
~~~

One way to read ws: `getline(cin, s)` reads to the newline character into `s`

The stream abstraction applies to other data sources (e.g Files), read from a file instead of `stdin`: In C++ we use `std::ifstream` to read from a file and `std::ofSteam` to write to a file. 

# File Access 

In C: 

~~~c
#include <stdio.h>

int main() {
	char s[256]
	FILE *file = fopen("suite.txt", "r"); 
	
	while (true) {
		fscanf(file, "%255s", s); // only reads 25 characters 
		if (feof(file)) break; 
		print("%s\n", s); 
	}
	fclose(file); 
}
~~~

In C++: 

~~~c++
#include <iostream>
#include <fstream>
using namespace std; 

int main() {
	ifstream file{"suite.txt"}; // declaring and initializing an ifstream to open the file 
	string s; 
	while (file >> s) {
		cout << s << endl; 
	}
} // As soon any fstream varaible goes out of scope, the file is closed. 
~~~

- As soon any `fstream` variable goes out of scope, the file is closed. 
- Anything you can do with `cin`/`cout`, you can do with an `ifstream`/`ofstream` respectively

*Example.* The stream abstraction can also apply to strings!

`#include <sstreams>`, types `std::istringstream` for reading from a string, `std::ostringstream` for writing to a string. 

*Example.* Convert a string into a number: 

~~~c++
int n; 
while (true) {
	cout << "Enter a number: "; 
	string s; 
	cin >> s; 
	istringstream ss{s}; 
	if (ss >> n) break; // stop if string contains a number 
	cout << s << "Is not a number, I said:" << endl; 
}
~~~

Entire `string` is being turned into an `int`, any `int`s in the `string` are thrown out. 