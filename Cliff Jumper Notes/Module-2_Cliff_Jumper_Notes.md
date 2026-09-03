# Module 2: Cliff Jumper Notes

*One continuous lesson stitched from the five Module 2 cliff notes of
Mathematical and Statistical Foundations (CMPINF-2105): how a table of
purchases and a stack of receipts becomes a system of linear equations, how
substitution solves it, why that procedure is called Gaussian elimination and
what it secretly builds (the inverse matrix), the three ways the method
breaks, how NumPy and SymPy do the same work in Python and why they show
different-looking answers to the same question, and one place in the real
world, gravitational lensing, where inverting a matrix tells astronomers
where a galaxy actually is.*

This document is written for a reader who is comfortable applying math and
less comfortable with the theory behind it. Every idea arrives attached to a
concrete case, and the case is the same one all the way through. Anything
the lectures did not say is marked "(added)" so you always know what is the
professor's and what is connective tissue.

## 1. The running example

Module 1 treated vectors and matrices as mathematical objects. Module 2 puts
them to work. The professor's framing for the whole module is a small
purchasing problem: a matrix of who bought how much of what, a vector of what
each person paid, and the question of what each item costs.

The running example in these notes is a coffee shop. Three regulars, three
items.

| Customer | Espresso | Latte | Muffin | Paid |
|---|---|---|---|---|
| Dana | 2 | 1 | 1 | 15 |
| Eli | 1 | 3 | 2 | 26 |
| Faye | 1 | 2 | 2 | 21 |

The left block is the **quantity matrix** Q. Rows are customers, columns are
items, and an entry is how many of that item that customer bought. The right
column is the **cost vector** c, one receipt per row. The unknowns are the
three prices, collected into a **price vector** p = [p1, p2, p3] for
espresso, latte, and muffin in that order.

```
Q = [[2, 1, 1],        c = [15, 26, 21]        p = [p1, p2, p3]
     [1, 3, 2],
     [1, 2, 2]]
```

The relationship the professor writes down is compact: when Q acts on p, the
result must be c. In symbols, Q p = c. Two of the three pieces are known.
The whole module is about recovering the third.

Why this is worth a whole module (added): it is the smallest honest version
of what a fitted model is. Known inputs (the quantities), known outputs (the
receipts), unknown weights in between (the prices). A linear regression is
this setup with more rows and noisier numbers.

## 2. From one matrix equation to three linear equations

Q p = c is shorthand. To work with it, write out what the matrix product
does. Each row of Q takes a dot product with p, and that dot product must
equal the matching entry of c. One row, one dot product, one equation:

```
2p1 + 1p2 + 1p3 = 15     (Dana)
1p1 + 3p2 + 2p3 = 26     (Eli)
1p1 + 2p2 + 2p3 = 21     (Faye)
```

The professor's point: writing the problem this way defines a **system of
linear equations**, one equation per row of the matrix. Three equations,
three unknowns. The job is to find values for p1, p2, p3 that make all three
true at once.

**Linear** means each unknown is only ever multiplied by a plain number and
added to the others. Nothing is squared, no two unknowns multiply each other,
nothing goes through a square root (added definition). That restriction is
what makes the geometry flat and the algebra mechanical. Every equation
above is a flat sheet in three-dimensional price space, and the solution is
the one point where all three sheets meet (added).

The count matters. Each equation is one fact about the prices. Three
unknowns need three independent facts (added). Section 6 is about what
happens when you have fewer than that, or when a fact turns out to be a
repeat.

## 3. Solving by substitution, one unknown at a time

The lecture's strategy is substitution. Take one unknown, use one equation to
write it as a formula in the other unknowns, then push that formula into
every remaining equation so the unknown disappears from them. Repeat on what
is left. Each round the system shrinks by one equation and one unknown. When
one equation with one unknown remains, solve it as a number and walk the
numbers back up the chain.

The mechanics of "write p1 as a formula" are two moves: subtract the other
terms from both sides, then divide by whatever is multiplying p1.

