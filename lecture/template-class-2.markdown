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




