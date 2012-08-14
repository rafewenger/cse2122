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



