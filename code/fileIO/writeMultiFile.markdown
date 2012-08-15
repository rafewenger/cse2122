---
title: writeMultiFile.cpp (Write to a multiple files)
layout: default
---

{% highlight cpp %}

#include <cstdlib>     // function exit() is in cstdlib
#include <fstream>     // class ofstream() is in fstream
#include <iomanip>
#include <iostream>
using namespace std;

int main()
{
  ofstream fout1;   // declare output file stream 1
  ofstream fout2;   // declare output file stream 2

  fout1.open("book1.txt", ios::out);  // open file book1.txt
  fout2.open("book2.txt", ios::out);  // open file book2.txt

  // check if "book1.txt" is opened for output
  if (!fout1.is_open())
    {
      cerr << "Unable to open file book1.txt." << endl;
      exit(10);
    }

  // check if "book2.txt" is opened for output
  if (!fout2.is_open())
    {
      cerr << "Unable to open file book2.txt." << endl;
      exit(10);
    }

  cout << "Writing to files book1.txt and book2.txt." << endl;

  // write to book1.txt
  fout1 << "Assets: $" << 10000 << endl;
  fout1 << "Liabilities: $" << 15000 << endl;

  // write to book2.txt
  fout2 << "Assets: $" << 12000 << endl;
  fout2 << "Liabilities: $" << 9000 << endl;

  fout1.close();     // close file stream fout1
  fout2.close();     // close file stream fout2

  return 0;  
}
{% endhighlight %}
