## <u>Pointers</u>

In C++, pointers are variables that store the memory addresses of other variables.

Pointers are used for accessing the resources which are external to the program – like heap memory. So, for accessing the heap memory (if anything is created inside heap memory), pointers are used. When accessing any external resource we just use a copy of the resource. If we make any changes to it, we just change it in the copied version. But, if we use a pointer to the resource, we’ll be able to change the original resource.

### Declaration

```c++
int *pointVar;
int* pointVar; // preferred syntax
int* pointVar, p;
```

### Address in C++

If we have a variable var in our program, &var will give us its address in the memory. For example,

```c++
#include <iostream>
using namespace std;

int main()
{
    // declare variables
    int var1 = 3;
    int var2 = 24;
    int var3 = 17;

    // print address of var1
    cout << "Address of var1: "<< &var1 << endl;

    // print address of var2
    cout << "Address of var2: " << &var2 << endl;

    // print address of var3
    cout << "Address of var3: " << &var3 << endl;
}
```
#### Output

```
Address of var1: 0x7fff5fbff8ac
Address of var2: 0x7fff5fbff8a8
Address of var3: 0x7fff5fbff8a4
```

### Working of C++ Pointers

```c++
#include <iostream>
using namespace std;
int main() {
    int var = 5;

    // declare pointer variable
    int* pointVar;

    // store address of var
    pointVar = &var;

    // print value of var
    cout << "var = " << var << endl;

    // print address of var
    cout << "Address of var (&var) = " << &var << endl
         << endl;

    // print pointer pointVar
    cout << "pointVar = " << pointVar << endl;

    // print the content of the address pointVar points to
    cout << "Content of the address pointed to by pointVar (*pointVar) = " << *pointVar << endl;
    
    return 0;
}
```

#### Output

```
var = 5
Address of var (&var) = 0x61ff08

pointVar = 0x61ff08
Content of the address pointed to by pointVar (*pointVar) = 5
```

## <u>Smart Pointers</u>

As we’ve known unconsciously not deallocating a pointer causes a memory leak that may lead to a crash of the program. Languages Java, C# has Garbage Collection Mechanisms to smartly deallocate unused memory to be used again. The programmer doesn’t have to worry about any memory leaks. C++ comes up with its own mechanism that’s Smart Pointer. When the object is destroyed it frees the memory as well. So, we don’t need to delete it as Smart Pointer does will handle it.

A Smart Pointer is a wrapper class over a pointer with an operator like * and -> overloaded. The objects of the smart pointer class look like normal pointers. But, unlike Normal Pointers it can deallocate and free destroyed object memory.

The idea is to take a class with a pointer, destructor, and overloaded operators like * and ->. Since the destructor is automatically called when an object goes out of scope, the dynamically allocated memory would automatically be deleted (or the reference count can be decremented).

```c++
// C++ program to demonstrate the working of Smart Pointer
#include <iostream>
using namespace std;

class SmartPtr {
	int* ptr; // Actual pointer
public:
	// Constructor: Refer https:// www.geeksforgeeks.org/g-fact-93/
	// for use of explicit keyword
	explicit SmartPtr(int* p = NULL) { ptr = p; }

	// Destructor
	~SmartPtr() { delete (ptr); }

	// Overloading dereferencing operator
	int& operator*() { return *ptr; }
};

int main()
{
	SmartPtr ptr(new int());
	*ptr = 20;
	cout << *ptr;

	// We don't need to call delete ptr: when the object
	// ptr goes out of scope, the destructor for it is automatically
	// called and destructor does delete ptr.

	return 0;
}
```

#### Output
```
20
```

### Types of Smart Pointers

C++ libraries provide implementations of smart pointers in the following types:

- auto_ptr
- unique_ptr
- shared_ptr
- weak_ptr

### <u>auto_ptr</u>

auto_ptr is a smart pointer that manages an object obtained via a new expression and deletes that object when auto_ptr itself is destroyed.

