# Static Members
```c++
struct Student {
    ...
    mutable int numMethodCalls = 0;
    ...
    float grade() const {
        ++numMethodCalls;
        return ...;
    }
    Student s{...};
    Student b{...};
    s.grade();
    b.grade();
    s.grade();
}
```
The count `numMethdoCalls` is for a particular object. Hard to keep track of all method calls across all `Student` objects this way. What if we want to keep track of how many `Student` objects are created.

What we want is a variable assocaited with the class itself, not a particular instance of that class (object). Static members are exactly that.
```c++
struct Student {
    static int numInstances;
    Student(int assns, int mt, int final) : ... {
        ++numInstances;
    }
}
```

```c++
// student.cc
int Student::numInstance = 0;
```
`static` fields must be initailized outside the class, `static` member functions don't depend on a specific instance for their computation, (They do not have an implicit `this` parameter, thus they can only access `static` fields and can only call other static members)

```c++
struct Student {
    ...
    static void howMany() {
        cout << numInstances << endl;
    }
}

Student Billy{ 60, 70, 80 };
Student Yei{ 50, 80, 90 };

Student::howMany(); // 2
Yei.howMany()l // 2
```

```c++
struct Foo {
    static int x;
    void doSomething() const {
        ++x;
        cout << "Hello" << endl;
    }
};

int Foo::x = 0;

int main() {
    Foo f;
    f.doSomething();
}
```
Changing a static field is not changing the object, hence can change a `static` field in a `const` method (`const` promises that the `this` pointer is const)

## Input and Output Operator (`>>`, `<<`)
```c++
class Vec {
    int x, y;
    public:
        ...
        int getX() const { return x; } // assessor (getter)
        int setY(int z) { y = z; }; // mutator (setter)
};
```
Providing assess to private fields can be done through assessor/mutator methods when appropriate. These methods can include code to check/maintain invariants. 

What about input/output operators? 
Whyd do we not declare input/output operators as class members: whenever you declare a method in the class, the first operand is the object, whereas for overloading operators, first operand should be the strema,

Those opertators shouldn't be members because the first operand shouldn't be the object, so, we can declare `friend` functions.
```c++
class Vec {
    ...
    friend std::ostream & operator<<(std::ostream &, Vec &);
};

ostream & operator<<(std::ostream & out, Vec & v) {
    return out << v.x << ", " << v.y;
}
```

If you already have all the getter and setter methods that you need to write I/O operators, then you don't need to declare it as a friend, but don't add getter and setters just for the I/O opertors because then everyone can access them, not just the I/O operator.