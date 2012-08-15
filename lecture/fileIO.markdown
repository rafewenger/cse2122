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
The name of the file stream is *fout*.
We used this name because it is similar to **cout**.
However, we could have used any unused variable name for the file handler 
(just like other variables).

Note the '**o**' in the file stream type **ofstream**.
This indicates an output file stream.
The flag **ios::out** in the call to *fout.open*.
opens the file for output.

Opening a file for output can fail because the file directory does not exist
or because the program does not have permission to write to the file.
To check if the file is open:
{% highlight cpp %}
  if (!fout.is_open())                     // check if file is opened for output
    {
      cerr << "Unable to open file hellofile.txt." << endl;
      exit(10);
    }
{% endhighlight %}
The error message is written to **cerr** not **cout**.
File stream **cerr** is similar to **cout** and is intended for error messages.
The command **exit** exits the program.
The number *10* is a return code for use by any other program
which called this program.
Returning any number other than *0* indicates an error,
in this case an error opening the file.
Complicated programs can have many different reasons for exiting.
Returning different return codes based on the different exit points
helps other programs determine how to handle the error.

We write to the file in the same way that we write to the terminal
using **cout**.
Note the similarities between **cout** and *fout*.  Everything we can do with
**cout** also applies to *fout*.
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

Program [writeFile1.cpp](../code/fileIO/writeFile1) is a complete example 
of a program which writes to a file.

Program [writeMultiFile.cpp](../code/fileIO/writeMultiFile) is an example
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
To convert the C++ string *file_name* into a C style string, 
use the member function *file_name.c_str()*:
{% highlight cpp %}
fout.open(file_name.c_str(), ios::out);
{% endhighlight %}

Program [writeFile2.cpp](../code/fileIO/writeFile2) is an example
of a program which reads a file name.


# Reading from a File

To read from a file, we need a-priori knowledge of the file format.
Is the file a list of integers, of floating point numbers
of strings, or of some combination of these?
Moreover, we need to know what each value represents,
i.e., is it age or weight or temperature?

To read from a file:

1. Create an input stream object;
2. Establish a connection to the file;
3. Check the validity of the connection;
4. Read from the file;
5. Close the file.

We usually also check that the read succeeded.

To declare and open the file stream:
{% highlight cpp %}
  ifstream fin;   // declare an input file stream

  fin.open("intList1.txt" , ios::in);   // open file intList.txt for input
{% endhighlight %}
The name of the file stream is *fin*.
We used this name because it is similar to **cin**
but could have used any unused variable name

Note the '**i**' in the file stream type **ifstream**.
This indicates an input file stream.
The flag **ios::in** in the call to *fout.open*.
opens the file for input.

Opening a file for input can fail because the file does not exist
or is not in the given directory or because the program does not
have permission to read from the file.
To check if the file is open:
{% highlight cpp %}
if (!fin.is_open())                // check if file is open for input
    {
      cerr << "Unable to open file intList1.txt." << endl;
      exit(10);
    }
{% endhighlight %}

We read from the file in the same way that we read from the terminal
using **cin**.
Everything we can do with **cin** also applies to *fin*.
{% highlight cpp %}
    fin >> x;     // read text from file
{% endhighlight %}

To close the file stream:
{% highlight cpp %}
  fin.close();       // close file stream fin
{% endhighlight %}
It is good programming practice to close
a file stream when you are done using the file.

Program [readFile1.cpp](../code/fileIO/readFile1) is a complete example 
of a program which reads from a file.

As with writing to a file, the program often prompts
for a file name,
and then opens the file with the given file name.
The file name must be converted to a C style string
for the function **open**:
{% highlight cpp %}
fin.open(file_name.c_str(), ios::in);       // open file file_name for input
{% endhighlight %}

Program [readFile2.cpp](../code/fileIO/readFile2) is an example
of a program which reads a file name.

Often, one wants to read all the data in the file.
To do so, one repeats repeating in a while loop until the read fails:
{% highlight cpp %}
    fin >> x;
    while (!fin.fail())
    {
        cout << "Read integer: " << x << endl;
        fin >> x;
    }
{% endhighlight %}
Note the first read before entering the **while** loop.

The *fin.fail()* function is true when the read fails.
Once an input operation fails, all subsequent input operations will fail.
The statement *while (fin)* is equivalent to *while(!fin.fail())*.

Program [readFile3.cpp](../code/fileIO/readFile3) is an example
of a program which reads input until the read operation fails.
Program [readFile4.cpp](../code/fileIO/readFile4) is another example
of a program which reads input until the read operation fails
using *while (fin)* instead of *while(!fin.fail())*.

A read fails because:

- The end of file is reached.
- A read error such as trying to read a character string as an integer.

When a read fails because of a read error, 
one usually wants to print an error message and sometimes halt.
The function *fin.eof()* is true when the end of file is reached.
To check if the program failed for some reason other than end of file,
use *if (!fin.eof())*:
{% highlight cpp %}
  if (!fin.eof())		// check for error  
    {
      cerr << "Error reading file " << file_name << endl;
      exit(20);
    }
{% endhighlight %}
Note the use of **cerr** instead of **cout** for printing the error message.
The command **exit(20)** exits the program on an error and returns
the code integer 20.

Program [readFile5.cpp](../code/fileIO/readFile5) is an example
of a program which checks for read errors.


# Common Errors

* Writing to a file which was opened for reading;
* Reading from a file which was opened for writing;
* Forgetting to open a file;
* Not checking for read failure.

