---
layout: default
title: Recursion
---

<blockquote>
A child couldn't sleep, so her mother told her a story about a little frog,
<br/>
&nbsp; who couldn't sleep, so the frog's mother told her a story about a little bear,
<br/>
&nbsp; &nbsp; who couldn't sleep, so the bear's mother told her a story about a little weasel...
<br/>
&nbsp; &nbsp; &nbsp; who fell asleep.
<br/>
&nbsp; &nbsp; ...and the little bear fell asleep;
<br/>
&nbsp; ...and the little frog fell asleep;
<br/>
...and the child fell asleep.
</blockquote>

A recursive function (or self-recursive function) is a function that calls
itself. The concept is familiar to mathematicians, who define the factorial
function as the following: `n! = n*(n-1)!` with the special case that `0! = 1`.
Since the `!` is used in the right side of the equal sign (in the first
equation), the definition is recursive: it refers to itself. This may stink of
circularity, but so long as the recursive case (right side of the equals sign)
gets *closer* to the base case (`0! = 1`, which has no recursion), then it'll
all work out.

Here is our definition, in C++, of the factorial function just defined:

{% highlight cpp %}
int factorial(int n)
{
    if (n <= 0)
    {
        return 1;
    }
    else
    {
        return (n * factorial(n - 1));
    }
}
{% endhighlight %}

Notice that the code matches perfectly with the mathematical definition. This
is quite nice, since it's clean and simple (like math, usually).

Anything that can be done with recursion can be done with regular loops, and
vice versa. For instance, the function factorial can be
implemented as follows:
{% highlight cpp %}
int factorial (int n)
{
  int product = 1;
  for (int i = 1; i <= n; i++) 
  {
      product = product*i;
  }

  return(product);
}
{% endhighlight %}

As another example, the n'th *harmonic number* is (1+1/2+1/3+...+1/n).
The following function computes the n'th harmonic number
in a **for** loop:
{% highlight cpp %}
double harmonic (int n)
{
  double sum = 0.0;
  for (int i = 1; i <= n; i++) 
  {
      sum = (1.0/i) + sum;
  }

  return(sum);
}
{% endhighlight %}

The recursive version of this function is:
{% highlight cpp %}
double harmonic (int n)
{
  if (n <= 1) 
  { 
      return(1); 
  }
  else 
  {
      double sum = (1.0/n) + harmonic(n-1);
      return(sum);
  }
}
{% endhighlight %}

As another example of recursion,
consider drawing a triangle with n rows and n columns,
such as:
{% highlight cpp %}
*
**
***
****
*****
{% endhighlight %}
Draw this triangle by first drawing a triangle with n-1 rows and n-1 columns
and then adding the last row.
The following is a recursive function to draw
a triangle with n rows and n columns:
{% highlight cpp %}
void draw_tri(int n)
{
  if (n <= 0) { return; }

  draw_tri(n-1);

  for (int i = 0; i < n; i++) 
    { cout << "*"; }
  cout << endl;
}
{% endhighlight %}

Note that the function returns 0 if n is LESS THAN or equal to 0.
While it would be silly to pass a negative number to this program,
someone might accidentally do it.
What will happen if you pass a negative integer 
to the following recursive function:
{% highlight cpp %}
void bad_draw_tri(int n)
{
  if (n == 0)    // *** BETTER IS if (n <= 0)
    { return; }

  draw_tri(n-1);

  for (int i = 0; i < n; i++) 
    { cout << "*"; }
  cout << endl;
}
{% endhighlight %}

If you answered that the program would enter an infinite loop,
you were not quite correct.
The program would actually run out of memory on something 
called the recursion stack and then would abort with some message
like segmentation fault.
See below for the discussion about the recursion stack.

So far, we have not seen the benefit of recursion. But it's important to note
that we can have recursion, and not have loops, but be able to accomplish all
the same tasks. In fact, some programming languages ("functional languages"
such as Lisp) sometimes completely abandon loops (no `while` or `for` loops)
and instead make recursion really easy. With recursion, you can do a task
repeatedly, if needed. But also with recursion, lots of hard problems become
much simpler.

> Recursion is the root of computation since it trades description for
> time. -- *Alan J. Perlis, Epigrams on Programming*

## Fractals

Fractals are self-similar patterns 
which have similar structures at different scales.
An example of a fractal is the Sierpinski triangle:
{% highlight cpp %}
*               
**              
* *             
****            
*   *           
**  **          
* * * *         
********        
*       *       
**      **      
* *     * *     
****    ****    
*   *   *   *   
**  **  **  **  
* * * * * * * * 
****************
{% endhighlight %}

To construct this triangle,
start with the following 2x2 triangle:
{% highlight cpp %}
*
**
{% endhighlight %}
Make three copies of the 2x2 triangle, placing one on top
and two on the bottom to construct a 4x4 triangle:
{% highlight cpp %}
*               
**              
* *             
****            
{% endhighlight %}
Make three copies of the 4x4 triangle, placing one on top
and two on the bottom to construct an 8x8 triangle:
{% highlight cpp %}
*               
**              
* *             
****            
*   *           
**  **          
* * * *         
********        
{% endhighlight %}
Repeating this one more time, gives the original 16x16 triangle:
{% highlight cpp %}
*               
**              
* *             
****            
*   *           
**  **          
* * * *         
********        
*       *       
**      **      
* *     * *     
****    ****    
*   *   *   *   
**  **  **  **  
* * * * * * * * 
****************
{% endhighlight %}
Note that we could continue the process, add infinitum.

