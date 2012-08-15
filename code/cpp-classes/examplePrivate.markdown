---
title: examplePrivate.cpp (Example of information hiding.)
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

// prototype
void read_point(const char * prompt, Point & p);
double compute_distance(const Point & p1, const Point & p2);

int main()
{
  LineSegment segA;
  Point q;

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

  return 0;
}

void read_point(const char * prompt, Point & p)
{
  cout << prompt;
  cin >> p.x;
  cin >> p.y;
}

double compute_distance(const Point & p1, const Point & p2)
{
  double diffx = p1.x - p2.x;
  double diffy = p1.y - p2.y;
  double dist = sqrt(diffx*diffx + diffy*diffy);

  return(dist);
}

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

double LineSegment::length()
{
  double L = compute_distance(endpoint0, endpoint1);
  return(L);
}
{% endhighlight %}
