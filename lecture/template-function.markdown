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

