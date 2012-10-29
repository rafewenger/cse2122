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

  LinkedList<Point> list_point;    // Linked list of points
{% endhighlight %}