**Forward.** Isolate p1 in Dana's equation:

```
p1 = 15/2 - (1/2)p2 - (1/2)p3
```

Substitute that into Eli's and Faye's equations and combine like terms.
Eli's p2 terms (3 and -1/2) add to 5/2, his p3 terms (2 and -1/2) add to 3/2,
and moving 15/2 across leaves 37/2. Multiply through by 2 to clear the halves.
Faye's reduces the same way to 3p2 + 3p3 = 27, and since her p2 and p3
coefficients match, divide by 3:

```
5p2 + 3p3 = 37
 p2 +  p3 =  9
```

p1 is gone. Two equations, two unknowns. Isolate p2 in the first:

```
p2 = 37/5 - (3/5)p3
```

Substitute into the second. The p3 terms combine to 2/5 and the constants to
8/5:

```
(2/5)p3 = 8/5        so        p3 = 4
```

**Back up.** A muffin costs 4. Put that into the formula for p2: 37/5 - 12/5
= 25/5 = 5. A latte costs 5. Put both into the formula for p1: 15/2 - 5/2 -
4/2 = 6/2 = 3. An espresso costs 3.

So p = [3, 5, 4]. The professor warns that the fractions in a chain like this
get big, and invites you to double check his arithmetic. Take that invitation
for your own work. Substitution is a chain,
and one dropped sign poisons everything below it.

**Check it.** Plug the answer into the ORIGINAL three equations, not the
reduced ones. Dana: 2(3) + 5 + 4 = 15. Eli: 3 + 3(5) + 2(4) = 26. Faye: 3 +
2(5) + 2(4) = 21. The lecture does the same check in NumPy: define the
matrix as an array, define the solution vector, take the dot product, compare
to the cost vector.

```python
import numpy as np
Q = np.array([[2, 1, 1], [1, 3, 2], [1, 2, 2]]) ## rows = customers
p = np.array([3, 5, 4]) ## espresso, latte, muffin
print(Q.dot(p)) ## expect [15 26 21]
```

Output: `[15 26 21]`. Note what this does and does not do (added). It
verifies a solution you already have. It does not find one. Finding one in
code is Section 7.

## 4. The procedure has a name, and it builds something

The substitution procedure is **Gaussian elimination**. The lecture gives its
other names too: **row reduction**, and **backsolving**. Backsolving describes
the two-step shape, down the system from top to bottom, then back up to
collect the unknowns.

Then the professor reframes what Q is. It holds the purchasing data, but
because Q acts on p to produce c, Q is also a function. Feed it prices, get
receipts. And while you were solving for p, you were implicitly building a
second matrix, one that undoes Q. It takes receipts and gives back prices.
That matrix is the **inverse** of Q, written Q^-1.

Multiply both sides of Q p = c by Q^-1 and you get p = Q^-1 c. The inverse
acting on the costs returns the prices. That one line is the reason to want
an inverse. The inverse undoes Q, so Q^-1 acting on Q gives the **identity
matrix**, the matrix with ones on the main diagonal and zeros everywhere else,
which changes nothing it touches (added definition).

**Why the notation is -1.** The professor's analogy: if Q were a number,
multiplying Q by Q^-1 would be dividing Q by Q, which gives one. For matrices
the product gives the identity matrix instead, which plays the role of one.
Carry the analogy carefully (added). There is no division of matrices. There
is "multiply by the inverse," which for ordinary numbers is the same thing as
dividing. Read Q^-1 c as "c with Q's effect removed," not as a fraction.

The practical payoff of having the inverse rather than just the answer
(added): substitution solved one cost vector. The inverse solves every cost
vector. Same shop, new day's receipts, and p = Q^-1 c gives the prices with
no new elimination. You pay for the elimination once and reuse the result.

## 5. Computing the inverse with an augmented matrix

