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
  Point endpoint0;
  Point endpoint1;

  double length();  // return the line segment length
};

double LineSegment::length()
{
  double L = compute_distance(endpoint0, endpoint1);
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
is an example of a program using this alternative version 
of class *LineSegment*.

Note that someone using *LineSegment::length* does not need to know
whether the endpoints are stored as *endpoint0* and *endpoint1*
or as an array *endpoint[2]*.
To set and read the endpoints one still needs this information.


## Information hiding

We can hide the attributes of class *LineSegment* by making them private.
{% highlight cpp %}
class LineSegment
{
private:
  Point endpoint0;
  Point endpoint1;

public:
  void setEndpoint(int iend, Point & q);
  Point getEndpoint(int iend);
  double length();  // return the line segment length
};
{% endhighlight %}

We need to add functions *setEndpoint()* and *getEndpoint()*
to access the attributes:
{% highlight cpp %}
void LineSegment::setEndpoint(int iend, Point & q)
{
  if (iend == 0)
    { endpoint0 = q; }
  else
    { endpoint1 = q; }
}

Point LineSegment::getEndpoint(int iend)
{
  if (iend == 0)
    { return(endpoint0); }
  else
    { return(endpoint1); }
}
{% endhighlight %}

The endpoints can only be accessed through the member functions
*setEndpoint*, *getEndpoint* and *length*.
{% highlight cpp %}
  // Read line segment
  read_point("Enter first endpoint (x,y): ", q);
  segA.setEndpoint(0, q);
  read_point("Enter second endpoint (x,y): ", q);
  segA.setEndpoint(1, q);

  // Output endpoints and length
  q = segA.getEndpoint(0);
  cout << "First endpoint: (" << q.x << "," << q.y << ")" << endl;
  q = segA.getEndpoint(1);
  cout << "Second endpoint: (" << q.x << "," << q.y << ")" << endl;
  cout << "Length: " << segA.length() << endl;
{% endhighlight %}
Someone using class *LineSegment* does not need to have any knowledge
about how the endpoints are stored.
She just needs to know how to use the member functions
*setEndpoint*, *getEndpoint* and *length*.

Program [examplePrivate.cpp](../code/cpp-classes/examplePrivate)
is an example of a program using the new version of class *LineSegment*.

In this version of class *LineSegment*,
endpoints are stored in two attributes, *endpoint0* and *endpoint1*.
However, it would actually be easier to use an array *endpoint[2]*
to store the endpoints.
Changing the attributes requires changing the code in the member functions 
*setEndpoint*, *getEndpoint* and *length*.
{% highlight cpp %}
class LineSegment
{
private:
  Point endpoint[2];

public:
  void setEndpoint(int iend, Point & q);
  Point getEndpoint(int iend);
  double length();  // return the line segment length
};

void LineSegment::setEndpoint(int iend, Point & q)
{
  endpoint[iend] = q;
}

Point LineSegment::getEndpoint(int iend)
{
  return(endpoint[iend]);
}

double LineSegment::length()
{
  double L = compute_distance(endpoint[0], endpoint[1]);
  return(L);
}
{% endhighlight %}
Note that the implementation 
of *LineSegment::setEndpoint* and *LineSegment::getEndpoint*
is simpler than in the previous version.

An example of accessing the endpoints is:
{% highlight cpp %}
  // Read line segment
  read_point("Enter first endpoint (x,y): ", q);
  segA.setEndpoint(0, q);
  read_point("Enter second endpoint (x,y): ", q);
  segA.setEndpoint(1, q);

  // Output endpoints and length
  q = segA.getEndpoint(0);
  cout << "First endpoint: (" << q.x << "," << q.y << ")" << endl;
  q = segA.getEndpoint(1);
  cout << "Second endpoint: (" << q.x << "," << q.y << ")" << endl;
  cout << "Length: " << segA.length() << endl;
{% endhighlight %}
This code to read and output a line segment is EXACTLY the same code as above.
There have been NO changes.
When all the attributes of a class are private and only accessed
through member functions,
only the member functions must be changed when the attributes change.

Hiding all the attributes of a class is extremely powerful.
It allows someone to modify a class without needing to check
all the code that uses that class.
As long as the member functions perform the same,
it does not matter how the data is represented.

Program [examplePrivate2.cpp](../code/cpp-classes/examplePrivate-2)
is an example of a program using the class *LineSegment*
with array *endpoint[2]*.


