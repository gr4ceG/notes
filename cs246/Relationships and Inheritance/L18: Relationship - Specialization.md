<style>
r { color: #F95050 }
navy { color: #000099 }
b { color: #50C1F9 }
o { color: #F7A63B }
g { color: #32a852 }
</style>

## Relationship - Specialization
```c++
class Book {
    string title, author;
    int numPages;
    public:
        Book(...);
    ...
};
```

For textbooks we want to keep track of topics:
```c++
class Text {
    string title, author;
    int numPages;
    string topic;
    public: 
        Text(...);
    ...
};
```

For comic books, we want to keep track of the hero:
```c++
class Comic {
    string title, author;
    int numPages;
    string hero;
    public: 
        Comic(...);
    ...
};
```

This is OKAY, but it dosen't capture the relationship among comics, texts and books. Moreover, how do we create an array (or other collection) that contains a mixture of these.

We could:
- use a union 
```c
union BookTypes {Book *, Text *, Comic *};
BookTypes myBooks[20];
```
- array of `void *`, can point to anything

These are not good solutions - they subvert the type system.
Rather, observe Texts and Comics are kinds of books - books with extra features. So to model this in c++ we use *inheritance*.

```c++
// BASE CLASS (super class)
class Book {
    string title, author;
    int numPages;
    public:
        Book(...);
    ...
};

// DERIVED CLASS: (Sub class, Child class)
class Text: public Book {
    string topic;
    public: 
        Text(...);
    ...
};

// DERIVED CLASS: (Sub class, Child class)
class Comic {
    string hero;
    public: 
        Comic(...);
    ...
};
```

Derived classes inherit fields and methods from their base class. So, `Text` and `Comic` get the `title`, `author`, `numPages` fields from `Book`. 

Any method that can be called on a `Book` can be called on a `Comic` or `Text`s.

Who can access the `title`, `author`, and `numPages` fields? They're private in `Book`, so outdsiders can't see them, but `Text`s and `Comic`s ARE `Book`s, so can they access them?

<r>No! **Even subclasses can't see the private members of their superclasses**</r>

How do we initialize a `Text` object? It has `title`, `author`, and `numPages` - but we can't access them in `Text`!

```c++
// BASE CLASS (super class)
class Book {
    string title, author;
    int numPages;
    public:
        Book(string title, string author, in numPages) :
            title{title}, author{author}, numPages{numPages} {}
};

// DERIVED CLASS: (Sub class, Child class)
class Comic {
    string hero;
    public: 
        Comic(string title, string author, int numPages, string hero) :
            Book{title, author, numPages}, hero{hero}{}
};
```
The above is nessessary for two reasons:
1. `title`, etc. are not accessible by `Comic`
2. Once again, when an object is constructed:
   1. Space is allocated
   2. Superclass components are initialized
   3. Fields are initialized
   4. Constructor body runs

And (ii) dosen't work if we don't specify it because if unspecified, the compiler tries to default construct our supperclass and `Book` has no default constructor. 

There are good reasons to keep superclass fields hidden from derived classes (encapsulation - less places to look for bugs regarding those fields). But, if you want to give a subclass access to certain members, you can use `protected` visibility.

```c++
class Book {
    Protected: // accessible to Book and its derived classes
        string title, author;
        int numPages;
    public:
        Book(...);
    ...
};

class Text: public Book {
    string text;
    public:
        ...
        void addAuthor(const string &a) {
            author += a; // OK, because author is assessible
        }
};
```

Better for encapsulation to keep fields `private` and provide `protected` getters and setters. That way, those methods can maintain invariants. 

```c++
class Book {
    string title, author; // private fields
    int numPages // private fields
    protected:
        void setAuthor(const String &a) {author = a;}
        string getAuthor(return author;)
    public:
    ...
};

...

void Text::setAuthor(const string &a) {
    setAuthor(getAuthor() + a);
}
```

The relationship among comic, text, and book is called a "is-a" relationship. A text is a Book and a comic is a book - in UML, 

... (4)

The "is-a" relationship i simplemented via public inheritance. Now, consider we want to write a method, `isHeavy`. When is a Book heavy?
- For ordinary books, "heavy" means > 200 pages
- For textbooks, "heavy" means > 500 pages
- For Comics, "heavy" means > 50 pages

```c++
class Book {
    ...
    public:
        ...
        bool isheavy() {return numPages > 200;}
};

class Text {
    ...
    public:
        ...
        bool isheavy() {return numPages > 500;} // Assume numPages is protected
};

class Comic {
    ...
    public:
        ...
        bool isheavy() {return numPages > 50;} // Assume numPages is protected
};

Book b{"A Small Book", "Dave", 50};
Comic c{"Thing", "Person", 60, "Recycling"};
cout << b.isHeavy() // false
     << c.isHeavy() // true
```
Because public inheritance is an "is-a" relationship, we can write 
```c++
Book b = Comic{"Big Comic", ..., 75, ...};
```
Question: is `b` heavy? i.e does `b.isHeavy` execute `Book::isHeavy` or `Comic::isHeavy`?

Answer: NO `b` is a `Book` - so `Book::isHeavy` that runs

`Book b = Comic(...);` tries to fit a comic object where there is only space for a Book object, the Comic is *sliced*: the hero field is chopped off and the comic is converted into a Book.

But, accessing through ptrs, no slicing is required
```c++
Comic c{...,60,...};
Book *pb = &c;
Comic *pc = &c;
cout << pc->isHeavy() // true
     << pb->isHeavy(); // false
```
And it still prints `false` when called through a `Book *` (even through it points at a comic).
```c++
Book *pb;
int n;
cin >> n;
if (n < 0) {pb = new Book{..., 60};}
else {pb = new Comic{...,60,...};}
pb->isHeavy(); 
```
The compiler doesn't know if `pb` points at a `Book`, `Comic`, or `Text`, it only knows `pb`'s static type is that of `Book *`, so it calls `Book::isHeavy`. So when pointed at by a class ptr (or ref), these objects behave as their base class. How do we make a comic act like a comic even when pointed at by a base class pointer?

Solution: make the method *virtual*

...(5)