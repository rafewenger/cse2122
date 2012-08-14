---
title: C++ classes
layout: default
---

A C++ class is a way of putting together data into a single object.
For instance,
{% highlight cpp %}
class Person
{
public:
    string name;
    int age;       // in years
    double height; // in cm
    double weight; // in kg
};
{% endhighlight %}
*name*, *age*, *height* and *weight* are called the *class attributes*
or *data members* of class *Person*.
Note that the class definition must be followed by a semicolon.

## Class Circle

A class can be composed of other classes:
For instance,
{% highlight cpp %}
class Point {
public:
    double x;
    double y;
};

class Circle
{
public:
    double radius;
    Point center;  // circle center
};
{% endhighlight %}

We use a period to access the attributes of the class:
{% highlight cpp %}
Circle c;

c.radius = 2.5;
c.center.x = 1.1;
c.center.y = 2.2;
{% endhighlight %}

We can pass a class to a function.
We usually pass a class by reference. (Why?)
{% highlight cpp %}
double compute_distance(Point & p1, Point & p2)
{
  double diffx = p1.x - p2.x;
  double diffy = p1.y - p2.y;
  double dist = sqrt(diffx*diffx + diffy*diffy);

  return(dist);
}
{% endhighlight %}

We can use the function *compute_distance* 
to determine if a circle contains a point:
{% highlight cpp %}
bool circle_contains(Circle & c, Point & q)
{
  double dist = compute_distance(c.center, q);
  if (dist <= c.radius) 
    { return(true); }
  else
    { return(false); }
}
{% endhighlight %}
Note that the function *compute_distance* 
does NOT know that *p1* is the center of a circle.

Program [circleContains.cpp](../code/cpp-classes/circleContains)
is an example of a program using *compute_distance* and *circle_contains*.

We could also define a member function *contains(q)* for class *Circle*
which returns true if the circle contains point *q*.
{% highlight cpp %}
class Circle
{
public:
    double radius;
    Point center;  // circle center

    bool contains(Point & q);
};

Circle::contains(Point & q)
{
  double dist = compute_distance(center, q);
  if (dist <= radius) 
    { return(true); }
  else
    { return(false); }
}
{% endhighlight %}

Program [circleContains2.cpp](../code/cpp-classes/circleContains-2)
is an example of a program using the member function *Circle::contains*.

## Class LineSegment

Another example of a class using *Point* and *compute_distance* is:
{% highlight cpp %}
class LineSegment
{
public:
  Point endpoint1;
  Point endpoint2;

  double length();  // return the line segment length
};

double LineSegment::length()
{
  double L = compute_distance(endpoint1, endpoint2);
  return(L);
}
{% endhighlight %}
Again, the function *compute_distance* has no idea that the two points
are attributes of the class *LineSegment*.

Program [lineSegment.cpp](../code/cpp-classes/lineSegment)
is an example of a program using the member function *LineSegment::length*.

An alternative representation of the endpoints in LineSegment uses an array:
{% highlight cpp %}
class LineSegment
{
public:
  Point endpoint[2];

  double length();  // return the line segment length
};

double LineSegment::length()
{
  double L = compute_distance(endpoint[0], endpoint[1]);
  return(L);
}
{% endhighlight %}

Program [lineSegment2.cpp](../code/cpp-classes/lineSegment-2)
is an example of a program using this alternative version of class *LineSegment*.
