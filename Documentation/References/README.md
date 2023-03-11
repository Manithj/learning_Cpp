## <u>References</u>

A reference variable is an alias, that is, another name for an already existing variable. Once a reference is initialized with a variable, either the variable name or the reference name may be used to refer to the variable.

### Creating References in C++

```c++
#include <iostream>
 
using namespace std;
 
int main () {
   // declare simple variables
   int    i;
   double d;
 
   // declare reference variables
   int&    r = i;
   double& s = d;
   
   i = 5;
   cout << "Value of i : " << i << endl;
   cout << "Value of i reference : " << r  << endl;
 
   d = 11.7;
   cout << "Value of d : " << d << endl;
   cout << "Value of d reference : " << s  << endl;
   
   return 0;
}
```

#### Output

```
Value of i : 5
Value of i reference : 5
Value of d : 11.7
Value of d reference : 11.7
```

### References vs Pointers

- You cannot have NULL references. You must always be able to assume that a reference is connected to a legitimate piece of storage.

- Once a reference is initialized to an object, it cannot be changed to refer to another object. Pointers can be pointed to another object at any time.

- A reference must be initialized when it is created. Pointers can be initialized at any time.