To find Q^-1, the lecture uses the same elimination moves on a different
object. Build an **augmented matrix**: Q with an identity matrix of the same
size attached to its right, a vertical line between them. Then perform
**row operations** (multiply or divide a whole row by a number, or subtract a
multiple of one row from another, treating rows as vectors) until the left
half has become the identity. Whatever then sits in the right half is Q^-1.

Why that works (added): every row operation is itself a small matrix
multiplication applied to both halves. The sequence of operations that turns
the left half from Q into I is, taken together, the matrix that turns Q into
I. That is Q^-1 by definition. The right half started as I and received the
same sequence, so it ends as Q^-1 times I, which is Q^-1. The identity on the
right is a recording device for the moves you made.

**Forward pass, ones on the diagonal and zeros below.** Start:

```
[ 2  1  1 | 1  0  0 ]
[ 1  3  2 | 0  1  0 ]
[ 1  2  2 | 0  0  1 ]
```

Divide row 1 by its first entry, 2, so a one sits in the first position:

```
[ 1  1/2  1/2 | 1/2  0  0 ]
[ 1   3    2  |  0   1  0 ]
[ 1   2    2  |  0   0  1 ]
```

For each remaining row, subtract the first row multiplied by that row's
first-column value. Both lower rows start with 1, so R2 - 1 R1 and R3 - 1 R1:

```
[ 1  1/2  1/2 |  1/2  0  0 ]
[ 0  5/2  3/2 | -1/2  1  0 ]
[ 0  3/2  3/2 | -1/2  0  1 ]
```

The first column reads 1, 0, 0. Repeat on row 2: multiply it by 2/5 to make
its second entry one, then clear below it with R3 - (3/2) R2:

```
[ 1  1/2  1/2 |  1/2   0   0 ]
[ 0   1   3/5 | -1/5  2/5  0 ]
[ 0   0   3/5 | -1/5 -3/5  1 ]
```

Scale row 3 by 5/3 so its third entry is one:

```
[ 1  1/2  1/2 |  1/2   0    0  ]
[ 0   1   3/5 | -1/5  2/5   0  ]
[ 0   0    1  | -1/3  -1   5/3 ]
```

The professor pauses here to make a point. The right half started as the
clean identity and is filling with fractions. That is expected. It is
turning into the inverse, and there is no reason the inverse should look
simple. The diagonal entry you divide by at each step is the **pivot** (added
definition), and the left half is now upper triangular. In equation terms the
last row involves only p3, which is the same "one equation, one unknown"
moment from Section 3, reached with rows instead of sentences (added).

**Backward pass, zeros above.** Subtract multiples of row 3 from rows 2 and 1
to clear the third column (R2 - (3/5) R3 and R1 - (1/2) R3), then subtract a
multiple of row 2 from row 1 to clear the second column (R1 - (1/2) R2):

```
[ 1  1/2  0 |  2/3  1/2  -5/6 ]         [ 1  0  0 |  2/3   0  -1/3 ]
[ 0   1   0 |   0    1    -1  ]   then  [ 0  1  0 |   0    1   -1  ]
[ 0   0   1 | -1/3  -1    5/3 ]         [ 0  0  1 | -1/3  -1   5/3 ]
```

The left half is the identity. The right half is the inverse:

```
Q^-1 = [[ 2/3,  0, -1/3],
        [  0,   1,  -1 ],
        [-1/3, -1,  5/3]]
```

**Check it.** Let the inverse act on the cost vector. Row 1: (2/3)(15) -
(1/3)(21) = 10 - 7 = 3. Row 2: 26 - 21 = 5. Row 3: -5 - 26 + 35 = 4. The
prices from Section 3, with no substitution.

The lecture's NumPy check of the second claim, that Q^-1 Q is the identity,
looks wrong at first glance. The diagonal is ones, but some off-diagonal
entries are tiny numbers in scientific notation. In the lecture's run one of
them is -5 times 10^-17. The professor spells it out: a decimal point, a long
run of zeros, then a five at the end. It is zero, just very slightly not zero. His fix is to round
every entry to the tenth decimal place, after which the identity appears, with
a few entries displayed as minus zero, which he says not to worry about.