```c++
// C++ program to demonstrate the use of auto_ptr
// C++ program to illustrate the use of auto_ptr
#include <iostream>
#include <memory>
using namespace std;

class A {
public:
	void show() { cout << "A::show()" << endl; }
};

int main()
{
	// p1 is an auto_ptr of type A
	auto_ptr<A> p1(new A);
	p1->show();

	// returns the memory address of p1
	cout << p1.get() << endl;

	// copy constructor called, this makes p1 empty.
	auto_ptr<A> p2(p1);
	p2->show();

	// p1 is empty now
	cout << p1.get() << endl;

	// p1 gets copied in p2
	cout << p2.get() << endl;

	return 0;
}
```

#### Output
```
A::show()
0x1b42c20
A::show()
0          
0x1b42c20
```
### <u>unique_ptr</u>

unique_ptr stores one pointer only. We can assign a different object by removing the current object from the pointer. 

```c++
// C++ program to demonstarte the working of unique_ptr
// Here we are showing the unique_pointer is pointing to P1.
// But, then we remove P1 and assign P2 so the pointer now
// points to P2.

#include <iostream>
using namespace std;
// Dynamic Memory management library
#include <memory>

class Rectangle {
	int length;
	int breadth;

public:
	Rectangle(int l, int b)
	{
		length = l;
		breadth = b;
	}

	int area() { return length * breadth; }
};

int main()
{
// --\/ Smart Pointer
	unique_ptr<Rectangle> P1(new Rectangle(10, 5));
	cout << P1->area() << endl; // This'll print 50

	// unique_ptr<Rectangle> P2(P1);
	unique_ptr<Rectangle> P2;
	P2 = move(P1);

	// This'll print 50
	cout << P2->area() << endl;

	// cout<<P1->area()<<endl;
	return 0;
}
```

#### Output
```
50
50
```

### <u>shared_ptr</u>

By using shared_ptr more than one pointer can point to this one object at a time and it’ll maintain a Reference Counter using the use_count() method. 

```c++
// C++ program to demonstarte the working of shared_ptr
// Here both smart pointer P1 and P2 are pointing to the
// object Adddition to which they both maintain a reference
// of the object
#include <iostream>
using namespace std;
// Dynamic Memory management library
#include <memory>

class Rectangle {
	int length;
	int breadth;

public:
	Rectangle(int l, int b)
	{
		length = l;
		breadth = b;
	}

	int area() { return length * breadth; }
};

int main()
{
	//---\/ Smart Pointer
	shared_ptr<Rectangle> P1(new Rectangle(10, 5));
	// This'll print 50
	cout << P1->area() << endl;

	shared_ptr<Rectangle> P2;
	P2 = P1;

	// This'll print 50
	cout << P2->area() << endl;

	// This'll now not give an error,
	cout << P1->area() << endl;

	// This'll also print 50 now
	// This'll print 2 as Reference Counter is 2
	cout << P1.use_count() << endl;
	return 0;
}
```

#### Output
```
50
50
50
2
```

### <u>weak_ptr</u>

Weak_ptr is a smart pointer that holds a non-owning reference to an object. It’s much more similar to shared_ptr except it’ll not maintain a Reference Counter. In this case, a pointer will not have a stronghold on the object.

```c++
// C++ program to demonstarte the working of weak_ptr
// Here both smart pointer P1 and P2 are pointing to the
// object Adddition to which they both does not maintain
// a reference of the object
#include <iostream>
using namespace std;
// Dynamic Memory management library
#include <memory>

class Rectangle {
	int length;
	int breadth;

public:
	Rectangle(int l, int b)
	{
		length = l;
		breadth = b;
	}

	int area() { return length * breadth; }
};

int main()
{
	//---\/ Smart Pointer
	shared_ptr<Rectangle> P1(new Rectangle(10, 5));

	// This'll print 50
	cout << P1->area() << endl;

	auto P2 = P1;
	// P2 = P1;

	// This'll print 50
	cout << P2->area() << endl;

	// This'll now not give an error,
	cout << P1->area() << endl;

	// This'll also print 50 now
	// This'll print 2 as Reference Counter is 2
	cout << P1.use_count() << endl;
	return 0;
}
```

#### Output
```
50
50
50
2
```




