---
title: Function templates
layout: default
---

## Function templates

Templates can also be used to define functions
which work on different types of objects.
Consider the following function to swap two values of type double:
{% highlight cpp %}
void mySwap(double & a, double & b)
{
  double temp = a;
  a = b;
  b = temp;
}
{% endhighlight %}

If we want to swap two integers, we need to overload function
mySwap with another definition.
{% highlight cpp %}
void mySwap(int & a, int & b)
{
  double temp = a;
  a = b;
  b = temp;
}
{% endhighlight %}


We need to do the same if we want to swap two variables of type float,
type bool, type string, etc.
Large Fortran and C programs and libraries are often cluttered
with numerous versions of the same function for different parameter types.

Function templates removes the need for multiple definitions
of the same function with different parameter types.
The function template for mySwap is:
{% highlight cpp %}
template <typename T>
void mySwap(T & a, T & b)
{
  T temp = a;
  a = b;
  b = temp;
}
...
  int i(5), j(8);
  float x(3.3), y(4.4);
  double u(5.5), v(6.6);
  bool b1(true), b2(false);

  mySwap(i,j);
  mySwap(x,y);
  mySwap(u,v);
  mySwap(b1,b2);
...
{% endhighlight %}

The template mySwap is so useful that it is defined
as template function "swap" in include file "algorithm"
in the Standard Template Library (STL).
To make your code easier for others (and you) to read,
always use the function "swap" from the Standard Template Library.
Do not define your own function "swap" or "mySwap".

{% highlight cpp %}
#include <algorithm>    // File containing function template swap.
using namespace std;    // Function template swap is in the std namespace.
...
  int i(5), j(8);
  float x(3.3), y(4.4);
  double u(5.5), v(6.6);
  bool b1(true), b2(false);

  swap(i,j);
  swap(x,y);
  swap(u,v);
  swap(b1,b2);
...
{% endhighlight %}

## Example of computing the inner product

Assume you want to compute the inner product of two arrays.
The following code computes the inner product of arrays of type double:
{% highlight cpp %}
void inner_product(const double u[], const double v[], 
                   const int n, double & result)
{
  result = 0;
  for (int i = 0; i < n; i++)
    { result = result + u[i]*v[i]; }
}
{% endhighlight %}

If you want a function to compute the inner product of two arrays
of type int, you need to create a copy of the function
with arguments of type int.
You need another copy for the inner product of arrays of type float
and of type short and of type long, etc.
Fortran libraries contain multiple copies of functions such as inner_product
with slightly different names 
(operation overloading is not supported in Fortran)
and different parameter types.

In C++ you can create one template function which computes
the inner product for all such types:
{% highlight cpp %}
template <typename T>
void inner_product(const T u[], const T v[], const int n, T & result)
{
  result = 0;
  for (int i = 0; i < n; i++)
    { result = result + u[i]*v[i]; }
}
...
  double x[LENGTH], y[LENGTH], prod_x_y;
  float u[LENGTH], v[LENGTH], prod_u_v;
  int a[LENGTH], b[LENGTH], prod_a_b;
...
  inner_product(x, y, LENGTH, prod_x_y);
  inner_product(u, v, LENGTH, prod_u_v);
  inner_product(x, y, LENGTH, prod_a_b);
...
{% endhighlight %}

However, the function above cannot be used to compute
the inner product of two arrays of different types.
For instance, the following example generates a compiler error:
{% highlight cpp %}
 6. template <typename T>
 7. void inner_product(const T u[], const T v[], const int n, T & result)
 8. {
 9.   result = 0;
10.  for (int i = 0; i < n; i++)
11.    { result = result + u[i]*v[i]; }
12. }
...
17.   float x[LENGTH], result;
18.   int a[LENGTH];
...
25.   inner_product(x, a, LENGTH, result);
...
{% endhighlight %}

The compiler error is:
{% highlight cpp %}
> g++ inner_product_error1.cpp
inner_product_error1.cpp: In function 'int main()':
inner_product_error1.cpp:25: error: no matching function for call to 'inner_product(float [5], int [5], const int&, float&)'
{% endhighlight %}

The problem is that the template specification requires both u and v
to be arrays of the same type and for result to have the same type as well.
We can modify template to compute the inner product of two arrays with
different types as follows:
{% highlight cpp %}
template <typename T1, typename T2, typename T3>
void inner_product(const T1 u[], const T2 v[], const int n, T3 & result)
{
  result = 0;
  for (int i = 0; i < n; i++) {
    result = result + u[i]*v[i]; 
  }
}
...
   float x[LENGTH];
   int a[LENGTH];
   double result;
...
   inner_product(x, a, LENGTH, result);
{% endhighlight %}
Note that the result also may have a different type.
This creates the potential for numerical error if result has type int
while the arrays have type float or double.

## Examples of function templates

Function templates can be used to perform the same operation
on different objects.
For instance, the following function template prints
the x and y coordinates of the object in the argument:
{% highlight cpp %}
template <typename T>
void print_coord(const T & object)
{
  cout << "x: " << object.x << endl;
  cout << "y: " << object.y << endl;
}
{% endhighlight %}

We can apply print_coord to any class which has members x and y:
{% highlight cpp %}
...
class Label
{
public:
  float x;
  float y;
  string name;
};

class Circle
{
public:
  int x;
  int y;
  int radius;
};

int main()
{
  Label l;
  Circle c;

  c.x = l.x = 3;
  c.y = l.y = 4;
  c.radius = 10;
  l.name = "Columbus";

  print_coord(c);
  print_coord(l);
...
{% endhighlight %}
Note that classes Label and Circle have different types for x and y
and have additional members other than x and y.
All that is necessary is that the class have a member x and a member y.
The type of x could even be different from the type of y:
{% highlight cpp %}
...
class Point {
  public:
    int x;
    double y;
};
...

  Point p;
  p.x = 5;
  p.y = 6.6;
  print_coord(p);
...
{% endhighlight %}
Note that cout is a C++ template which is why 
it works on objects of different types.

