# Module 1: Cliff Jumper Notes

*One continuous lesson stitched from the eleven Module 1 cliff notes of
CMPINF-2105: what a linear equation is and why its picture is a straight line,
how a third variable turns the line into a plane, vectors as the tool that
sorts an equation's information by type, the arithmetic of vectors and the
norm that measures their length, the dot product that folds a whole equation
into three symbols and the angle formula that says what it measures, NumPy as
the way all of this gets computed, matrices as grids of data and as functions
that rotate, scale, and shear, eigenvectors as the directions a matrix leaves
alone, the by-hand method for finding them through the determinant and the
quadratic formula, the one-line NumPy version, and the payoff: compressing a
face by keeping only the eigenvectors that matter. Examples and code not
attributed to the professor are the notes' own.*

## 1. Linear algebra is one pattern, repeated

The professor opens with a definition he immediately calls circular: linear
algebra is the study of linear systems and linear equations. His fix is to
skip the definition and build the thing. The module's plan is to walk through
example scenarios, see how they capture real world problems, and then define
the tools (vectors, matrices, and the rest) that make up linear algebra.

The scenario is a fruit stand. You are sent with $30 that must be spent
entirely, apples cost $2 each, oranges cost $3 each, and fractional fruit is
allowed, so one and a half apples is a legal purchase. He calls it contrived.
It is contrived on purpose. The two rules are what make the math clean (added
note): "spend all of it" turns an inequality into an equation, and "fractions
allowed" puts the variables on a continuous number line instead of on the
integers.

Written out in full the scenario is $30 = ($2 x apples) + ($3 x oranges),
where the italic words stand for the number purchased. A **variable** is a
symbol standing for some number or some other mathematical object, written in
italics as the hint that it is a variable and not a label. Sometimes a full
word, sometimes a single letter like x or y. He then strips the equation
down: the multiplication sign becomes a dot, the dollar signs go, the
parentheses go, and what is left is 30 = 2.apples + 3.oranges. Fewer
characters, same equation.

That stripped form is the whole subject in miniature. A **linear equation** is
any equation of the form

    y = a_1.x_1 + a_2.x_2 + a_3.x_3 + ... + a_N.x_N

One value on the left. On the right, as many terms as you like, every one of
them a number times a variable, all added together. For the fruit stand, y is
the $30 budget, x_1 and x_2 are the apple and orange counts, and a_1 and a_2
are the $2 and $3 prices. More grocery items means more terms, one per item,
and nothing else changes. The subscript i is just a slot number (added):
x_3 is the third variable and a_3 is the number multiplying it.

What "linear" rules out (added): no variable is squared, no two variables are
multiplied together, no variable sits inside a square root or a sine. Each
variable appears once, multiplied by a plain number. That single restriction
is why every picture in the first two lectures comes out straight or flat.

Because the right side repeats one pattern, there is a shorthand. The
**summation** sign, which he calls "this big Epsilon thing", says add up
a_i.x_i for every i from 1 to N:

    y = sum from i=1 to N of a_i.x_i

Read it by unrolling: i = 1 gives a_1.x_1, i = 2 gives a_2.x_2, and so on up
to a_N.x_N, then add them all. The big long equation is now one line. It
comes back whenever a big equation needs a small space.

A worked version with different numbers. A coffee cart sells bagels at $3 and
coffees at $4, and you have exactly $24 to spend. The linear equation is
24 = 3.bagels + 4.coffees. In summation form, with x_1 as bagels, x_2 as
coffees, a_1 = 3, a_2 = 4, and N = 2, it is the sum from i = 1 to 2 of
a_i.x_i. For two items the sigma saves nothing. For two hundred it saves a
page. In NumPy, the Python package section 9 introduces, the summation is
one call (added):

    import numpy as np
    a,x = np.array([3,4]),np.array([4,3]) ## prices, counts
    print(np.sum(a*x)) ## 24, the unrolled summation

Hold onto that line. Section 7 gives it a name.

## 2. Infinitely many solutions, and why the picture is a line

How many apple and orange combinations spend the $30 exactly? Infinitely many.
You can see it by rearranging the equation with a little algebra, under the
one rule he states plainly: whatever you do to one side of the equals sign,
you must do to the other, or the two sides stop being equal.

His two moves on the fruit stand: subtract 2.apples from both sides, then
divide both sides by 3. The 30 becomes 10, the 2.apples becomes 2/3.apples,
and 3.oranges becomes oranges because 3/3 is 1 and the 1 is not written. The
result is oranges = 10 - 2/3.apples. Tell him the number of apples and the
equation hands back the number of oranges that spends the budget exactly. His
check: 3 apples means 8 oranges, $6 plus $24, $30.

The coffee cart, solved the same way. Start from 24 = 3.bagels + 4.coffees.
Subtract 3.bagels from both sides: 24 - 3.bagels = 4.coffees. Divide both
sides by 4: coffees = 6 - 3/4.bagels. Check with 4 bagels: 6 minus 3 is 3
coffees, and $12 of bagels plus $12 of coffee is $24. Check the ends: 0
bagels gives 6 coffees, 8 bagels gives 0 coffees. Every bagel count between,
fractions included, works too. A continuous range of inputs gives a
continuous range of outputs. Infinite.

The rearranged equation has a shape with a name. It is y = b + m.x, which he
calls a really common way of representing straight lines. The **slope** m
tells you how y changes if you change x a little bit. The **intercept** b
tells you what y is when x is zero: set x to zero, the m.x term vanishes, and
b is what is left over. For the fruit stand m = -2/3 and b = 10. Each
additional apple means 2/3 of an orange less, and zero apples means 10
oranges. For the coffee cart m = -3/4 and b = 6. Each extra bagel costs three
quarters of a coffee, and zero bagels means six coffees.

Two readings worth keeping (added). The slope is negative because the budget
is fixed: more of one thing forces less of the other. And the slope is the
price ratio, minus the price of x over the price of y. A bagel eats $3 of
budget and each $4 of budget is one coffee, so one bagel displaces 3/4 of a
coffee. The slope is the exchange rate between the two items.

What does linear mean? Plot the equation with apples on the horizontal axis
and oranges on the vertical axis and you get a straight line. His slider
widget moves a point along it, and for each new apple the allowed oranges
drop by about 2/3, the same drop everywhere. See the plot and widget in the
Lecture 1 notebook section "Example 1: The Fruitstand". Constant slope means
straight line means linear (added). That is the test.

## 3. A third variable makes a plane

Lecture 2 adds one ingredient. The grocery store sells apples at $2, oranges
at $3, and now bread at $5 a loaf. Same $30, spent entirely, fractions
allowed. His description is "just a slight complication": three variables
instead of two. The equation is 30 = 2.apples + 3.oranges + 5.bread, still a
linear equation because it fits the general form, one a times an x per item.

He wants oranges as a function of the other two again. His road: subtract 30
and 3.oranges from both sides, then divide both sides by minus three, which
isolates oranges on one side and every other variable on the other. He tells
you to check his work yourself. What comes out (added, the transcript
describes the steps and the notebook prints the line) is oranges =
10 - 2/3.apples - 5/3.bread. Each other item now shows up as its own exchange
rate against oranges. Two slopes instead of one.

