---
layout: default
title: File I/O
---

A C++ *input stream* is a stream of input that is being fed
into the program for use.
A C++ *output stream* is a stream of output that is being generated 
by the program.
For example, *cin* is an input stream which reads data from the terminal.
*cout* is an output stream which writes data to the terminal.

Any significant program uses files to store data.
C++ reads and writes to files using file streams.
To use a file stream operation, we include the file *fstream*:
{% highlight cpp %}
#include <fstream>
{% endhighlight %}

To write a file:

1. Create an output stream object;
2. Establish connection to a file;
3. Check validity of the connection;
4. Start writing;
5. Close the file stream.

Example:
{% highlight cpp %}
#include <cstdlib>           // function exit() is in cstdlib
#include <fstream>         // class ofstream() is in fstream
#include <iostream>

using namespace std;

int main()
{
  ofstream fout;                           // declare an output file stream

  fout.open("hellofile.txt", ios::out);    // open file file_name for output

  if (!fout.is_open())                     // check if file is opened for output
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

