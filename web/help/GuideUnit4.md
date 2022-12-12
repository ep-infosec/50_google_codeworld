Lists
=====

The next step toward more powerful programming is the use of lists.  You have
seen them before, and used them to make lists of points for `polygon`, `curve`,
and similar functions.  In this part, we'll look at more powerful tools that are
available to capture repetition using lists.

A list is, well, just a list of things.  To write a list, use square brackets
around the whole list, and commas between the things in it.  For example:

* `[ 1, 2, 3, 4 ]` is a list of numbers.
* `[ circle(2), rectangle(3,5), blank ]` is a list of pictures.

You can use the function called `pictures` to combine a list of pictures
together into one picture.  It's just like the `&` operator, except that it
works on a whole list of pictures instead of just two.  For example:

~~~~~ . clickable
program = drawingOf(allThePictures)
allThePictures = pictures([
    solidRectangle(4, 0.4),
    solidCircle(1.2),
    circle(2)
    ])
~~~~~

List comprehensions
-------------------

Lists are a little bit interesting when you use them with `pictures`, but
they get a lot *more* interesting if you write list *comprehensions*.  A
list comprehension is a way to write a list of things by starting with a
simpler list, and copying something once for each thing in it.  So if you
have a list of numbers, you can turn it into a list of *circles*, each
with a different radius.  That looks like this:

~~~~~ . clickable
program = drawingOf(target)
target  = pictures([ circle(r) | r <- [1, 2, 3, 4, 5] ])
~~~~~

The list comprehension was `[ circle(r) | r <- [1, 2, 3, 4, 5] ]`, and it
has a few parts:

* The brackets around the whole thing tell you that it's a list. The vertical
  line in the middle separates the left part from the right part.
* On the left side of the vertical line is an expression, that describes the
  elements of the *resulting* list.  Here, each element of the resulting list
  is a circle, but with a different radius.  Notice that we use a *variable*
  called "r" that hasn't been defined yet!
* On the right side of the vertical line, you say what list to start with,
  and name its members.  That's where the variable *r* came from!  You can
  use this new variable on the left side.

So when you look at that list comprehension, it says to start with the list
`[ 1, 2, 3, 4, 5 ]`, and for every number in it, call that number `r`, and
put `circle(r)` in the resulting list.  The resulting list is `[ circle(1),
circle(2), circle(3), circle(4), circle(5) ]`.  But you didn't have to
type that over and over.

List ranges
-----------

With list comprehensions, it's now useful to have lists of numbers, because
you can turn them into lists of pictures.  But writing `[1, 2, 3, 4, 5]` was
still pretty long.  And imagine if you wanted a hundred things, instead of 5!

Luckily, there are easier ways to do it.  if you write `[1 .. 5]`, that's
shorthand for `[1, 2, 3, 4, 5]`.  In the same way, `[3 .. 10]` is shorthand
for `[ 3, 4, 5, 6, 7, 8, 9, 10 ]`.  Much easier!

Even better, if you want to count by 2s or 3s or 10s, you can!.  You just
have to give the first two numbers, then use `..` to continue from there.

Here's another example:

~~~~~ . clickable
program = drawingOf(star)
star    = pictures([ rotated(rectangle(10, 1/10), angle)
                     | angle <- [10, 20 .. 360] ])
~~~~~

So we start with a list of numbers counting by 10s from 10 to 360.  Then we
call each of those numbers the variable "angle", and get a list of pictures
that rotate something by each of those angles.  Finally, we combine all of
this pictures using the `pictures` function, and draw the result.

Advanced list comprehensions
----------------------------

There are a few other things you can do with list comprehensions.  You
might find them useful.

First, you can filter out certain members of the list you start with.
Suppose you want to draw those circles, like in the `target` example, but
you don't want to draw the middle one.  One way to say that is:

~~~~~ . clickable
program = drawingOf(target)
target  = pictures([ circle(r) | r <- [1 .. 5], r /= 3 ])
~~~~~

Notice that `/=` means "not equal to".  So this says to make a picture out
of circles built from each radius from 1 to 5, *except* for 3.

Second, you can base your list comprehension on several lists.
This will draw a grid of circles:

~~~~~ . clickable
program = drawingOf(grid)
grid    = pictures([ translated(circle(1/2), x, y)
                     | x <- [-9 .. 9], y <- [-9 .. 9] ])
~~~~~

Because there are two base lists separated by commas, this will draw a
circle for *every* *possible* *combination* of x and y from those lists.

Another way to use a list comprehension with two lists is to use two
vertical lines.  This is called a *parallel* list comprehension.  Instead
of including a result for all possible combinations, this will only match
the first element of each list, then the second from each list, and so on.
Here's an example, using a list of numbers, and a list of colors!

~~~~~ . clickable
program = drawingOf(circles)
circles = pictures([ colored(circle(r), c) | r <- sizes
                                           | c <- colors ])
sizes   = [ 1, 2, 3, 4, 5 ]
colors  = [ red, green, blue, yellow, purple ]
~~~~~

If you used a comma to separate the base lists, this would draw red,
green, blue, yellow, *and* purple circles at *each* of the sizes, and that
isn't what you want.  By using a parallel list comprehension, you make
sure only the smallest circle is red, then the next smallest is blue, and
so on.