The coffee cart gets muffins at $2. The equation is 24 = 3.bagels +
4.coffees + 2.muffins. Solving for coffees by the professor's road: subtract
24 and 4.coffees from both sides to get -4.coffees = 3.bagels + 2.muffins -
24, then divide both sides by -4 to get coffees = 6 - 3/4.bagels -
1/2.muffins. Check with 2 bagels and 4 muffins: 6 minus 1.5 minus 2 is 2.5
coffees, and $6 plus $8 plus $10 is $24. Dividing by a negative flips every
sign on the right, which is why the minus three road and the subtract-then-
divide-by-positive-three road land in the same place (added).

How many solutions? Infinitely many again, for the same two reasons. But now
you pick two numbers freely, apples and bread, and the equation hands back
the third. Two free choices instead of one (added). The solution set went
from a line's worth of points to a surface's worth.

That surface is the picture. Two variables plotted against each other gave a
line. Three variables need a third axis, the number of bread loaves, and
plotting the equation on three axes gives a **plane**, a flat surface (added
definition: flat means no curvature, and any straight line between two
points on it stays on it). See the rotating 3D surface in the Lecture 2
notebook section "Example 2: A Grocery Store". He rotates it so you can get a
sense of the flatness.

His key move is to view the plane from the right angle, his slide hint being
"40 and minus three", at which point it almost looks like a line. What you
are seeing is the linear relationship between bread and oranges with the
number of apples fixed. Fix one variable and a three variable equation
collapses to a two variable one, and two variable linear equations are lines
(added). For the coffee cart, fix bagels at 2 and coffees = 4.5 - 1/2.muffins,
a line with intercept 4.5 and slope -1/2. Fix bagels at 4 and the intercept
drops to 3 but the slope stays -1/2. Changing the fixed variable slides the
line up or down and never tilts it. That is the flatness of the plane
showing up in the algebra.

## 4. Vectors: the equation's information, sorted by type

Lecture 3 is where he says the course starts talking about linear algebra in
its most typical fashion. The grocery equation carries three kinds of
information: the total budget on the left, the number of each item (the
variables), and the price of each item (the numbers multiplying them). His
alternative to one long equation is to group the information by type,
separating the counts from the costs. Vectors do that, and in his phrase
they represent the information as points in a high dimensional space.

The motivation he gives is flexibility. Prices have been treated as fixed,
but prices change, and you want a representation of the cost information you
can update later. In practice (added) a single equation hard-codes the prices
into the formula, while a separate cost vector is a parameter you can swap
without rewriting the model. Every later operation in the module is built on
that separation of the data from the rule.

A **vector** is an ordered list of numbers. He writes it as a column with a
small arrow over the letter, c with an arrow, to say this c is not a number.
For the grocery store the **cost vector** is (2, 3, 5): apples, oranges,
bread, in that order. A **component** is one entry, identified by its
position (added definition), and the order is part of the meaning. For the
fruit stand, with bread ignored, the cost vector is just (2, 3), and his
geometric reading is that it marks a specific point in two dimensional
space, not the usual x-y plane but the apple-orange cost plane. It is common
to draw a vector not as a point but as an arrow to that point.

The arrow starts at the **origin**, the point where every coordinate is zero
(added definition), (0, 0) in two dimensions and (0, 0, 0) in three. He
returns to the apples-versus-oranges line from Lecture 1 and draws the
current (apples, oranges) pair as an arrow emanating from zero zero to the
point on the line, and the arrow swings as the slider moves. Then the same in
three dimensions with sliders for apples, oranges, and bread, the arrow
moving along whichever axis you change. See both widgets in the Lecture 3
notebook section "What are Vectors?". One thing to hold (added): the vector
is the list of numbers, the arrow is how you draw it. Any time a picture gets
confusing, go back to the list.

The coffee cart's cost vector is c = (3, 4, 2) for bagels, coffees, muffins.
Drop the muffins and the two item version (3, 4) is a point three units
along the bagel-cost axis and four up the coffee-cost axis.

## 5. Vector arithmetic: add, subtract, scale

Three operations, all done position by position.

**Adding a number to a vector.** The grocery raises every price by $1. Write
a second vector c2 equal to the cost vector plus one, and the rule is to add
the number to every component. His fruit stand case goes from (2, 3) to
(3, 4). The coffee cart's (3, 4, 2) after a $1 hike is (4, 5, 3). His demo
slider adds more and more, and the new vector moves further out along both
axes. See the add_to_vector widget in the same notebook section.

**Adding two vectors.** Vector addition is **componentwise**: in his words,
the first component of a plus the first component of b, then the second with
the second, and so on. His generic example is a = (2, 3) and b = (1, 2),
giving a + b = (3, 5). The two vectors must be the same length, or there is
nothing for a component to pair with (added). The picture, in his words: take
a copy of b, same angle and same length, and attach its tail to the endpoint
of a. Where the copy ends is a + b. His demo draws a ghost copy of b, and
changing b's length or angle changes the ghost, but sticking it on the end of
a always lands on a + b. See the addVectors widget.

For a = (1, 4) and b = (3, 2): first components 1 + 3 = 4, second components
4 + 2 = 6, so a + b = (4, 6). Start at the origin, walk to (1, 4), then walk 3
right and 2 up. Vector addition is "and then" (added): position plus
displacement, this week's sales plus next week's.

**Subtracting two vectors.** Exactly the same, componentwise. His a - b is
(1, 1). The picture flips: attach the ghost copy's head to the end of a and
walk backwards along it. See the subtractVectors widget. For a = (1, 4) and
b = (3, 2), a - b = (-2, 2) and b - a = (2, -2). Order matters, reversing it
flips every sign, and negative components are fine, (-2, 2) is an arrow
pointing up and to the left. Subtraction is the operation for "what changed"
(added): the difference between two positions, the error between a
prediction and an actual.

**Scaling a vector by a scalar.** A new vector g holds the other kind of
information, the number of each item purchased. His example is g =
(4, 2, 3), meaning 4 apples, 2 oranges, 3 loaves, and tripling the purchase
gives 3.g = (12, 6, 9), every component multiplied by 3. A **scalar** is a
plain number that multiplies a vector, and he prefers to say the vector was
scaled by a scalar rather than multiplied, because multiplication has
contextually dependent meaning once vectors are involved. Lecture 5 has two
different vector multiplications waiting, so scalar multiplication gets its
own name now.

The picture: scaling g by 3 is like sticking two extra copies of g end to end
on g. Same angle, three times the length. His slider walks three cases. A
positive scalar above 1 lengthens the vector along the same angle, a scalar
below 1 shrinks it, and a negative scalar keeps the same line but points the
vector the opposite way. The angle to the axes never changes. See the
two-panel plot and the scale_vector widget.

For a coffee cart order g = (2, 1, 4), doubling gives (4, 2, 8), halving gives
(1, 0.5, 2), and negating gives (-2, -1, -4). Half a coffee is legal here, the
professor allowed fractional items in Lecture 1. All of it is one line each
in NumPy (added):

    import numpy as np
    c,g = np.array([3,4,2]),np.array([2,1,4]) ## costs, order
    print(c+1) ## [4 5 3], price hike
    print(2*g, -1*g) ## [4 2 8] [-2 -1 -4], scaled
    a,b = np.array([1,4]),np.array([3,2]) ## two generic vectors
    print(a+b, a-b) ## [4 6] [-2  2]

## 6. Direction and length: the physicist's vector and the norm

