---
title: STL algorithms: Searching and Sorting
layout: default
---

## The Standard Template Library

The designers of C++ realized that there are many extremely useful
basic data structures and procedures.
beyond the basic types and operators supplied by C++.
Instead of creating a bloated language by adding these 
to the language definition,
they defined the C++ Standard Template Library (STL).
The C++ Standard Template Library is a set of template specifications
of some basic data structures and procedures.
These are independent from the language definition.

The C++ Standard Template Library is implemented as a set of C++ templates.
There are many implementations of the Standard Template Library
but they all must conform to the specifications set
by the ISO (International Organization for Standards) C++ Standards Committee.
Routines and classes in the C++ Standard Template Library are
in the namespace std.

The template class vector is part of the Standard Template Library.
The Standard Template Library also contains a template class list
for constructing linked lists.
In this section, we will talk about some algorithms provided 
by the Standard Template Library.

## Iterators

Algorithms in the Standard Template Library (STL) use iterators.
Iterators store references to individual vector or list elements.
An interator for vector<int> (vector of type int) is defined as:
{% highlight cpp %}
  vector<int>::iterator iter;
{% endhighlight %}
The iterator iter has type vector<int>::iterator.

Every STL vector vector has a member function begin() 
which returns an iterator for the first element of the vector or list
and a member function end() which represents the end of the vector or list.
If iterator iter references some element of a vector or list,
then iter++ is the next element of the vector or list.
If iter refers to the last element of the vector v or list l,
then iter++ equals v.end() or l.end().

The following example prints all elements of a vector using a vector iterator:
{% highlight cpp %}
#include <iostream>
#include <vector>
using namespace std;

int main()
{
  vector<int> v;

  for (int i = 3; i < 7; i++) 
    { v.push_back(2*i); }

  for (vector<int>::iterator iter = v.begin();
       iter != v.end(); iter++)
    { cout << " " << *iter; }
  cout << endl;

  return 0;
}
{% endhighlight %}

The end of a vector is the position after the last element of the vector:

![End of an array](/cse2122/images/array-3.png "End of array")

## Algorithm min_element

The function template min_element returns a reference to the minimum element
in a C array, a C++ vector or a C++ list.
The include file algorithm contains the definition of min_element.
We pass min_element a pointer to the beginning of the array 
and a pointer to the address after the last element in the array.
Function min_element returns a pointer to the minimum element in the array.
{% highlight cpp %}
#include <iostream>
#include <algorithm>  // STL algorithms are contained in file algorithm.
using namespace std;

int main()
{
  const int LENGTH = 5;
  int arr[LENGTH] = { 7, 4, 5, 2, 8 };

  // Note: min_element returns a pointer to the min element.
  int * min_arr_ptr = min_element(arr, arr+LENGTH);
  cout << "Min element of array arr: " << *min_arr_ptr << endl;

  return 0;
}
{% endhighlight %}

The address after the last element in array arr is (arr+LENGTH) = (arr+5).
This address is used to represent the end of the array.
(Note that array arr is a pointer so (arr+5) is addition 
in pointer arithmetic.)

![Array arr](/cse2122/images/array-4.png "Array arr")

Function min_element can also be applied to an STL vector or list.
We pass min_element v.begin() and v.end() where v.begin()
is an iterator marking the beginning of the list and
v.end() is an iterator marking the end of the list.
{% highlight cpp %}
#include <iostream>
#include <algorithm>  // STL algorithms are contained in file algorithm.
#include <vector>
using namespace std;

int main()
{
  const int LENGTH = 5;
  int arr[LENGTH] = { 7, 4, 5, 2, 8 };
  vector<int> v;

  for (int i = 0; i < LENGTH; i++) 
    { v.push_back(10*arr[i]); }

  // Note: min_element returns a vector iterator (reference into vector.)
  vector<int>::iterator min_v_iter = min_element(v.begin(), v.end());
  cout << "Min element of vector v: " << *min_v_iter << endl;

  return 0;
}
{% endhighlight %}


## Algorithm find

The function template find returns a reference to a given element
in a C array, a C++ vector or a C++ list.
The include file algorithm contains the definition of function find.
We pass find a pointer to the beginning of the array,
and a pointer to the address after the last element in the array,
and a value x.
Function find returns a pointer to the element in the array
which equals x.
If there is no such element,
then find returns a pointer to the address after the last element in the array.
{% highlight cpp %}
#include <iostream>
#include <algorithm>  // STL algorithms are contained in file algorithm.
using namespace std;

int main()
{
  const int LENGTH = 5;
  int arr[LENGTH] = { 7, 4, 5, 2, 8 };
  int x;

  cout << "Enter x: ";
  cin >> x;

  // Note: find returns a pointer to the element found.
  int * x_ptr = find(arr, arr+LENGTH, x);

  if (x_ptr == arr+LENGTH) {
    cout << "Element not found." << endl;
  }
  else {
    cout << "Element is a[" << x_ptr-arr << "]." << endl;
  }

  return 0;
}
{% endhighlight %}

The address after the last element in the array is (arr+LENGTH).
This address represents the end of the array.
If x_ptr point to a[i], then i equals (x_ptr-arr).

To apply find to an STL vector v, pass v.begin(), v.end()
and the value x to find.
If no element has value x, then find will return v.end().
{% highlight cpp %}
#include <iostream>
#include <algorithm>  // STL algorithms are contained in file algorithm.
#include <vector>
using namespace std;

int main()
{
  const int LENGTH = 5;
  int arr[LENGTH] = { 7, 4, 5, 2, 8 };
  vector<int> v;
  int x;

  for (int i = 0; i < LENGTH; i++) 
    { v.push_back(10*arr[i]); }

  cout << "Enter x: ";
  cin >> x;

  // Note: find returns a vector iterator.
  vector<int>::iterator x_iter = find(v.begin(), v.end(), x);

  if (x_iter == v.end()) {
    cout << "Element not found." << endl;
  }
  else {
    cout << "Element is v[" << x_iter-v.begin() << "]." << endl;
  }

  return 0;
}
{% endhighlight %}
