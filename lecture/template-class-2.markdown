---
title: Template class (part 2)
layout: default
---

## Templated class

Consider the template class LinkedList:

{% highlight cpp %}
template <typename T> // T can be anything, e.g. foo
class LinkedList
{
    private:
    struct node
    {
        T value; // not double, T!
        node *pnext;
    };

    node *head;

    public:
    LinkedList();
    void insert_front(T value); // T!
    ...
    T nth(int n); // T!
    ...
};
...
{% endhighlight %}

We defined linked lists of different types as follows:

{% highlight cpp %}
    LinkedList<string> list1;     // Linked list of strings
    LinkedList<int> list2;        // Linked list of integers
    LinkedList<double> list3;     // Linked list of doubles
    LinkedList<bool> list4;       // Linked list of boolean values
{% endhighlight %}

We can also use a class type to define a linked list of more complex structures.
For instance, a point is defined by its x and y coordinates.
We can create a linked list of points as follows:

{% highlight cpp %}
class Point
{
  double x;
  double y;
};
...

  LinkedList<Point> point_list;    // Linked list of points
{% endhighlight %}

Note that the member functions receive and return variables
of type Point:
{% highlight cpp %}
template <typename T>
class LinkedList
{
    ...
    public:
    ...
    void insert_front(T value); // T!
    ...
    T nth(int n); // T!
    ...
};
...

  LinkedList<Point> point_list;
  Point p, q;

  for (int i = 0; i < 10; i++)
  {
    p.x = i;
    p.y = i*i;
    point_list.insert_front(p);
  }

  q = point_list.nth(2);
...
{% endhighlight %}

We have defined Point with coordinates of type double,
but maybe we also want points with coordinates of type int or float.
Define Point as a template to give this flexibility:

{% highlight cpp %}
template <typename COORD_TYPE>
class Point 
{
  COORD_TYPE x;
  COORD_TYPE y;  
}
...

  Point<double> p;      // Point whose coordinates have type double
  Point<int> q;         // Point whose coordinates have type int
  Point<float> r;       // Point whose coorindates have type r
{% endhighlight %}

We can use the template class Point to define different types
of linked lists of points:
{% highlight cpp %}
template <typename COORD_TYPE>
class Point 
{
  COORD_TYPE x;
  COORD_TYPE y;  
}
...
// Linked list of points whose coordinates have type double
  LinkedList<Point<double> >;    // Note space between '>' and '>'

// Linked list of points whose coordinates have type int
  LinkedList<Point<int> >;  

// Linked list of points whose coordinates have type float
  LinkedList<Point<int> >;  
{% endhighlight %}

Note that there MUST be a space between '>' and '>'.  
A common error is to forget this space.
Unfortunately, the symbol '>>' has a separate meaning in C++.
If you forget the space, the compiler will interpret '>>' 
as the right shift operator
and generate a whole bunch of (unintelligible) error messages.

## Two or more template parameters

A template can have two or more type parameters.
For instance, a circle is defined by the x and y coordinates
of its center and its radius:
{% highlight cpp %}
class Circle
{
  public:
    double x;
    double y;
    double radius;
};
{% endhighlight %}

We may also want a circle whose x and y coordinates are integers
and its radius is type double.
Or perhaps we also want the x and y coordinates to have type double
and the radius to be an integer.
We can define a template class with separate types 
for the coordinates and the radius.
{% highlight cpp %}
template <typename COORD_TYPE, typename RADIUS_TYPE>
class Circle
{
  public:
    COORD_TYPE x;
    COORD_TYPE y;
    RADIUS_TYPE radius;
};
...

  Circle<double, double> c1;  // Coordinates type double, radius type double.
  Circle<int, double> c2;     // Coordinates type int, radius type double.
  Circle<double, int> c3;     // Coordinates type double, radius type int.
{% endhighlight %}

When we create the circle, the k'th type in the template parameter list
is set to be the k'th type in the template argument list.

We can create a linked list of circles as follows:
{% highlight cpp %}
template <typename COORD_TYPE, typename RADIUS_TYPE>
class Circle
{
  public:
    COORD_TYPE x;
    COORD_TYPE y;
    RADIUS_TYPE radius;
};
...

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<Circle<int, double> >

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<Circle<double, int> >
{% endhighlight %}

We can also use the Point class in defining the coordinates 
of the center of the circle:
{% highlight cpp %}
template <typename T>
class Point 
{
  T x;
  T y;  
};

template <typename COORD_TYPE, typename RADIUS_TYPE>
class CircleA
{
  public:
    Point<COORD_TYPE> p;  // Coordinates are p.x and p.y
    RADIUS_TYPE radius;
};
...

  // Circle with coordinates of type int, radius of type double
  CircleA<int, double> c;

  c.p.x = 3;          // Set x-coordinate
  c.p.y = 4;          // Set y-coordinate
  c.radius = 5.5;     // Set radius

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<CircleA<int, double> >

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<CircleA<double, int> >
{% endhighlight %}

A different way to use the class Point is as follows:
{% highlight cpp %}
template <typename T>
class Point 
{
  T x;
  T y;  
};

template <typename POINT_TYPE, typename RADIUS_TYPE>
class CircleB
{
  public:
    POINT_TYPE p;
    RADIUS_TYPE radius;
};
...

  // CircleB with coordinates of type int, radius of type double
  CircleB<Point<int>, double> c;

  c.p.x = 3;          // Set x-coordinate
  c.p.y = 4;          // Set y-coordinate
  c.radius = 5.5;     // Set radius

  // Linked list of circles: coordinates type int, radius type double.
  LinkedList<CircleB<Point<int>, double> >

  // Linked list of circles: coordinates type double, radius type int.
  LinkedList<CircleB<Point<double>, int> >
{% endhighlight %}

Template class CircleB gives more flexibility than template class CircleA.
For instance, CircleB could have different types for coordinates x and y
or could have other data stored with the point.
On the other hand, anyone writing a new class to be used as a POINT_TYPE 
must remember to have x and y fields in the class.
The flexibility of CircleB also creates the potential for more
things to go wrong.

