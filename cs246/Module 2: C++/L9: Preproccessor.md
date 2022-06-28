*Recall*: the preproccessor - transforms your program before the compiler sees it 

`#____` - preprocessor directive, (e.g `#include`) 

Include old C headers (don't do this in class) 
new naming conventions, instead of (e.g `#include <stdio.h>`, use `#include<cstdio>`

```
#define VAR Val
```
Sets a preproccessing variable to have a value, all occurances of `VAR` in yout source file get replaced with `val`, excpt occurances in quoted string. 

*Example:*
``` c++
#define MAX 10
int x[MAX] // would be transformed to int x[10]
```

There is no good reason to do this in c++, use `const` definitions instead 

`#define FLAG` - sets the variabel `FLAG`, its value is the empty string. Defined constants are useful for conditional compiation.

*Example:*
``` c++
#define SECURITYLEVEL

#if SECURITYLEVEL == 1
short int 
#elif SECURITYLEVEL == 2
long long int 
#endif
publickey; 
```

***Special case:***

``` c++
#if 0
...
#endif 
```

`#if 0` id never true, so, all the inner text is removed before the compiler sees it, works as heavy duty "comment out". 

```c++
#define Y X
#define X 5

int main(){
	return X+Y // return 5+5 (mutiple passings, so the above declaration is valid)
}
```

Can also preprocessors define symbols, via the `cmd`-line compiler args

``` c++
int main(){
	cont << x < endl
}

> in LINUX: compiling  
g++ -DX=15 define.cc -o define 
./define 

> 15
```

`#ifdef NAME/#ifndef NAME`: `true` if **preproccess var/flag has/has not been defined**

``` c++
int main(){
	#ifdef DEBUG
		cout << "setting x = 1" << endl
	#endif
	int x = 1; 
	while (x<100) {
		++x; 
		#ifdef DEBUG
			cout << "x is now" << x << endl; 
		#endif 
	}
}
```

Compiled: 

``` bash
g++ loop.cc -o loop // we see NO print statements 

g++ -DDEBUG loop.cc -o
./loop // we see out own prints
```

# Seperation Compilation 

Spilt the program into composalie modules, each which provide: 

- ***Interface***: type definitions and prototypes (declarations) for functions (`.h` file)
- ***Implementation***: full definition for every provided function (`.cc` file)

*Recall*: 

- ***Declaration***: asserts existence 
- ***Definition***: Full details; allocates space (for vars/fns)

*Example*: 

```c++
// vec.h (interface) 
struct vec {
	int x,y; 
y; // definition of type vex 

vec operator+(const vec &v1, const vec &v2); 



// main.cc (clinet)
#include "vec.h"
int main(){
	vec v{1, 2}; 
	v=v+v; 
}

// vec.cc (implementation) 
#include "vec.h"
vec operator+(const vec &v1, const vec &v2){
	return {v1.x+v2.x, v1.y+v2.y}; 
	// this is ok because compiler knows return type is vec
}
```

*Recall*: An entity can be declared as many times as you want, but defined only once. 

# Compiling Seperately 

==NEVER compile `.h` files==

```bash
g++ -c vec.cc 	#-c means compile only, do not link 
				 	#does not build an executable, instead produces an object (.o) file 
g++ -c main.cc
g++ vec.o main.o -o main 
# link the object files together to produce executable #main 
```

- What happens if we change `vec.cc`? Only need to recompile `vec.cc` and relink. 
- What if we changed `vec.h`? Must recompile both `vec.cc` and `main.cc` (they both include `vec.h`), and relink.
- How can we keep track of dependencies and perform minimal complication? Linux tool: `make`

# Makefile 

Create a `Makefile` that says which files depend on which other files, and how to build your program. 

```bash
main: main.o vec.o # target main depends on vec.o and main.o 
	g++ main.o vec.o -o main # how to buid targetmain 
#the space before g++ MUST BE A TAB CHARACTER

main.o: main.cc veh.h
	g++ -std=c++14 -c main.cc
vec.o: vec.cc vec.h
	g++ -std=c++14 -c vec.cc

.PHONY: clean 

clean: 
	rm *o main 
```

Then from `cmd` line, Make `#builds` the whole project - just change `vec.cc`

- Make # compiles `vec.o` and relinks 
- Make target - builds the requested target, without target (as above), builds the first 
- Make just checks time stamps, if a target is older than its dependencies, it must be rebuilt (can cascade/recurse)

## Generalize with Vars 

``` make
CXX = g++ # compiler name 
CXXFLAGS = -std=c++14 -wall -MMD # compiler flags 
OBJECT = main.o vec.o 
EXEC = main 

${EXEC}: ${OBJECTS}
	${CXX} ${OBJECTS} -o ${EXECS}
	
main.o: main.cc vec.h

vec.o: vec.h vec.cc
```

Can now omit recipes, `make` assumes the recipe is: 
```bash 
${CXX} ${CXXFLAGS} -c _____.cc
```

Great, but the biggest problems is knowing dependencies: we get help from `g++`, `-MMD` flag produces compilation dictionaries. 

```bash 
g++ -MMD -c vec.cc
```

produces `vec.o` and `vec.d`

```bash 
cat vec.d
> vec.o: vec.cc vec.h
```

``` make
CXX = g++ # compiler name 
CXXFLAGS = -std=c++14 -wall -MMD # compiler flags 
OBJECT = main.o vec.o 
EXEC = main 
DEPENDS = ${OBJECTS:.o=.d}

-include `${DEPENDS}`
//clean as before 
```

As your project grows, we only have to add `.o` files to the `OBJECTS` variable

