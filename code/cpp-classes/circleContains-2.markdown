---
title: circleContains2.cpp (Example of member function.)
layout: default
---

{% highlight cpp %}

#include <iostream>
#include <cmath>

using namespace std;

class Point
{
public:
  double x;
  double y;
};

class Circle
{
public:
  Point center;
  double radius;

  bool contains(Point & q);
};

// prototype
void read_point(const char * prompt, Point & p);
void read_circle(const char * prompt, Circle & c);
double compute_distance(Point & p1, Point & p2);

int main()
{
  Circle c;
  Point p;

  read_point("Enter point (x,y): ", p);
  read_circle("Enter circle (x,y,radius): ", c);

  double dist = compute_distance(p, c.center);
  if (dist <= c.radius) {
    cout << "Point is inside circle." << endl;
  }
  else {
    cout << "Point is not inside circle." << endl;
  }

  return 0;
}

void read_point(const char * prompt, Point & p)
{
  cout << prompt;
  cin >> p.x;
  cin >> p.y;
}

void read_circle(const char * prompt, Circle & c)
{
  cout << prompt;
  cin >> c.center.x;
  cin >> c.center.y;
  cin >> c.radius;
}

double compute_distance(Point & p1, Point & p2)
{
  double diffx = p1.x - p2.x;
  double diffy = p1.y - p2.y;
  double dist = sqrt(diffx*diffx + diffy*diffy);

  return(dist);
}

bool circle_contains(Circle & c, Point & q)
{
  double dist = compute_distance(c.center, q);
  if (dist <= c.radius) 
    { return(true); }
  else
    { return(false); }
}

// Class Circle member function
Circle::contains(Point & q)
{
  double dist = compute_distance(center, q);
  if (dist <= radius) 
    { return(true); }
  else
    { return(false); }
}
{% endhighlight %}
