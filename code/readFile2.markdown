---
title: readFile2.cpp (Read from a file)
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
  string file_name;
  int x;

  cout << "Enter file name: ";
  cin >> file_name;

  // open file file_name for input
  fin.open(file_name.c_str(), ios::in);  

  // check if file is opened for input
  if (!fin.is_open())
    {
      cerr << "Unable to open file " << file_name << endl;
      exit(10);
    }

  // read text from file
  for (int i = 1; i <= 5; i++)
    {
      fin >> x;
      cout << "Read integer: " << x << endl;
    }

  // close file stream fin
  fin.close();

  return 0;  
}

{% endhighlight %}
