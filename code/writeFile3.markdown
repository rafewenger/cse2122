---
title: writeFile3.cpp (Write to a file)
layout: default
---

{% highlight cpp %}

#include <cstdlib>     // function exit() is in cstdlib
#include <fstream>     // class ofstream() is in fstream
#include <iostream>
#include <string>      // type string is in file string
using namespace std;

int main()
{
  ofstream fout;   // declare an output file stream
  string file_name;

  cout << "Enter file name: ";
  cin >> file_name;

  // open file file_name for output
  // file_name.c_str() returns a C style string
  fout.open(file_name.c_str(), ios::out);

  // check if file is opened for output
  if (!fout.is_open())
    {
      cerr << "Unable to open file " << file_name << endl;
      exit(10);
    }

  // write text to the file
  fout << "Hello World!" << endl;
  fout << "Goodbye World!" << endl;

  // close file stream fout
  fout.close();

  return 0;  
}
{% endhighlight %}