```python
Qinv = np.array([[2/3, 0, -1/3], [0, 1, -1], [-1/3, -1, 5/3]])
c = np.array([15, 26, 21])
print(Qinv.dot(c)) ## expect the prices
print(Qinv.dot(Q)) ## nearly the identity
print(np.round(Qinv.dot(Q), 10)) ## the identity
```

The first print gives `[3. 5. 4.]`. The second gives ones on the diagonal,
zeros in most spots, and one entry of about 2.2 times 10^-16 in the
bottom-left corner. The third gives the clean identity. Which entries pick up
the noise depends on the exact numbers and the order the additions happen in.
Section 8 explains where the noise comes from. The practical rule (added):
when you expect zeros from floating point arithmetic, round or compare with a
tolerance. Never test a float against exactly zero.

## 6. Three ways backsolving breaks

Backsolving gives exact solutions, but the professor is direct that it only
works for specific types of matrices. He covers three restrictions.

**Peril one: fewer equations than unknowns.** Ignore the last equation. Two
equations, three unknowns. Follow the same steps and, where Section 3 reached
a row holding only p3, there is now nothing in the third row. You are stuck
holding those two equations. Instead of one solution there are infinitely
many, because the remaining equation defines a plane in three-dimensional
space (Module 1 showed an example) and any point on that plane is a
potential solution to that equation. The rule: you must have at least as many
equations as unknowns.

For the coffee shop, drop Faye. Dana and Eli alone are satisfied by p =
[3, 5, 4], and also by p = [2, 2, 9] (check: 4 + 2 + 9 = 15 and 2 + 6 + 18 =
26). Two receipts cannot tell those price lists apart. In the sheet picture
from Section 2 (added): three sheets meet at a point, two sheets meet along a
whole line, and every point on that line is a candidate.

**Peril two: an equation that repeats another.** Three equations is
necessary but not enough. If one equation is a **linear combination** of the
others (built by scaling other rows and adding them, added definition) it
carries no new information. The professor's case is a third row that is
exactly double the second, with the receipt doubled as well. Three rows,
three unknowns, and the elimination still stalls: following the exact steps
from Section 5, you eventually reach a third row with no one left in the
third position. In his words, the p3 solution has disappeared. That outcome
means Q is **not invertible**. Q^-1 does not exist.

Replace Faye's row with two of Eli's, receipt 52:

```
S = [[2, 1, 1],       c = [15, 26, 52]
     [1, 3, 2],
     [2, 6, 4]]
```

After clearing the first column, row 3 is [0, 5, 3 | -1, 0, 1], exactly twice
row 2's left half [0, 5/2, 3/2]. Subtracting 2 R2 wipes row 3's left half to
zeros. No pivot for p3. Why this is really peril one in disguise (added): the
second row already says Eli paid 26 for his order. A third row saying two of
Eli's order costs 52 is the same fact in a bigger coat. Two independent facts
about three prices, with a row count that says three, so nothing looks wrong
until the arithmetic stalls.

**The determinant is the test.** You can catch peril two without running the
elimination. Compute the **determinant** of the matrix. For the doubled-row
matrix it is zero. Per the lecture, the determinant measures the volume of
the three-dimensional parallelogram (the slanted box) produced when the
matrix acts on the unit cube, the 3D version of the area-of-a-parallelogram
picture from earlier in the course. Zero volume means the box has been
flattened (added). Flat is fatal: multiple points on the unit cube land
on the same point of the flattened shape, and an inverse, if it existed,
could not know which starting point to send a given output back to. So it
does not exist. The rule: check invertibility by calculating the determinant
and confirming it is non-zero.

The determinant of the coffee matrix Q is 3 (added). That is why every
fraction in Q^-1 is in thirds. Divide by 3 and you get fractions. Divide by
zero and you get nothing. Same fact, seen from the arithmetic side.

