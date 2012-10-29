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

