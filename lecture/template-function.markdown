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
inner_product_error1.cpp:25: error: no matching function for call to 
        'inner_product(float [5], int [5], const int&, float&)'
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

## Examples of function template translate

Function templates can be used to perform the same operation
on different objects.
For instance, the following function template
adds (dx,dy) to an objects x and y locations.
{% highlight cpp %}
template <typename T1, typename T2>
void translate(const T1 dx, const T1 dy,
               T2 & object)
{
  object.x += dx;
  object.y += dy;
}
{% endhighlight %}

We can apply translate to any class which has members x and y:
{% highlight cpp %}
...
class Circle
{
public:
  double x;
  double y;
  int radius;
};

class Rectangle 
{
public:
  int x;
  int y;
  int width;
  int height;
};

class Label
{
public:
  float x;
  float y;
  string name;
};


int main()
{
  Circle c;
  Rectangle r;
  Label l;
...
  translate(0.1, 0.2, c);
  translate(1, 2, r);
  translate(0.1, 0.2, l);
...
{% endhighlight %}

Note that classes Circle, Rectangle and Label have different types for x and y
and have additional members other than x and y.
All that is necessary is that the class have a member x and a member y.

To ensure that the classes have the correct members,
it is preferable to create a class Point with members x and y,
and then derive the other classes from that class.
{% highlight cpp %}
...
template <typename T>
class Point
{
public:
  T x;
  T y;
};

class Circle:public Point<double>
{
public:
  int radius;
};

class Rectangle:public Point<int> 
{
public:
  int width;
  int height;
};

class Label:public Point<float>
{
public:
  string name;
};
...
  translate(0.1, 0.2, c);
  translate(0.1, 0.2, l);
  translate(1, 2, r);
...
{% endhighlight %}

Consider again procedure translate:
{% highlight cpp %}
template <typename T1, typename T2>
void translate(const T1 dx, const T1 dy,
               T2 & object)
{
  object.x += dx;
  object.y += dy;
}
{% endhighlight %}

Note that dx and dy have the same types T1.
This can cause a compiler error if dx is an integer and dy is a float
of vice versa.
{% highlight cpp %}
44.  translate(1, 0.2, c);
45.  translate(0.1, 0, l);
{% endhighlight %}

Compiling the above code generates the following compiler errors:
{% highlight cpp %}
>  g++ translate_error1.cpp
translate_error1.cpp: In function 'int main()':
translate_error1.cpp:44: error: no matching function 
    for call to 'translate(int, double, Circle&)'
translate_error1.cpp:45: error: no matching function 
    for call to 'translate(double, int, Label&)'
{% endhighlight %}

One solution is to use only floats.
The following code compiles without errors:
{% highlight cpp %}
translate(1.0, 0.2, c);
translate(0.1, 0.0, l);
{% endhighlight %}

Another solution is to use separate types for dx and dy.
In the following code, parameter dx has template type T1
while parameter dy has template type T2.
The code compiles without errors:
{% highlight cpp %}
template <typename T1, typename T2, typename T3>
void translate(const T1 dx, const T2 dy,
               T3 & object)
{
  object.x += dx;
  object.y += dy;
}
...
  translate(1, 0.2, c);
  translate(0.1, 0, l);
...
{% endhighlight %}