**The rescue: more equations than unknowns.** Keep the doubled row but bring
the original third equation back as a fourth. Four equations, three
unknowns, at least three of them independent. The duplicate row is still no
help for p3, but the fourth row still has a one in the third position, so p3
can be solved, and the elimination finishes. Extra equations do not hurt as
long as enough of them are independent. A redundant row just rides along.

```
R = [[2, 1, 1],       c = [15, 26, 52, 21]
     [1, 3, 2],
     [2, 6, 4],
     [1, 2, 2]]
```

Three independent rows survive and the unique solution [3, 5, 4] comes back.
The count of independent rows is the **rank** of the matrix, a term the
lecture does not use (added). Counting independent rows, not raw rows, is the
real test.

**Peril three: the single-price assumption.** The whole setup assumes each
item has one price that does not change from row to row. In the real world
the shoppers may be at different stores, and the same item's price varies
from store to store. The true price is then a distribution, not a number.
The professor's verdict: backsolving is too rigid for that scenario, and you
need probabilistic approaches, which come later in the course. What too rigid
means in practice (added): backsolving demands one price vector that explains
every receipt exactly. With store-to-store variation, no single vector does,
and the arithmetic returns a wrong exact answer or no answer, with no hint
that the model was the problem. The later probabilistic tools trade "exactly"
for "best fit" and report how much they could not explain.

Here are the three cases in code (added). Nothing in this block is from the
lecture. It shows what the failure looks like when a library hits it.

```python
S = np.array([[2, 1, 1], [1, 3, 2], [2, 6, 4]]) ## row 3 = 2 x row 2

for name, M in (("Q", Q), ("S", S)):
    print(name, "det =", round(np.linalg.det(M), 10))
    try:
        print(np.linalg.inv(M))
    except np.linalg.LinAlgError as ex:
        print("no inverse:", ex) ## Singular matrix

R = np.array([[2, 1, 1], [1, 3, 2], [2, 6, 4], [1, 2, 2]])
c4 = np.array([15, 26, 52, 21])
print(np.linalg.lstsq(R, c4, rcond=None)[0].round(10)) ## [3. 5. 4.]
```

Q reports `det = 3.0` and prints its inverse. S reports `det = 0.0` and the
inverse call raises `LinAlgError: Singular matrix`, the library's way of
saying "no pivot for p3." The four-row rescue is not square, so `inv` and
`solve` (next section) will not take it, but NumPy's least-squares solver
returns the exact prices from the three independent rows. Guarding the
inverse with try/except is the habit to keep: a singular matrix in
production data is a data problem, and you want the program to say so rather
than crash.

## 7. The same work in NumPy

The professor's stance: NumPy is the tool you actually reach for when a real
problem needs an inverse. It is very optimized and one of the fastest
solutions available. Everything from Sections 3 through 5 is a few lines.

```python
c = Q @ p ## same as np.dot(Q, p)
Qinv = np.linalg.inv(Q)
print(Qinv)
print(np.round(Qinv @ Q, 10)) ## identity, after rounding
print(Qinv @ c) ## prices back
print(np.linalg.solve(Q, c)) ## same answer, no Qinv in hand
```

Three things from the lecture in that block. The `@` symbol is shorthand for
the NumPy dot product, and `Q @ p` and `np.dot(Q, p)` are equivalent. The
`inv` function in NumPy's linear algebra library takes a square matrix and
returns its inverse, and `Qinv @ Q` shows the same near-identity with tiny
off-diagonal floats that rounding to ten decimal places removes. And `solve`
is the more abstract route: hand it the matrix and the cost vector, ask for
the prices, and it returns them without you ever holding the inverse. The
lecture says that behind the scenes solve is doing exactly the same thing,
calculating the inverse and dotting it with the cost vector.

Output of the inverse:

```
[[ 0.66666667  0.         -0.33333333]
 [ 0.          1.         -1.        ]
 [-0.33333333 -1.          1.66666667]]
```