Lecture 4 offers a second way to see the same object. Physicists love vectors
and linear algebra because they provide a concise way to encode high
dimensional information in one abstraction. His illustration is a spaceship
moving through three dimensions and through time, with its current location
and current velocity encoded as a single vector instead of tracked
separately. In physics, a **vector** is anything that has a direction and a
length.

The list view tells you how to store a vector and how to add or scale it.
The direction-and-length view tells you what it means (added). The bridge
between them is the norm.

His running example is a baseball thrown from the origin. A simulation steps
through time, a dashed line traces the trajectory, and a gray arrow shows the
position vector p at each step. See the draw_trajectory widget in the
Lecture 4 notebook section "A Physicist Intro to Vectors". The ball is thrown
in two dimensions, so p has two components: p_1 is the location in the x
direction, the horizontal, and p_2 is the location in the y direction, the
vertical, measured as distance from the origin.

The question the norm answers: how far is the ball from where it was thrown?
The **norm** is a vector's length, or magnitude, written with two sets of
vertical bars around the vector, ||p||. For a two dimensional vector it is
the square root of the sum of the squares of the components. His emphasis:
the norm of a vector is always a number, not a vector.

A delivery drone lifts off from its pad at (0, 0) and after a few seconds
sits 6 meters east and 8 meters up, so p = (6, 8). Square the components, 36
and 64. Add, 100. Square root, 10. The drone is 10 meters from its pad in a
straight line. Squaring gets rid of signs and makes the contributions add
cleanly, and the square root undoes the squaring so the answer is back in
the original units, meters in and meters out (added).

Higher dimensions work the same way. If the ball is thrown in three
dimensional space, add one squared term per component under the root. The
drone at (2, 3, 6), meaning 2 east, 3 north, 6 up, has squares 4, 9, 36, sum
49, norm 7. A four component vector (1, 2, 2, 4) has squares 1, 4, 4, 16, sum
25, norm 5. The picture is gone at four dimensions, the arithmetic is
identical, and that is the point of having a formula (added).

    import numpy as np
    p = np.array([2,3,6]) ## drone position, meters
    print(np.sqrt(np.sum(p**2))) ## 7.0, square add root
    print(np.linalg.norm(p)) ## 7.0, same thing

Where did the formula come from? It is derived from the Pythagorean theorem,
which some will remember from a trigonometry section on triangles containing
a right angle. The **hypotenuse** is the longest side of a right triangle,
the one directly opposite the right angle. For a right triangle with base a
and height b, the hypotenuse c satisfies a squared plus b squared equals c
squared, and taking the square root of both sides gives c equals the square
root of a squared plus b squared. That is the norm equation for a two
component vector with the sides renamed. A vector (p_1, p_2) drawn from the
origin is the hypotenuse of a right triangle whose base runs p_1 along the
horizontal axis and whose height runs p_2 straight up (added). The drone at
(6, 8) is a 6-8-10 triangle, a 3-4-5 triangle doubled.

To drive the point home he reruns the baseball simulation with the right
triangle drawn in orange under the vector, base a, height b, hypotenuse c.
See the second draw_trajectory widget in the same section, the one that
draws the triangle once t is above zero.

## 7. The dot product: the whole equation in three symbols

Lecture 5 finishes the compression that Lecture 1 started. Two vectors, both
of length 3: g holds the amounts (apples, oranges, bread) and c holds the
costs, (2, 3, 5). Look at the budget equation and what you see is each
vector's components multiplied pairwise, then summed. First component of c
times first of g, plus second times second, plus third times third.

This happens so often it gets a name. The **vector dot product** is the sum
of the componentwise products of two vectors, written c . g with a small dot,
or in summation form the sum over i of c_i times g_i. The arrows on top say c
and g are vectors. The subscript i says you are looking at the ith component
of each, which is a number. The payoff is that the whole budget equation
becomes 30 = c . g. The same exact information, shortened again. Two vectors
go in, one number comes out (added), and that is worth saying out loud
because the next operation returns a vector.

The coffee cart's cost vector is c = (3, 4, 2) and an order is g = (2, 1, 4).
Pairwise products: 3 times 2 is 6, 4 times 1 is 4, 2 times 4 is 8. Sum: 18.
The order costs $18, the receipt total in one operation. The NumPy line from
section 1, np.sum(a*x), was a dot product all along (added). The dot product
was not invented and then found useful. It is the linear equation with its
two kinds of information pulled apart.

He flags a confusion. The dot symbol is doing two jobs: between two vectors
it means the vector dot product, between two components (c_i . g_i) it means
the usual multiplication of two numbers. His words: "this double usage of
notation, it's confusing, it's a little unfortunate." Look at what sits on
either side of the dot (added). Arrows on top, vector dot product, result a
number. Subscripts, ordinary multiplication.

That raises the question of a more natural vector multiplication, one that
keeps the pairwise products as a vector. That exists. **Elementwise
multiplication** multiplies two vectors component by component and keeps the
results as a new vector of the same length, written with an asterisk, c * g.
For the coffee cart, c * g = (6, 4, 8), still a vector, each entry one line
item's subtotal. Elementwise multiplication is the itemized receipt and the
dot product is the total at the bottom (added). His demo shows two vectors
and their elementwise product in black, and he notes the black vector can
grow pretty quickly, "as you would expect, multiplicatively." See the
visualizeElementProduct widget in the Lecture 5 notebook section "Vector Dot
Products".

The relationship between the two: the dot product is just the sum of the
elementwise products. He writes it with vertical bars around c * g to mean
the sum of that vector, c . g = |c * g|. The second demo adds the norms of a
and b and the dot product to the legend, and his warning from playing with
it is that it is difficult to see the relationship between the two lengths
and the dot product. It is not simply the norms multiplied together.
Something else is going on.

    import numpy as np
    c,g = np.array([3,4,2]),np.array([2,1,4]) ## costs, order
    print(c*g) ## [6 4 8], elementwise
    print(np.sum(c*g), np.dot(c,g)) ## 18 18, dot both ways

## 8. What the dot product measures: the angle formula

The something else is an angle. The second way to write the dot product is

    a . b = ||a|| times ||b|| times cos(theta)

where theta is the angle between a and b, measured at the origin where both
arrows start (added definition). He says the derivation comes from
trigonometry and right triangles and does not spend time on it. His
description of cosine: it tells you the size of the angle horizontally
across from the 90 degrees in a right triangle, and visually you imagine a
right triangle wedged between a and b.

His interpretation is the sentence to memorize: the dot product is the length
of the first vector times the length of the second vector, "penalized by how
aligned or not aligned the two vectors are." The cosine is the alignment
share that weights the product of the lengths. As a number (added) cosine
runs from 1 at angle zero, same direction, through 0 at 90 degrees, no shared
direction, to -1 at 180 degrees, opposite directions. So the dot product is
the full product of the lengths when the vectors agree, nothing when they
are perpendicular, and negative when they point against each other.

Two roads to the same number. Take a = (3, 0) and b = (2, 2). The component
road: 3 times 2 plus 0 times 2 is 6. The angle road: ||a|| is 3, ||b|| is the
square root of 8, about 2.828, the angle between them is 45 degrees because a
lies on the x axis and b sits halfway between the axes, and cos 45 is about
0.707. Multiply: 3 times 2.828 times 0.707 is 6.0. The component road is what
you compute. The angle road is what it means.

