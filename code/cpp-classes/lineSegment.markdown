---
title: lineSegment.cpp (Example of class LineSegment.)
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
public:
  Point endpoint0;
  Point endpoint1;

  double length();  // return the line segment length
};

// prototype
void read_point(const char * prompt, Point & p);
void read_line_segment(const char * header, LineSegment & seg);
double compute_distance(const Point & p1, const Point & p2);

int main()
{
  LineSegment segA;
  LineSegment segB;

  read_line_segment("Enter first line segment: ", segA);
  read_line_segment("Enter second line segment: ", segB);

  double lengthA = segA.length();
  double lengthB = segB.length();

  if (lengthA > lengthB) 
    { cout << "First line segment is longer." << endl; }
  else if (lengthB > lengthA) 
    { cout << "Second line segment is longer." << endl; }
  else
    { cout << "Line segments have equal length." << endl; }

  return 0;
}

void read_point(const char * prompt, Point & p)
{
  cout << prompt;
  cin >> p.x;
  cin >> p.y;
}

void read_line_segment(const char * heading, LineSegment & seg)
{
  cout << heading << endl;
  read_point("Enter first endpoint (x,y): ", seg.endpoint0);
  read_point("Enter second endpoint (x,y): ", seg.endpoint1);
}

double compute_distance(const Point & p1, const Point & p2)
{
  double diffx = p1.x - p2.x;
  double diffy = p1.y - p2.y;
  double dist = sqrt(diffx*diffx + diffy*diffy);

  return(dist);
}

double LineSegment::length()
{
  double L = compute_distance(endpoint0, endpoint1);
  return(L);
}
{% endhighlight %}
