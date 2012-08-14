---
title: Example of a C++ Class
layout: default
---

The class *Complex* is a class of complex numbers.
{% highlight cpp %}
class Complex
{
	public:
		double r;
		double i;

	Complex(double, double);
	Complex(double );
	Complex();

	Complex add(Complex&);

	Complex operator+ (Complex&);
	Complex operator- (Complex&);
	Complex operator* (Complex&);
	Complex operator/ (Complex&);
	Complex operator/ (double);

	Complex conj();
	double  norm();
	void    print();

};
{% endhighlight %}
The class has two attributes *r* and *i*,
the real and imaginary parts of the complex number.
It has three constructors and member functions 
*add*, *conj*, *norm* and *print*.
It also overloads the operators \+,\-,\* and \/.

The definition of the class is split into two files.
Another file calls the program.

**[complex.h](/cse2122/code/complex_h.html) : Contains the declaration of the Complex class**

**[complex.cpp](/cse2122/code/complex_cpp.html) : Contains definitions of the functions belonging to class Complex**

**[main.cpp](/cse2122/code/complex_main_cpp.html) : Contains main() function that uses the Complex class**

**To compile the program complex_main_cpp, all the files need to be put 
together under the same project in Visual Studio/CodeBlocks/Xcode/Eclipse.**