His demo swaps the knobs so you set each vector's angle and norm and the plot
reports the norms, theta, the cosine, and the dot product. See the
visualizeDotProduct2 widget. He walks the two extremes. At 90 degrees the
vectors share no directionality, the cosine is 0, and the dot product is 0 no
matter how big or small he makes them. At the same angle the cosine is 1 and
the dot product is the product of the norms, his demo values being lengths 1
and 2.4 with a dot product of 2.4.

Three more with numbers you can check. a = (3, 0) and b = (0, 5): 0 plus 0,
dot product 0, lengths 3 and 5, no alignment. a = (1, 2) and b = (3, 6),
where b is a scaled by 3 so they point the same way: 3 plus 12 is 15, and the
product of the norms, root 5 times root 45, about 2.236 times 6.708, is 15.0,
full credit. a = (2, 1) and b = (-1, 2): -2 plus 2 is 0, perpendicular even
though neither lies on an axis. The dot product finds the right angle without
a protractor.

**Perpendicular** means at 90 degrees, also called orthogonal, and two
perpendicular vectors always have a dot product of zero (added definition).
File that one. Section 12 leans on it hard.

## 9. Vectors in Python: lists, then NumPy

Lecture 6 turns the notation into a tool. Python lists are "a very natural
candidate" for representing vectors: a list of numbers is an ordered list of
numbers. He builds one by writing the numbers in square brackets, or
programmatically with range wrapped in list, his range running from six up to
but not including ten.

The componentwise operations all work on lists with a for loop. Addition: for
each position, grab that component of A and the same component of B, add
them, append to a new list C. Dot product: start a running sum at 0, and for
each position multiply the matching components and add the product in. His
example's dot product is 80. Norm: start at 0, square each element and add it
in, take the square root of the total. His example's norm is 5.48. For a =
[2, 6, 3] and b = [4, 1, 5], the sum is [6, 7, 8], the dot product is 8 + 6 +
15 = 29, and the norm of a is the root of 4 + 36 + 9 = 49, which is 7.

    import math
    a,b = [2,6,3],[4,1,5] ## two same-length lists
    c = [a[i]+b[i] for i in range(len(a))] ## sum, [6, 7, 8]
    d = 0.0
    for i in range(len(a)): d += a[i]*b[i] ## dot, 29.0
    n = math.sqrt(sum(x**2 for x in a)) ## norm, 7.0

Each loop is the summation sign from section 1 made literal (added). The
sigma says "for i from 1 to N, add this up." The for loop says the same
thing with a zero-based index.

Two problems with lists, in his words. The operations require an awful lot of
code to do something simple, when the mathematical value of vectors is that
big complicated things get written in a small space. And for very large
vectors the list code slows down. He does not get into why.

**NumPy** is the fix, a widely used Python package for representing vectors
and other objects from linear algebra, usually abbreviated to np. His two
reasons it is great: it gives you the same kind of abstraction you get
writing math on paper, and it is much faster because of something called
**vectorized computation**, which removes the need for the for loops (added
definition: doing an operation on a whole array at once instead of looping
over its elements). His explanation, offered with the caveat that it is not
a focus of the course: Python is a high level language, but what sits
underneath its objects is written in C, and every time Python code runs it
drops down to C, does the operation, and translates the result back up. NumPy
spends more time in C and bounces between the two less. His escape hatch:
"If that explanation didn't mean anything to you, that's okay." NumPy is
"clearly the better tool" and it is what the course uses.

Import it and alias it, import numpy as np. Build a vector by wrapping a
list in np.array, or programmatically with np.arange, NumPy's version of
range. Printed, an array looks like a list. Its type is different: print
type(a) and you get numpy.ndarray, not list. Then the three operations become
three calls. Addition is a + b. Dot product is np.dot(a, b). Norm lives in
NumPy's built-in linear algebra library, reached as np.linalg, so it is
np.linalg.norm(a). His reaction: "Look at how much less code was required."

    import numpy as np
    a,b = np.array([2,6,3]),np.array([4,1,5])
    print(a+b) ## [6 7 8]
    print(np.dot(a,b)) ## 29
    print(np.linalg.norm(a)) ## 7.0

One trap (added): a + b on two arrays is componentwise, but a + b on two
Python lists concatenates them into one longer list. Same symbol, different
meaning, which is why the type matters.

The lecture closes with a benchmark. Two functions, one adding two lists in a
loop and one adding two NumPy arrays with a + b, each just the earlier code
wrapped in a function. Then two list vectors holding the numbers from zero
to a million and two NumPy arrays with exactly the same elements, the only
difference being the type. **timeit** is a Jupyter magic command, written
with a percent sign, that runs the code after it multiple times and reports
the average, seven runs in his case, and he says you do not need to
understand magic commands for this course. His results as stated in the
recording: 1.68 milliseconds for the lists and 944 microseconds for NumPy. A
millisecond is one thousandth of a second, a microsecond is one millionth.
His conclusion: a huge speed up for exactly the same mathematical function.
On the machine these notes were written on, 200,000 element vectors added in
about 7.9 ms with lists and about 0.43 ms with NumPy, a ratio near 19 to 1
(added, measured with the timeit module). Your numbers will differ. The
direction will not.

## 10. Matrices: grids of numbers, and images as data

Lecture 7 brings the second big object. A **matrix** is a set of row vectors
or a set of column vectors, depending on how you want to use it. If a vector
is a list of numbers, a matrix is a grid of numbers. The same grid reads two
ways: down, each column is a vector, or across, each row is a vector, and
which reading you use depends on the use case.

A hardware shop tracks three products across two stores, rows for stores and
columns for products:

    M = [[5, 1, 7],
         [2, 8, 3]]

Read across and each row is one store's stock vector, (5, 1, 7) for store one.
Read down and each column is one product's stock across stores, (5, 2) for
product one. Same numbers, two different vectors, depending on the question.

The **dimension** of a matrix is the number of rows, then the number of
columns, in that order, always. His example M with four column vectors of
three components each is 3 by 4. The hardware shop is 2 by 3. Rows first is
the convention the whole field runs on, and every rule about which matrices
can be multiplied or added is a rule about dimensions (added). Vectors are
just one dimensional matrices.

A matrix with rows and columns is two dimensional, but matrices can be higher
dimensional. Instead of a rectangle, picture a brick or a cube, a stack of
matrices, and in that case think of the object as a set of matrices rather
than a set of vectors. These are sometimes called **tensors** in machine
learning. A spreadsheet is a matrix and a workbook of identical sheets is a
tensor (added). One more index, nothing mystical.

Matrices do two jobs. The first is holding data, and his example is an image.
A PNG, portable network graphic, is an x by y grid with three matrices
stacked back to back, which makes it a tensor. He loads a flower image and
shows that in Python it is a three dimensional NumPy array: rows, columns,
and a third dimension of length three. See the flower and the printed slices
in the Lecture 7 notebook section "Matrices as Data". The first two
dimensions are the pixel coordinates. The third is the color, written as
**RGB**: three numbers, each on the range 0 to 255, saying how much red,
green, and blue to mix. Layer one of the image is a matrix of red values,
layer two a matrix of green values, and slicing one layer out gives a plain
two dimensional matrix.

A two pixel by two pixel image as a 2 by 2 by 3 tensor, pure red top left,
pure green top right, pure blue bottom left, white bottom right:

    import numpy as np
    img = np.array([[[255,0,0],[0,255,0]],[[0,0,255],[255,255,255]]])
    print(img.shape) ## (2, 2, 3)
    print(img[:,:,0]) ## red layer: [[255 0],[0 255]]