Both `Qinv @ c` and `solve` print `[3. 5. 4.]`. The trailing points mean the
answers came back as floats (added).

When to use which (added): `solve` when you only want the answer. `inv` when
you need the inverse itself, to apply it to several cost vectors or to look
at it.

The catch the professor raises: those decimals are hard to read. The inverse
is correct, and equivalent to the fractions from Section 5, but working out
which concise fraction 0.66666667 stands for takes effort, and 1.66666667
takes more. Put the two side by side and the fraction form tells you at a
glance that the corners are thirds. The decimal form makes you guess (added).

## 8. Why the decimals happen: how a computer holds a number

To motivate the fix, the lecture asks how NumPy and Python represent numbers
at all. Writing two integers as a = 13 and b = 6 is human readable, but it is
not what the computer holds. The computer represents each number in base 2
as a **bit string**, a sequence of ones and zeros where each position stands
for a power of two, starting at 2 to the 0, then 2 to the 1, 2 to the 2, and
so on. The ones and zeros say whether that power of two is included in the
sum that rebuilds the number.

```python
from bitstring import Bits
a, b = 13, 6
for n in (a, b, a + b):
    print(n, Bits(uint=n, length=8).bin) ## base 2, 8 bits
```

```
13 00001101
6 00000110
19 00010011
```

In this printout the rightmost bit is the 2 to the 0 place and the powers
grow to the left (added). Reading 13: a one in the ones place, a zero in the
twos, a one in the fours, a one in the eights. 8 + 4 + 1 = 13. Reading 6: 4 +
2.

Adding a and b, the computer adds the bit strings bitwise, column by column
from the 2 to the 0 place. The rule the lecture states: every position must
be a one or a zero, so when two ones add to 2, that is too large for one
column. Write a zero and carry a one into the next column to the left.
Walking 13 + 6: ones column 1 + 0 = 1. Twos column 0 + 1 = 1. Fours column 1 +
1 = 2, write 0, carry 1. Eights column 1 + 0 + the carry = 2, write 0, carry
1. Sixteens column 0 + 0 + the carry = 1. Result 00010011 = 16 + 2 + 1 = 19.

The professor is explicit that you do not need to understand binary
representation or binary arithmetic for this course. It is shown only to
explain what Python is doing underneath. He also provides extra Python code
that mirrors the CPU's bit addition for anyone who wants the detail.

Here is why it matters to the story anyway (added). A fraction like 1/3 has
no finite base 2 form, so the moment it is stored as bits it is already an
approximation, off by about one part in 10^16. Every decimal in the NumPy
inverse inherits that, and when two such approximations should cancel to
zero they cancel to 10^-16 instead. That is the noise in Section 5.

**SymPy** stands for symbolic Python. It does **symbolic arithmetic**, which
the professor describes as doing math in a human readable way, keeping
fractions as fractions instead of calculating them out to decimals. The
course will not explain how symbolic arithmetic works internally. The goal is
to know the tool exists and how to use it for readable results.

```python
from sympy import Rational
a, b = 1/3, 1/4
print(a + b) ## plain Python float
x, y = Rational(1, 3), Rational(1, 4)
print(x + y) ## sympy keeps the fraction
```

```
0.5833333333333333
7/12
```

The two answers are equal in value. But looking at 0.5833333333333333 it is
not immediately clear that this is the much simpler fraction 7/12. Symbolic
arithmetic keeps the simple form all the way through.

## 9. The professor's elimination function, and the closed form

