---
layout: default
title: File I/O
---

A C++ *input stream* is a stream of input that is being fed
into the program for use.
A C++ *output stream* is a stream of output that is being generated 
by the program.
For example, **cin** is an input stream which reads data from the terminal.
**cout** is an output stream which writes data to the terminal.

Any significant program uses files to store data.
C++ reads and writes to files using file streams.
To use a file stream operation, we include the file **fstream**:
{% highlight cpp %}
#include <fstream>
{% endhighlight %}


# Writing to a File

To write a file:

1. Create an output stream object;
2. Establish connection to a file;
3. Check validity of the connection;
4. Start writing;
5. Close the file stream.

To declare and open the file stream:
{% highlight cpp %}
  ofstream fout;                           // declare an output file stream

  fout.open("hellofile.txt", ios::out);    // open file file_name for output
{% endhighlight %}
The name of the file stream is **fout**.
We used this name because it is similar to **cout**.
However, we could have used any unused variable name for the file handler 
(just like other variables).

Note the '**o**' in the file stream type **ofstream**.
This indicates an output file stream.
The flag **ios::out** in the call to **fout.open**.
opens the file for output.

Opening a file for output can fail because the file does not exist
or is not in the given directory or because the program does not
have permission to write to the file.
To check if the file is open:
{% highlight cpp %}
  if (!fout.is_open())                     // check if file is opened for output
    {
      cerr << "Unable to open file hellofile.txt." << endl;
      exit(10);
    }
{% endhighlight %}

We write to the file in the same way that we write to the terminal
using **cout**.
Note the similarities between **cout** and **fout**.  Everything we can do with
**cout** also applies to **fout**.
{% highlight cpp %}
  // write text to the file
  fout << "Hello World!" << endl;
  fout << "Goodbye World!" << endl;
{% endhighlight %}

To close the file stream:
{% highlight cpp %}
  fout.close();       // close file stream fout
{% endhighlight %}
When the program ends,
the operating system closes all files streams.
However, it is very, very good programming practice to close
a file stream when you are done using the file.
Programs which use many multiple files but do not close those files
will eventually run out of resources.
If I catch you forgetting to close a file stream 
on your programming assignments,
I will deduct points.

Program [writeFile1.cpp](../code/writeFile1) is a complete example 
of a program which writes to a file.

Program [writeMultiFile.cpp](../code/writeMultiFile) is an example
of a program which writes to more than one file.

We usually don't want to have the file name hard-wired into the program.
Instead the program prompts for a file name,
reads the filename into a C++ string 
and then opens the file with the given file name.
For example:
{% highlight cpp %}
#include <string>

string file_name;

cout << "Enter file name: ";
cin >> file_name;
{% endhighlight %}

Unfortunately, the **open()** function only takes C style strings.
To convert the C++ string **file_name** into a C style string, 
use the member function **file_name.c_str()**:
{% highlight cpp %}
fout.open(file_name.c_str(), ios::out);
{% endhighlight %}

Program [writeFile3.cpp](../../code/writeFile3) is an example
of a program which reads a file name.


# Reading from a File