The red layer lights up where the pixel is red or white and nowhere else.
That slice is exactly what he prints for the flower.

## 11. Matrices as functions: multiplication, rotate, scale, shear

The second job. A matrix can represent a linear transformation of other
matrices or vectors, and in that case you think of the matrix as a function.
When a matrix is used that way, the function is called a **linear map** or a
linear system of equations, and later lectures build on it. His two reasons
for writing functions as matrices: it can accelerate computation, NumPy again,
and it gives an intuitive explanation of what the function does. A function
like f(x) = 3x takes a number in and gives a number out. A matrix used as a
function takes a vector, or a whole matrix of points, in and gives a
transformed one out (added). The matrix is the f.

Before the function can act, the operation has to exist. Component-wise
operations come first: two matrices of the same size can be added,
subtracted, multiplied, and divided component by component, same slot in,
same slot out, written with plain +, -, and *. For

    M = [[2, 0],      N = [[1, 4],
         [1, 3]]           [2, 5]]

M + N is [[3, 4], [3, 8]] and M * N is [[2, 0], [2, 15]]. Nothing crosses rows
or columns.

Then the one that is not component-wise. **Matrix multiplication**, also
called the matrix dot product and written with the same dot as the vector
dot product, is what is meant when a matrix M "acts on" a matrix N. The rule:
think of M as a set of row vectors and N as a set of column vectors, and the
dot product of M and N is the vector dot product of each row of M with each
column of N. Row i of M with column j of N lands in row i, column j of the
result. Top left is row one with column one, next to it row one with column
two, and so on until every pair is done. For the same M and N:

    top left     = (2,0) . (1,2) = 2*1 + 0*2 = 2
    top right    = (2,0) . (4,5) = 2*4 + 0*5 = 8
    bottom left  = (1,3) . (1,2) = 1*1 + 3*2 = 7
    bottom right = (1,3) . (4,5) = 1*4 + 3*5 = 19

so M . N is [[2, 8], [7, 19]], against M * N of [[2, 0], [2, 15]]. Same
matrices, different operation, different answer. And N . M is [[6, 12],
[9, 15]], so order matters: "M acts on N" and "N acts on M" are different
functions (added).

    import numpy as np
    M,N = np.array([[2,0],[1,3]]),np.array([[1,4],[2,5]])
    print(M*N) ## component-wise, [[2 0],[2 15]]
    print(np.dot(M,N)) ## matrix product, [[2 8],[7 19]]

His example multiplies two 3 by 3 matrices, but in general you need only that
the second dimension of M matches the first dimension of N. Columns of M
equals rows of N, because each row of M is dotted with each column of N and a
vector dot product needs both vectors the same length (added). The result is
rows of M by columns of N. A 2 by 3 times a 3 by 2 gives a 2 by 2, and a 3 by
2 times a 3 by 2 makes NumPy refuse, because 2 against 3 on the inside leaves
no matching lengths.

That is why the **rotation matrix** works. Pick an angle theta and build the
2 by 2 from cosine and sine values:

    R(theta) = [[cos(theta), -sin(theta)],
                [sin(theta),  cos(theta)]]

Encode the x-y coordinates of a scatter plot as a 2 by n matrix P, x values
along the top row and y values along the bottom, one column per point. R is
2 by 2, P is 2 by n, the inner 2 matches, and R(theta) . P is the same cloud
of points rotated around the origin by theta. His demo rotates a scatter
plot with a slider. See the dinosaur widget in the Lecture 7 notebook section
"Matrices as Functions". Put (1, 0), (0, 1), and (1, 1) into P as columns and
rotate by 90 degrees, where cos is 0 and sin is 1, so R is [[0, -1], [1, 0]]:

    R . P = [[0, -1],  . [[1, 0, 1],   = [[0, -1, -1],
             [1,  0]]     [0, 1, 1]]      [1,  0,  1]]

(1, 0) went to (0, 1), a quarter turn counterclockwise, (0, 1) went to
(-1, 0), (1, 1) went to (-1, 1). Every point turned the same amount around the
origin. Angles go in as radians, so pi over two for 90 degrees (added).

Two more functions. Scaling: pick a number a and build M = [[a, 0], [0, a]].
Acting on points, it rescales their distance from the origin by a factor of
a, and if a is negative the points pass through the origin and reflect to the
opposite quadrant. With a = 2 the three points above become (2, 0), (0, 2),
(2, 2), twice as far out. With a = -1 they become (-1, 0), (0, -1), (-1, -1).
And the **shear**, in his words a matrix that shrinks in one dimension by a
factor a and stretches in another by a different factor b. His shear matrix
is the product of two matrices, one carrying each factor:

    [[1, a],  . [[1, 0],  = [[1 + a*b, a],
     [0, 1]]     [b, 1]]     [b,       1]]

It shears horizontally by a and vertically by b. Changing a shifts points
horizontally and their vertical position does not change at all. Changing b
gives the vertical shear. See the shear widget with its red and blue
reference vectors, right after the scaling demo. With a = 0.5 and b = 0 the
matrix is [[1, 0.5], [0, 1]], and acting on (1, 0), (0, 1), (1, 1) it gives
(1, 0), (0.5, 1), (1.5, 1). The point on the floor did not move, there was no
height to shift. Every other point slid sideways by half its height, and no
height changed. That is a horizontal shear (added). The red and blue arrows
in the demo are the x and y directions after the shear. Watch which one
tilts and which one stays put, because that is where Lecture 8 begins.

## 12. Eigenvectors: the directions a matrix leaves alone

Sometimes it is helpful to find the vectors that remain unchanged by a linear
transformation. Lecture 8 shows it with the shear. Each choice of a and b
gives a different matrix M, and as b varies M changes every time the slider
moves, but the blue reference vector does not change. Its angle stays put no
matter what b is. See the shear widget in the Lecture 8 notebook section
"Eigenvectors & Eigenvalues" and watch the blue arrow while moving b. That is
the thing being hunted.

A vector v is an **eigenvector** of the matrix M if v does not rotate when M
acts on it. The dot product of M and v is a new vector with the same exact
angle as v, which can shrink or grow along that original axis but cannot
turn. As an equation:

    M . v = lambda * v

The left side is M acting on v, the right side is v scaled by some number
lambda. That number is the **eigenvalue** corresponding to the eigenvector v,
and it represents the amount of shrinking or growing that happens to v when M
acts on it. In words (added): M does to v exactly what multiplying by the
number lambda does. A whole grid of numbers collapses to one number, but only
for that special direction.

A matrix that stretches x by 3 and y by 2, M = [[3, 0], [0, 2]], sends (1, 0)
to (3, 0), which is 3 times (1, 0), and sends (0, 1) to (0, 2), which is 2
times (0, 1). Those two are eigenvectors with eigenvalues 3 and 2. It sends
(1, 1) to (3, 2), which is not any number times (1, 1), so (1, 1) is not an
eigenvector at all. Two directions survive untouched, every other direction
gets turned (added).