A peek under the hood of Section 5. The professor did not grind the
augmented-matrix steps by hand. He wrote a function, **symbolic Gaussian
elimination**, that follows the Section 5 algorithm on a SymPy matrix and
prints the matrix at every step. As the lecture describes it: take a square
matrix M and read its dimensions. Build the augmented matrix with the
identity on the right. Go row by row, normalizing the i-th component of the
i-th row to one and subtracting multiples of that row from the rows below,
until the bottom left is zeros and the diagonal is ones. That forward sweep
is one for loop. Then go back up, taking each row from the bottom and
subtracting multiples of it from the rows above. Print as you go. When it
finishes, the left is the identity and the right is the inverse, in
fractions, the same numbers NumPy produced in decimals. Run on Q, its trace
matches the sequence of matrices in Section 5 (added).

Then the professor's turn: since the inverse is a fixed sequence of steps,
you could write the whole thing out once as an equation and reuse it. SymPy
makes that easy. Build a matrix of symbols instead of numbers and ask for its
inverse.

```python
from sympy import symbols, Matrix, lambdify
a11, a12, a21, a22 = symbols("a11 a12 a21 a22")
A = Matrix([[a11, a12], [a21, a22]])
Ainv = A.inv()
print(Ainv)
```

```
Matrix([[a22/(a11*a22 - a12*a21), -a12/(a11*a22 - a12*a21)],
        [-a21/(a11*a22 - a12*a21), a11/(a11*a22 - a12*a21)]])
```

Read it as one shared denominator, a11*a22 - a12*a21, over a rearranged
matrix: swap the diagonal entries, negate the off-diagonal ones. The
professor's take: for two-by-two these equations are not so crazy. Fill in
the four numbers and out comes the inverse. He defines a function that takes
values for the four symbols and evaluates the expression:

```python
f = lambdify((a11, a12, a21, a22), Ainv) ## plug numbers in
print(f(4, 7, 2, 6))
print(Matrix([[4, 7], [2, 6]]).inv()) ## exact fractions
```

```
[[ 0.6 -0.7]
 [-0.2  0.4]]
Matrix([[3/5, -7/10], [-1/5, 2/5]])
```

By hand for [[4, 7], [2, 6]]: the denominator is 4(6) - 7(2) = 10. Swap the
diagonal (6 and 4), negate the off-diagonal (-7 and -2), divide by 10. Same
matrix as both printouts, once as decimals and once exact. That shared
denominator is the determinant from Section 6 (added), and here is where the
closed form earns its keep. The professor says that once you see it, you can
start to understand why relationships between rows cause problems: you would
end up with a zero in one of these denominators. Every entry of the inverse
divides by a11*a22 - a12*a21. If one row is a multiple of another, that
quantity is zero, all four entries are division by zero, and no inverse
exists. [[1, 2], [2, 4]] gives 1(4) - 2(2) = 0 (added). That is peril two,
written as a formula.

The same approach gets complicated quickly. The professor builds a symbolic
three-by-three matrix and asks for its inverse. It takes a second longer, and
the equations get really large really quickly. It becomes confusing what you
have done. But his closing point stands: those large expressions are just the
Section 5 steps written out as one formula instead of executed on specific
numbers. For scale (added): the printed symbolic 3x3 inverse runs to roughly
960 characters, and every entry divides by a six-term denominator, the 3x3
determinant, against the 2x2's two terms. Closed forms are for
understanding. For anything bigger than 2x2, let the algorithm run the
steps.

## 10. Where it shows up: gravitational lensing

The module ends with a real place matrix inversion does work. The professor
notes there are a bunch of real-world examples and picks one he finds really
interesting: figuring out where galaxies actually are when gravity has warped
their light on the way to us.

The science is based on general relativity, in which very massive objects
(the sun, galaxies, black holes) bend space-time around them, and in doing so
bend the light emitted from far-off galaxies as it passes. See the telescope
image at the start of Lecture 5, taken by the Hubble or the James Webb space
telescope. The lesson it carries: the bright object in the middle is a
supermassive thing, probably a galaxy or black hole. The ring of light around
it is not part of that object. It is light from another normal-looking galaxy
behind the massive object, wrapped around the outside of it on its way to
Earth. The question is how physicists correct for the lensing to find where
that loop of light is actually coming from.