We construct the Sierpinski triangle by storing it in a matrix
and then drawing the matrix on the screen.
The following function constructs a *length x length* Sierpinski triangle
with lower left corner at (*row*, *col*) in the matrix.
It constructs the triangle by recursively constructing 
three *(length/2)x(length/2)* triangles,
one on top and two on the bottom.
{% highlight cpp %}
void setSierp(char m[MAX_LENGTH][MAX_LENGTH], int row, int col, int length)
{
  int half_length = length/2;

  if (length == 1) { m[row][col] = '*'; }
  if (length <= 1) { return; }

  setSierp(m, row, col, half_length);
  setSierp(m, row, col+half_length, half_length);
  setSierp(m, row+half_length, col, half_length);
}
{% endhighlight %}


## Sorting

Insertion sort is an algorithm for sorting an array of n objects
(numbers, names, etc.)
The algorithm is:

* If the array contains only one element, return.
(There is nothing to sort.)
* Otherwise, recursively sort the first (n-1) elements of the array.
* Insert the last (n'th) element into its correct position,
by swapping it with elements to its left.

The function for insertion sort is:
{% highlight cpp %}
// Sort first n elements of array a[].
void insertionSort(int a[], int n)
{
  int j;

  if (n <= 1) { return; }

  insertionSort(a, n-1);
  // Elements a[0], a[1], ..., a[n-1] are now in sorted order.

  // Insert a[n] in (a[0], a[1], ..., a[n-1]).
  j = n-1;
  while (j >= 1 && a[j-1] > a[j]) {
    swap(a[j-1], a[j]);  // swap exchanges the two elements
    j = j-1;
  }
}
{% endhighlight %}

## Chess, Tic Tac Toe, Checkers, etc.

How does a robot play a board game? It uses recursion. Consider the following general technique for Chess:

- See if there is a winning move (check-mate move); if so, return the value "YES!".
- If not, make a list of possible moves.
- For each possible move, pretend to make that move, and start over checking for moves (recursive call).
- If any of the recursive calls returned "YES!", also return "YES!"
- Otherwise, return "NO :("

The result of this recursive procedure is that the best move, the move that
could eventually lead to a win (a "YES!"), is chosen. If no path results in a
good move, well, the robot's already lost the game (no move eventually leads to
a win), so it should resign. A "search tree" was constructed and searched; for
any "tree" in computer science, a recursive procedure is used to analyze it
because each branch of a tree looks like a new tree. Self-similarity of
different components of a problem domain is a prime reason recursion is used.

## The stack

When a function calls another function, it cannot proceed until that other
function has completed. So a recursive function call waits on the recursive
call to finish. Thus, if a function calls itself *n* times, then there are
*n-1* functions waiting around for their recursive calls to finish. These
waiting functions are pushed onto the "stack" (like a stack of plates, each
function waits on top of the other). The deepest function call is at the top of
the stack, and when that function finishes, it gets "popped off" the top of the
stack, so that the previous function can proceed on its way.

If a function calls itself recursively too many times, the stack (which is just
computer memory, and thus finite) may "overflow," and the whole program
crashes. So we need to be careful to write recursive algorithms that don't get
too deep (generally it takes thousands or more recursive calls to overflow the
stack).

> One of our great features of our compiler is that it happens to turn
> out that it is very easy to have a good recursive function in it. I
> am very fond of them. They are hardly used by
> customers. Nevertheless, it is very important that they are in. The
> reason is that they give us possibilities that make the tool
> inspiring. -- E. W. Dijkstra, 1962

## Picture-in-a-picture

> **Tortoise:** That's the word I was looking for! "POPPING-TONIC" is what it's
> called, and if you remember to carry a bottle of it in your right hand as you
> swallow the pushing-potion, it too will be pushed into the picture; then,
> whenever you get a hankering to "pop" back out into real life, you need only
> take a swallow of popping-tonic, and presto! You're back in the real world,
> exactly where you were before you pushed yourself in.

> **Achilles:** That sounds very interesting. What would happen it you took
> some popping-tonic without having previously pushed yourself into a picture?

> **Tortoise:**  I don't precisely know, Achilles, but I would be rather wary
> of horsing around with these strange pushing and popping liquids. Once I had
> a friend, a Weasel, who did precisely what you suggested--and no one has
> heard from him since.

> **Achilles:** That's unfortunate. Can you also carry along the bottle of
> pushing-potion with you?

> **Tortoise:** Oh, certainly. Just hold it in your left hand, and it too will
> get pushed right along with you into the picture you're looking at.

> **Achilles:** What happens if you then find a picture inside the picture
> which you have already entered, and take another swig of pushing-potion?

> **Tortoise:** Just what you would expect: you wind up inside that
> picture-in-a-picture.

> **Achilles:** I suppose that you have to pop twice, then, in order to
> extricate yourself from the nested pictures, and re-emerge back in real life.

> **Tortoise:** That's right. You have to pop once for each push, since a push
> takes you down inside a picture, and a pop undoes that.

> **Achilles:** You know, this all sounds pretty fishy to me... Are you sure
> you're not just testing the limits of my gullibility?

> **Tortoise:** I swear! Look-here are two phials, right here in my pocket. If
> you're willing, we can try them. What do you say? --
> *G&ouml;del, Escher, Bach: An eternal golden braid*

