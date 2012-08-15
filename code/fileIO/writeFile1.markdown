---
title: writeFile1.cpp (Write to a file)
layout: default
---

{% highlight cpp %}

#include <cstdlib>     // function exit() is in cstdlib
#include <fstream>     // class ofstream() is in fstream
#include <iostream>

using namespace std;

int main()
{
  ofstream fout;   // declare an output file stream

  // open file file_name for output
  fout.open("hellofile.txt", ios::out);  

  // check if file is opened for output
  if (!fout.is_open())
    {
      cerr << "Unable to open file hellofile.txt." << endl;
      exit(10);
    }

  cout << "Writing to file hellofile.txt." << endl;

  // write text to the file
  fout << "Hello World!" << endl;
  fout << "Goodbye World!" << endl;

  // close file stream fout
  fout.close();

  return 0;  
}
{% endhighlight %}