In its simplest form the system is represented with a **singular isothermal
sphere model**, which the professor calls a simplistic model for how light is
lensed when gravity is involved. Its pieces are the observed locations of
light and a matrix A that represents the effect of the massive object on the
source of light. In the simple model A is two-by-two and represents two
things: how massive the black hole is, and how much **tidal force** it
creates, where tidal forces are basically twisting the light coming from
behind it. Why two-by-two (added): a position on the sky is an x and a y, so a
matrix that maps positions to positions is 2x2.

Now the coffee shop with the labels swapped (added). There, a known quantity
matrix acted on unknown prices to produce observed receipts. Here, a lens
matrix acts on unknown true positions to produce observed positions:
observed = A times true. If you can estimate some details about the black
hole, such as how big it is, you can build A and calculate its inverse. Then
act on the observed light with A^-1 and you back out where the light is
actually coming from. Same one line as Section 4: true = A^-1 observed. A
wrong estimate of the black hole gives a wrong A, a wrong inverse, and a
reconstructed galaxy in the wrong place. The physics estimate and the linear
algebra are two halves of one job (added).

The professor walks through code from a GitHub library, not required for the
class, that represents light from a galaxy as an object with adjustable size,
position relative to your vantage point, oblongness, and spin. A black hole is
imagined between you and the galaxy. Most of the calculation detail is
skipped, but the core is that two of the steps are just matrix dot products.
The library's rotate function calculates a rotation matrix, seen in previous
videos, and uses it to act on the original x, y points. It does this twice:
once based on the size of the black hole, then again based on the tidal
forces. In the interactive demo, moving the black hole around visibly warps
the galaxy's light, growing it has a more and more dramatic effect until the
result looks like the telescope ring, and adding tidal force spins the light.
Every one of those effects is captured by the two dot products. Take the
physics of the gravity well, represent it as rotation matrices, let them act
on the points of light, and you can simulate the telescope image.

A small worked picture of the shape of the calculation (added). This is an
illustration, not the singular isothermal sphere model. A **rotation matrix**
[[cos t, -sin t], [sin t, cos t]] turns a point through angle t, and a toy
lens L plays the role of A:

```python
def rotate(deg, xy): ## rotation matrix acting on a point
    t = np.radians(deg)
    R = np.array([[np.cos(t), -np.sin(t)], [np.sin(t), np.cos(t)]])
    return R @ xy

print(np.round(rotate(90, np.array([1, 0])), 6)) ## (1,0) -> (0,1)

L = np.array([[2, 0], [0, 0.5]]) ## toy lens, stretch x, squash y
src = np.array([1, 2])
obs = L @ src
print(obs) ## what the telescope sees
print(np.linalg.inv(L) @ obs) ## back to the source
```

```
[0. 1.]
[2. 1.]
[1. 2.]
```

The rotation moved (1, 0) to (0, 1). The lens moved the source (1, 2) to the
observed (2, 1). Inverting the lens and applying it to the observation
returned (1, 2). That last line is the whole lecture in one operation: known
observation, known lens, inverse recovers the truth.

## 11. The thread

Every section of this module is one idea wearing different clothes. A known
matrix acts on an unknown vector to produce a known vector. Substitution
solves it once. Gaussian elimination is substitution with a name and a fixed
set of moves, and running those moves against an attached identity matrix
records them as the inverse, which solves it for every right-hand side. The
inverse exists only when the matrix has as many independent rows as
unknowns, which the determinant tests in one number, and it assumes one true
value per unknown, which real data does not always grant. NumPy runs the
moves fast in decimals that carry binary noise. SymPy runs them exactly in
fractions and can even write the moves out as a formula, whose shared
denominator is the determinant and whose zero is the failure. And in the sky,
a 2x2 matrix built from a black hole's mass and tidal force turns a galaxy
into a ring, and its inverse turns the ring back into the galaxy.

Same matrix, same inverse, same one line: unknown = inverse times observed.
The rest of the module is learning when that line is allowed.
