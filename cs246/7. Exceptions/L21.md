...
```c++
class Text: public Book {
    ...
    public:
        Text & operator=(const Book & o) override {
            Book::operator=o;
            topic = o.topic; // illegal o is a book -> can't do this, but assume we can
            return *this;
        } 
}
```
Note `Text::operator=` is permitted to return (by reference) a subtype object and still be a valid override. But, the parameter types must be the same, or else it is not an override. By the "is-a" priciple, if `Book` can be assigned to by another `Book`, the a `Text` can be assigned to a `Book`.

```c++
Text t{...};
Book b{...};
Text *pt = &t;
*pt = b; // calls operator= with virtual disputed
         // uses a Book to assign to a text, would be BAD
```

Also 
```c++
Comic c{...};
t = c; // Uses a comic object to assign to a text object - BAD!
```
Before `operator=` was `virtual` we had partial assignment through base class pointers, if it is `virtual` we get the problem of mixed assignments. 

*What is the solution?*

Assignment through base class pointers/references dosen't reallt make sense - we need to kow the objects we're assigning are the same type, which defeats the purpose of working polymorphism.

Disallow assignment through base class pointers by making the assignment operator `private` (or more likely, `protected`) in the base class - can stay public and non-virtual in derived class.

```c++
Text t{...};
Text a{...};
t = a; // allowed 

Book *p = &t, z = &a;
*p = *z; // ILLEGAL (which we want)
```

But, this is also illegal to do: 

```c++
Book b{...};
Book d{...};
b = d; // can't assign Book objects - No good!
```

**Recommendation:** All base classes should be `abstract`. 

```c++
class AbstractBook {
    String title, author;
    int numPages;
    protected:
        AbstractBook & operator(const Abstract Book &other);
    public:
        AbstractBook(...);
        virtual ~AbstractBook() = o;
};
// implentation for ~AbstractBook
```

```c++
class NormalBook {
    public:
        NormalBook(...) ...
        ~NormalBook(){}
        NormalBook &operator=(const NormalBook &other) {
            AbstractBook::operator=(other);
            return *this;
        }
}
```

...(9)

```c++
Vector<int> v{d,3};
for (int i = 0; i < 1000; ++i) {
    cout << v[i] << endl;
}
```
`operator[]` for vectors is unchecked - assumes you are indexing a valid index.

`vector::at` is a checked version of indexing.
```c++
for (int i = 0; i < 1000; ++i) {
    cout << v.at()i << endl;
}
```
`at` is checked - but what does it do when you give an invlid index?

In C - the solution is either to set a global error value or return a sentinel error value. `Vector` could validly return any type in its types domain, and global error values lead to ugly code and as such often gets ignored.

C++ Solution - raise an exception!

# Exceptions
An exception is some value raised (thrown) by a function that ends execution of that function and passes that error along to the caller.

`Vector::at` can detect that an error has occured, but it dosen't know what to do about it. The caller can't detect error but knows what they'd like to do in the chance of a failure. So, error handling is an implicitly non-local problem.

`Vector::at` specifically raises a `std::out_of_range` object when a bad access is made. (`std::out_of_range`) is an exception class from the header `<exception>`. What do we do about exceptions?

```c++
void f() {
    vector v{0, 3}l
    v::at(10000);
}

void g() {
    f();
}

int main() {
    g();
}
```
`main` calls `g` which calls `f` which calls `vector::at` raises an exception. What happens? By default, the program stops executing.

More specifically, `vector::at` raises the excepton, `f` recieves it but does not deal with it, so it progogates the exception to `g`, which does that same thing so the execution reaches amin and isent handled there. So, it an exception reached main and main dosent handle it, the program terminates.

```c++
int main() {
    try {
        g();
    } catches (std::out_of_range e) {
        cout << "get error" << e.what() << endl;
    }
}
```

If exception raising code is wrapped in a `try/catch` block, it can handle the exception is it has a ... handler.

What if `g` wants to catch the exception, handle some behaviour, and raise another error?

```c++
void g() {
    try {
        f();
    } catch (std::out_of_range) {
        ...
        throw someError{};
    }
}

int main() {
    try {
        g();
    } catches (someError e) {
        cout << "get error" << endl;
    }
}
```

What if `g` wanted to raise the same error?

```c++
void g() {
    try {
        f();
    } catch (std::out_of_range e) {
        throw;
    }
}
```
Why just `throw` and not `throw e`?