Certain types of matrices yield eigenvectors and eigenvalues with really
useful properties for describing the matrix as a function, and his case is
the **symmetric matrix**: the top right triangle is the mirror image of the
bottom left triangle. Draw a line from the top left corner to the bottom
right, flip the top right triangle over it, and it lines up. His picture is a
4 by 4 with letters in mirrored positions. See the colored 4 by 4 in the
notebook right after the eigenvector definition. A 3 by 3 like
[[1, 4, 7], [4, 2, 9], [7, 9, 3]] is symmetric: the 4 sits at row 1 column 2
and again at row 2 column 1, the 7 and the 9 mirror the same way, and the
diagonal is its own mirror image. Symmetric matrices show up whenever a grid
records a relationship that runs both ways (added), a distance table between
cities being the everyday case, and section 16's covariance matrix being the
one that matters here.

The payoff: a symmetric n by n matrix will have n unique eigenvectors, and
they will all be mutually perpendicular to each other. If v1 and v2 are
different eigenvectors of a symmetric M, they are oriented 90 degrees apart,
which is the same as saying their dot product is zero. That is section 8's
rule about perpendicular vectors, applied to eigenvectors (added). For
M = [[2, 1], [1, 2]], M sends (1, 1) to (3, 3), eigenvalue 3, and sends
(1, -1) to (1, -1), eigenvalue 1, and (1, 1) . (1, -1) = 1 - 1 = 0. A right
angle, as promised. For a matrix that is not symmetric, M = [[2, 1], [0, 3]],
the eigenvectors are (1, 0) with eigenvalue 2 and (1, 1) with eigenvalue 3,
and their dot product is 1. Two perfectly good eigenvectors, 45 degrees apart.
The perpendicular guarantee is a symmetric-matrix guarantee and nothing more.

    import numpy as np
    S,T = np.array([[2,1],[1,2]]),np.array([[2,1],[0,3]])
    for M in (S,T):
        vals,vecs = np.linalg.eig(M) ## columns of vecs are eigenvectors
        print(vals, np.dot(vecs[:,0],vecs[:,1])) ## 0.0 for S, 0.707 for T

The whole set of eigenvectors, v1 through vn, effectively defines a new
coordinate system, like the typical x-y plane but rotated or spun a little.
That coordinate system is called M's **eigenspace**, and it is useful for
describing data in what are called the natural coordinates for that dataset.
The x and y axes are a choice, not a law (added). If a cloud of points is
stretched along a diagonal, the natural axes are along the stretch and
across it, and for a symmetric matrix built from that data the eigenvectors
are those axes and the eigenvalues say how much stretch lives along each. In
the example above, (1, 1) and (1, -1) are the x-y axes turned 45 degrees.

His warning closes the lecture: these interpretations are only true if M is
symmetric. If M is not symmetric, or does not meet some other very specific
requirements, this may not be the case for its eigenvectors. The definition
itself survives for any square matrix (added). What you lose without symmetry
is the guarantee of n perpendicular ones.

## 13. Finding them: the identity, the determinant, the quadratic

How do you find eigenvalues and eigenvectors for a given M? Lecture 9 is the
heaviest of the module, and the mechanics are all 2 by 2 arithmetic. For an n
by n matrix there will be n eigenvectors and n corresponding eigenvalues,
with subscripts pairing them. The strategy in general: first find all the
eigenvalues, then use each one to find its eigenvector through the rescaling
relationship. The definition has two unknowns in it, v and lambda, and the
trick below removes v, leaving an equation in lambda alone (added).

The trick needs a special matrix. The **identity matrix** I is a square n by
n matrix with ones along the main diagonal from top left to bottom right and
zeros everywhere else. It is called identity because if it acts on any matrix
or any vector, you just get that matrix or vector back. I . (7, -2) is
(7, -2), and I . A is A for any A. It is the matrix version of multiplying by
one (added), and it is needed because the eigenvector equation has a matrix
on one side and a plain number on the other. lambda times I is that number
dressed up as a matrix.

Start from M . v = lambda * v and subtract M . v from both sides. The left
side becomes a vector full of zeros. Both terms on the right contain v, so
pull it out front as if v were a number, and what is left is

    (lambda * I - M) . v = 0

There are two ways that can be true. The first is that v is the zero vector,
which works for any matrix M every time. That is the **trivial solution**,
and it is ignored because it says nothing about M compared to any other
matrix. If v is not the zero vector, then the stuff in the parentheses,
lambda * I - M, must somehow be equal to zero. Which is strange. lambda * I
is an n by n matrix, M is an n by n matrix, their difference is still an n by
n matrix, and it does not have to be a matrix full of zeros. It has to be
zero in a different sense.

The sense that matters: does M, as a function, stretch the x-y plane in a
way that shrinks space so that the resulting area is zero? His own words are
"I know this is a bit weird to think about," and then he gives a picture. The
**unit square** has base and height 1 and its bottom left corner at the
origin, with corners (0, 0), (0, 1), (1, 1), and (1, 0), written as the
columns of a 2 by 4 matrix S. Keep M generic as [[a, b], [c, d]] and let it
act on S. Row one of M dotted with each column, then row two:

    M . S = [[0, b, a + b, a],
             [0, d, c + d, c]]

The result still has four corners, but instead of the unit square it defines
a parallelogram, a rectangle warped a little. Different a, b, c, d give
different parallelograms with different areas, and sometimes the area is
zero. That is the case being hunted. With M = [[3, 1], [1, 2]] the corners
land at (0, 0), (1, 2), (4, 3), (3, 1), a tilted parallelogram with real
area. With M = [[2, 4], [1, 2]] they land at (0, 0), (4, 2), (6, 3), (2, 1),
and every one of those points sits on the line y = x/2. The square has been
flattened onto a single line, and a flat parallelogram has area zero. That
matrix is "somehow equal to zero" in the sense the lecture means: not a
matrix of zeros, but a function that squashes a whole plane down to a line
(added). See the parallelogram widget with its a, b, c, d dials in the
Lecture 9 notebook section "Finding Eigenvalues and Eigenvectors". With a = 1,
d = 1, b = c = 0 it is the identity and you get the square back, sliding a or
d stretches it into a rectangle, and b and c produce the slant.

With some work you can recover the geometry and show that the area of the
parallelogram is a*d - b*c, using nothing but the four numbers in M. His
sanity check: when b and c are zero the shape is a rectangle, rectangle area
is base times height, which is a*d, and the b*c term is the correction for
the slant. That number is the **determinant**: for a 2 by 2 matrix, the area
of the parallelogram M produces when it acts on the unit square,

    det(M) = m11 * m22 - m12 * m21

with subscripts row then column. For [[3, 1], [1, 2]] it is 6 - 1 = 5 and the
parallelogram measures 5. For [[2, 4], [1, 2]] it is 4 - 4 = 0. For a 3 by 3
the same approach uses a unit cube and measures the volume of the three
dimensional parallelogram, with the formula for rows (a, b, c), (d, e, f),
(g, h, i) being aei + bfg + cdh - ceg - bdi - afh. For [[1, 2, 0], [0, 3, 1],
[2, 0, 1]] that is 3 + 4 + 0 - 0 - 0 - 0 = 7. For 4 by 4 and up it is the
same idea with the higher dimensional equivalent of volume and very long
equations, which he says you can certainly sort out, and then he returns to
2 by 2. Nobody expands a 5 by 5 by hand (added). np.linalg.det does it.

Back to the real question: when is the determinant of lambda * I - M zero?
Multiply through. lambda * I puts lambda on the diagonal and zeros off it,
and subtracting M component-wise gives

    lambda * I - M = [[lambda - m11,  -m12       ],
                      [-m21,          lambda - m22]]

