# variables, references and pointers

When we define (declare) a variable the system reserves space in memory to store the value associated with that variable. This is the reason why we need to specify the type of the variable since the required space depends on it. For example (typically), an ```int``` and ```float``` need 4 bytes whereas ```long``` and ```double``` need 8 bytes. Because every variable is associated with a location in memory we can determine the memory address where the variable is located using the & operator. Note that the & operator can have different meaning depending on context.
## Variables and references
```
#include <iostream>
int main(){
//a location in memory is reserved and labeled x
int x=2;
//y is just another name for the same location. no reservation is done.
int& y=x;
//a location is reserved for z and the value of x is copied
int z=x;
z=17;
y=13;
//print the value of the variables and their respective addresses
std::cout<<"x= "<<x<<" and <<"&x="<<&x<<std::endl;
std::cout<<"x= "<<y<<" and <<"&x="<<&y<<std::endl;
std::cout<<"x= "<<z<<" and <<"&x="<<&z<<std::endl;

}
```
Note that ```int& y=x;``` declares _y_ as a reference to _x_ whereas ```&x``` gives the memory address of _x_. The different declarations used above carry to the parameters in function calls. For example,
```
#include <iostream>
void byValue(int n){
    n=17;
}
void byRef(int& n){
    n=12;
}
int main(){
  int x=2;
  byValue(x);
  std::cout<<x<<std::endl;
  byRef(x);
    std::cout<<x<<std::endl;

}
```
So in the call to the function ```byValue(x)``` it is __as if__ we declare ```int n=x;``` and therefore _n_ is a __copy__ of _x_. By contrast, ```byRef(x)``` is is __as if__ we declare ```int& n=x;``` so no copy is made and _n_ is a reference to _x_.
Usually we call by reference when either we want to change the input or  when the input is large and copying becomes expensive. We can use the best of both by using a const reference
```
int byCRef(const int& n){
    n=37;//error cannot modify n
    return 2*n;
}
```
Also, const allows us to pass literals and temporaries.
```
int byT(int n){
    return 7*n;
}
int byCRef(const& n){
    n=n+1;//error n is const
    return 2*n;
}
int byRef(int& n){
    n=n+1;//changes the value of parameter
    return 2*n;
}
int main(){
    byRef(2);//error cannot bind a non-const lvalue to rvalue
    byCRef(2);//OK
    byRef(byT(2));//error since the return value of byT is a temp
    byCRef(byT(2));//OK
}
```
## Return values


## Pointers
A pointer variable is a variable that holds and address. We say variable _p_ points to variable _x_ if _p_ holds the address of _x_: ```int *p=&x;```.
```
int main(){
int x=17,y=45;
int* p=&x;
std::cout<<p<<std::endl;//prints the value of p, i.e. the address of x
std::cout<<*p<<std::endl;//prints the value store at the location p, i.e. x
*p=23;//change the value of x
p=&y;//p now stores the address of y
}
```
Pointers usually are used when we need to dynamically allocate memory.
```
int main(){
    int *p=new int;//reserve space for int. Value undefined
    int *q=new int(8);//reserve space for int and store 8
    *p=55;//store value 55 at address p
    delete p;//release the reserved memory;
}
```
## Templates

On many occasions we write multiple versions of the same code to handle different types. For example suppose we want to write a function to add two numbers (using the + operator) we write

```
int add(int x,int y){
    return x+y;
}
int add(double x,double y){
    return x+y;
}

```
Recall also that the + operator can be used to concatenate strings so we have to add that also. Since the all of those versions only the type changes, c++ allows us to pass the type as a parameters using templates.

```
#include <iostream>
#include <string>

template<typename T>
T add(T x,T y){
    return x+y;
}
int main(){
    int x=2,y=3;
    double u=3.4,v=3;
    std::string s="hello",k="there";

    std::cout<<add(x,y)<<std::endl;
    std::cout<<add(u,v)<<std::endl;
    std::cout<<add(s,k)<<std::endl;
}

```

In the above example the compiler automatically deduces the type which sometimes it cannot and we have to specify it as follows:

```
add<int>(x,y);
add<double(u,v);
add<std::string>(s,k);

```

Note that the template is instantiated __as needed__ at compile time. Also, we can pass parameters to the template other than types. For example

```
template <int n>
void doit(){
    int a[n];
}
```

# Classes

