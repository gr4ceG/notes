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