Apply the determinant, careful with the minus signs:

    0 = (lambda - m11)(lambda - m22) - (-m12)(-m21)
    0 = lambda^2 - lambda(m11 + m22) + (m11*m22 - m12*m21)

Look at the lambdas. A scalar times lambda squared, minus a scalar times
lambda, plus a constant. This is just a quadratic equation, and quadratic
equations have a formula. He writes the leading coefficient as (1) so the a
of the formula is visible. Two shortcuts (added): m11 + m22 is the sum of the
diagonal and m11*m22 - m12*m21 is det(M), so for any 2 by 2 the equation is
lambda squared minus the diagonal sum times lambda plus the determinant,
equals zero. Two numbers off the matrix and you have it.

The **quadratic formula**: for a*x^2 + b*x + c = 0,

    x = (-b plus or minus sqrt(b^2 - 4ac)) / 2a

Here a is 1, b is -(m11 + m22), and c is m11*m22 - m12*m21, so

    lambda = ((m11 + m22) plus or minus
              sqrt((m11 + m22)^2 - 4(1)(m11*m22 - m12*m21))) / 2(1)

The plus or minus yields two different eigenvalues, lambda1 with the plus and
lambda2 with the minus, two for a 2 by 2 as promised. For M = [[4, 2],
[1, 3]] the diagonal sum is 7 and the determinant is 12 - 2 = 10, so the
equation is lambda^2 - 7 lambda + 10 = 0, and lambda = (7 plus or minus
sqrt(49 - 40)) / 2 = (7 plus or minus 3) / 2. So lambda1 = 5 and lambda2 = 2.
For section 12's symmetric [[2, 1], [1, 2]], diagonal sum 4 and determinant
3 give lambda^2 - 4 lambda + 3 = 0 and lambda = (4 plus or minus 2) / 2,
which is 3 and 1, the two values found by inspection there.

## 14. From eigenvalue to eigenvector, by hand and in NumPy

Now v1, the eigenvector for lambda1. Call its components a and b, so v1 =
(a, b), and ask what a and b are. What is known: lambda1 is a number, and M
acting on v1 equals v1 rescaled by lambda1. Write the left side out as two
row dot products:

    m11*a + m12*b = lambda1 * a
    m21*a + m22*b = lambda1 * b

Treat those as two separate equations and subtract lambda1*a and lambda1*b
from the respective sides:

    0 = (m11 - lambda1)*a + m12*b
    0 = m21*a + (m22 - lambda1)*b

Both must be true at once. The trivial solution a = 0, b = 0 shows up again
and is ignored for the same reason. Focus on the first equation: subtract
m12*b from both sides, divide by (m11 - lambda1), and move the minus sign
into the denominator:

    a = (m12 / (lambda1 - m11)) * b

Now a is written as a function of b, and v1 = ((m12 / (lambda1 - m11)) * b,
b). If lambda1 happens to equal m11 that division fails, and the second row
equation does the same job solved for b in terms of a (added). For M = [[4,
2], [1, 3]] with lambda1 = 5: a = (2 / (5 - 4)) * b = 2b, so v1 = (2b, b).
Pick b = 1 and v1 = (2, 1). Check: M . (2, 1) = (8 + 2, 2 + 3) = (10, 5) = 5
* (2, 1). For lambda2 = 2: a = (2 / (2 - 4)) * b = -b, so v2 = (-1, 1).
Check: M . (-1, 1) = (-4 + 2, -1 + 3) = (-2, 2) = 2 * (-1, 1). Both checks
land exactly on lambda times the vector, and that check is the whole
definition, so if it passes you are done.

At this point the method is stuck on b, and that is not a flaw. There are
infinitely many eigenvectors associated with a given eigenvalue, found by
varying b, and every one of them is aligned with the others: same angle,
different length, rescaled versions of each other. Whatever b you choose, M
acting on that vector gives back a version scaled by lambda1. (2, 1), (4, 2),
and (-6, -3) are all eigenvectors for lambda1 = 5 (added). His demo lets you
tune m12, m11, and lambda1 and slide b, and every vector it produces lines up
on one axis. See the v1 slider widget at the end of the Lecture 9 notebook
section.

The method on one page (added): diagonal sum and determinant off the matrix,
quadratic formula for the two lambdas, first row equation for a in terms of
b with b = 1, then check M . v against lambda * v.

    import numpy as np
    M = np.array([[4,2],[1,3]])
    tr,det = M[0,0]+M[1,1],np.linalg.det(M) ## diagonal sum, determinant
    lams = ((tr+np.sqrt(tr**2-4*det))/2,(tr-np.sqrt(tr**2-4*det))/2) ## quadratic formula
    for lam in lams: v = np.array([M[0,1]/(lam-M[0,0]),1]); print(lam.round(6), v, np.dot(M,v), lam*v) ## last two match

Lecture 10 makes all of that one call. NumPy's linear algebra library has a
function that returns both the eigenvalues and the eigenvectors for a square
matrix:

    import numpy as np
    M = np.array([[4,2],[1,3]])
    vals,vecs = np.linalg.eig(M) ## eigenvalues, eigenvector matrix
    print(vals) ## [5. 2.]
    print(vecs.round(4)) ## [[ 0.8944 -0.7071] [ 0.4472  0.7071]]

How to read it: the first item is the list of eigenvalues, the second is a
matrix where each column is a single eigenvector, matched by position. The
eigenvector for 5 is the first column, (0.8944, 0.4472), and the eigenvector
for 2 is the second, (-0.7071, 0.7071). Compare to the hand result (added):
(2, 1) divided by its length root 5 is (0.8944, 0.4472), and (-1, 1) divided
by root 2 is (-0.7071, 0.7071). Same lines, different lengths. Remember there
are infinitely many eigenvectors per eigenvalue, all with the same angle, so
NumPy has to give you one of them, and it chooses the one with unit length,
norm equal to one. He verifies that by asking for the norm of each column,
and both come back as one:

    print(np.linalg.norm(vecs,axis=0)) ## [1. 1.], one norm per column

axis=0 means work down each column (added). Do not read the eigenvector
matrix by rows. The rows mean nothing on their own.

## 15. Programming matrices

The rest of Lecture 10 is the matrix toolkit. Just as with vectors you could
use nested lists, lists of lists, but you will have a much better time using
NumPy arrays. Write a list containing lists, one inner list per row, and wrap
it in np.array. Printing shows the grid. The type, even though it looks like
a list when printed, is numpy.ndarray. Three by three and up works the same
way with more inner lists.

    import numpy as np
    M = np.array([[5,1,7],[2,8,3]]) ## two rows, three columns
    print(M) ## [[5 1 7] [2 8 3]]
    print(type(M)) ## <class 'numpy.ndarray'>

NumPy has functions for creating a matrix of a given size filled with ones or
with zeros, the size given as rows by columns, and his trick for a matrix of
all fours is a matrix of ones multiplied by the scalar 4, which is section
5's scaling rule applied to a grid (added).

    print(np.ones((2,5))) ## 2 by 5 of 1.
    print(np.zeros((4,2))) ## 4 by 2 of 0.
    print(7*np.ones((3,3))) ## 3 by 3 of 7.

