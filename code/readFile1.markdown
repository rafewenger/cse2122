---
title: readFile1.cpp (Read from a file)
layout: default
---

{% highlight cpp %}
#include <cstdlib>     // function exit() is in cstdlib
#include <fstream>     // class ofstream() is in fstream
#include <iostream>
using namespace std;

int main()
{
  ifstream fin;   // declare an input file stream
  int x;

  // open file intList1.txt for input
  fin.open("intList1.txt", ios::in);  

  // check if file is opened for input
  if (!fin.is_open())
    {
      cerr << "Unable to open file intList1.txt." << endl;
      exit(10);
    }

  // read text from file
  fin >> x;
  cout << "Read integer: " << x << endl;

  // close file stream fin
  fin.close();

  return 0;  
}
{% endhighlight %}