The size goes in as a tuple with its own parentheses inside the function's
parentheses, and the values print with a decimal point because these
builders make floating point matrices (added).

For looking at a matrix the course relies on Matplotlib's pyplot, aliased
plt. He builds a 20 by 30 matrix of random numbers drawn uniformly between
zero and one with NumPy's random number generator, then plots it with
imshow, the image show function, which colors each cell of the grid by the
number in it, and adds plt.colorbar so you can read which number each color
means. See the noisy random grid in the Lecture 10 notebook section "Python
Programming with Matrices". It looks like noise because it is noise.

    import numpy as np, matplotlib.pyplot as plt
    M = np.random.random((10,15)) ## 10 by 15, uniform 0 to 1
    plt.imshow(M) ## one colored cell per number
    plt.colorbar() ## which color means which number

Every matrix has a property called shape that gives its dimensions as
(rows, columns), no parentheses after it because it is a property, not a
call (added). To read one number, index with row position and column
position in one pair of square brackets, M[row, column]. For a whole column,
put a colon in the row slot, M[:, column]. For a whole row, colon in the
column slot, M[row, :]. The lengths tell you which is which: in his 20 by 30
example a column comes back with length 20 because there are 20 rows, and a
row comes back with length 30. NumPy counts positions from zero, so M[0, 0]
is the top left cell (added).

    M = np.array([[5,1,7],[2,8,3]])
    print(M.shape) ## (2, 3)
    print(M[1,2]) ## row position 1, column position 2: 3
    print(M[:,1]) ## column position 1: [1 8], length 2
    print(M[0,:]) ## row position 0: [5 1 7], length 3

This is the indexing that section 11's point matrices and section 14's
eigenvector matrix both rely on (added): P[0, :] is every x value, and
vecs[:, 0] is the first eigenvector.

## 16. Why bother: eigenfaces and compression

Lecture 11 answers the question the eigenvector lectures earn. Why go through
all this work? His scope note first: this section is just an example, it
probably will not be covered on homeworks or on the final exam, and a future
machine learning course covers it in greater detail under **principal
component analysis**, where eigenvectors and eigenvalues emerge naturally.

The idea: eigenvectors represent a convenient way to cluster large datasets by
identifying redundant information, and if you can identify redundant
information you can possibly compress the space needed to store the data. If
two columns of your data always move together, you are paying to store the
same fact twice (added). Eigenvectors find the directions where the data
really varies, the eigenvalues rank those directions by how much variation
lives along each, and you keep the big ones.

His dataset is an image, a black and white photo of former US President
George W. Bush, from **scikit-learn** (sklearn), a very useful Python package
for machine learning that also ships standardized datasets used for teaching
and for benchmarking one algorithm against another. One of them is a
collection of famous people's faces. The image is just a matrix, 62 rows by
47 columns, each number between zero and one telling imshow how black or
white that cell should be, drawn with a color map called bone. See the face
and its printed matrix in the Lecture 11 notebook section "Example Eigenfaces
and Data Compression". A grayscale image needs only one layer, so unlike
section 10's flower it is a plain two dimensional matrix (added).

What PCA does, in his steps. Hand it a matrix of data, not necessarily
square. It looks at every pair of columns and asks how correlated they are,
and from those pairwise correlations it builds a square matrix, which he
calls the covariance matrix. Compute that square matrix's eigenvectors and
eigenvalues. The eigenvectors with the largest eigenvalues identify the
dimensions along which most of the information in the dataset is occurring.
Keep some number of those and throw out the rest, projecting the big matrix
down to a smaller one with fewer dimensions. **Correlation** here means how
strongly two columns move together (added definition, he defers the full
definition to a later class). The notebook's markdown adds that the axes
PCA finds are the eigenvectors, that they are orthogonal to each other, that
each eigenvalue is the amount of variance along its eigenvector, and that
the eigenvector with the highest eigenvalue is the first principal component.

Here is where section 12 pays off (added). The covariance matrix has one row
and one column per data column, and the entry for columns i and j is the
same as for j and i, so it is symmetric. That is exactly the condition under
which the eigenvectors come out perpendicular and form a clean new
coordinate system, the eigenspace, for the data. PCA is the eigenspace with
the small axes thrown away.

His demo has a slider for the number of eigenvectors. Using n of them
projects the 62 by 47 image down to 62 by n. With just two, the result is
62 by 2, which he reports as about 4 percent of the original size, a very
big compression with a ton of data thrown out. The small matrix plus the PCA
algorithm can then be projected back up to 62 by 47, with some information
lost. The two eigenvector rebuild is pretty blurry, not a whole lot like the
original face, but with recognizable elements: dark blobs roughly where the
eyes go, a vertical stripe where the nose should be, dark blobs for
nostrils, a horizontal line for the mouth. What gets recovered is the major
patterns. See the side by side original and compressed images under the
slider. With three, four, or five eigenvectors the rebuild looks more and
more like the face, around 10 to 15 it is pretty close to the human eye, and
his closing setting of 14 eigenvectors, the 14 corresponding to the 14
largest eigenvalues of the covariance matrix, gives 62 by 14, about 30
percent as much data, and a visibly similar face. The ratio is n over 47
because the rows are kept (added): 2 over 47 is about 4 percent, 14 over 47
about 30.

The notebook's code shape, which the recording runs without narrating
(added):

    from sklearn.decomposition import PCA
    pca = PCA(n_components=n) ## keep n eigenvectors
    small = pca.fit(bush).transform(bush) ## 62 by n
    rebuilt = pca.inverse_transform(small) ## back to 62 by 47

And the whole pipeline in plain NumPy on a matrix small enough to read, using
only functions from this module (added). Six readings from three sensors on
the same machine, where sensor two reads about one degree above sensor one
and sensor three reads about double. Three columns, one piece of information.

    import numpy as np
    X = np.array([[10,11,21],[12,13,24],[11,12,22],[14,16,28],[13,14,27],[15,16,30]],float)
    Xc = X-X.mean(axis=0) ## center each column on its mean
    vals,vecs = np.linalg.eig(np.dot(Xc.T,Xc)/(len(X)-1)) ## covariance, then eigen
    w = vecs[:,[np.argmax(vals)]] ## keep the top eigenvector only
    Xhat = np.dot(np.dot(Xc,w),w.T)+X.mean(axis=0) ## down to 6 by 1, back to 6 by 3
    print(np.sort(vals)[::-1].round(2), np.abs(Xhat-X).max().round(2)) ## [20.21 0.19 0.03] 0.62

The eigenvalues, largest first, are 20.21, 0.19, and 0.03, which is 98.9
percent, 0.9 percent, and 0.1 percent of the total variance. One eigenvector
carries almost all of it. Keeping that one stores 6 numbers instead of 18, a
third of the space, and the rebuilt readings are within 0.62 of the originals
everywhere. Keep two and the worst error drops to 0.15. That is the face demo
with numbers you can check by hand, and it is every piece of the module in
one place: the columns are vectors, the dot product built the covariance
matrix, the matrix is symmetric so its eigenvectors are perpendicular, the
eigenvalues rank the directions, and matrix multiplication projects the data
down and back up.

The circular definition from section 1 has an answer now (added, a reading
of the module, not his definition). Linear algebra is the study of what
happens when you write the world as numbers times
variables, added up. Vectors sort the numbers, matrices act on them, and
eigenvectors tell you which directions the action leaves alone. Everything
in between was arithmetic.
