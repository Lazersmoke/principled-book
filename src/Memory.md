# Memory

We expect programs to keep track of lots of different things for us.
For example, we expect calculators to "remember" what we have typed in so far, so it can keep doing math on it.
We also expect web browsers to "remember" which page we are on, and things like which bookmarks are in the bookmarks bar.
Unfortunately, computers aren't magical, so they can't just "remember" information; they must store that information somewhere, and then refer back to it later, when they need it.

## Binary

The way computers do this in practice is by using encoding the information they want to remember into binary.
Binary is simply a sequence of a bunch of $1$s and $0$s, *on*'s and *off*'s, *true*'s and *false*'s, etc.
Each individual $1$ or $0$ is called a *bit*, and they are said to be *set*, meaning *on*, or *unset*, meaning *off*.
It's also called base $2$ because it is the number system that uses exactly $2$ distinct digits.
In contrast, the system of "normal numbers" is called base $10$ because it uses exactly $10$ distinct digits: $0123456789$.
When it might be confusing which base we are talking about, since numbers like $101$ are present in both bases, we use a subscript like $101_2$ to mean "the base $2$ number $101$," or $10_{10}%_$ to mean "the base $10$ number $101$."
We usually write binary using $1$s and $0$s because it is easier than writing `onoffonononoff`.
Here is an example of some binary:

$$0011100110100010$$

Notice that this doesn't really mean anything to us as it is.
In order for storing some information in binary to make sense, there needs to be a way to convert between the information and its binary representation.
This specification for this conversion is called *an encoding of data in binary*, and actually performing the conversion is called *encoding* and *decoding*.
As a simple example, let's say we want to encode the user's favorite color in binary, out of these options: Red, Blue, Yellow, or Green.

It's pretty simple to creating an encoding that will work for these options.
We just have to assign an arbitrary sequence of 1s and 0s to each option:

Color    Binary
------- --------
Red      $00$
Blue     $01$
Yellow   $10$
Green    $11$

Now that we have an encoding, we can interpret the above binary sequence as a bunch of colors smooshed together in a row.
We can break $0011100110100010$ into $00$, $11$, $10$, $01$, $10$, $10$, $00$, $10$.
Each of those corresponds to a color in our table, so we can convert them: `Red`, `Green`, `Yellow`, `Blue`, `Yellow`, `Yellow`, `Red`, `Yellow`.

There's nothing particularly special about our encoding for colors.
We could have just as easily used this encoding of base $10$ numbers in binary:

 Base $10$   Base $2$
----------- ----------
$0$         $0000$
$1$         $0001$
$2$         $0010$
$3$         $0011$
$4$         $0100$
...         ...
$14$        $1110$
$15$        $1111$

This encoding is actually worth memorizing, since it is the standard on between these bases.
The easy way to do this is to notice that the first base $2$ digit corresponds to $1$, the second corresponds to $2$, and so on using powers of $2$.
For example, $1010_2$ becomes $(1 \cdot 2^3) + (0 \cdot 2^2) + (1 \cdot 2^1) + (0 \cdot 2^0) = 10_{10}%_$.

We can interpret the above binary sequence as a bunch of $4$-bit base $10$ numbers (using the encoding from the table) smooshed together in a row.
First, we split $0011100110100010$ into $0011$, $1001$, $1010$, and $0010$.
Now we can use our encoding to convert each of these into base $10$:
\begin{align}
  0011_2&=(0 \cdot 2^3) + (0 \cdot 2^2) + (1 \cdot 2^1) + (1 \cdot 2^0) = 3_{10}\\%_
  1001_2&=(1 \cdot 2^3) + (0 \cdot 2^2) + (0 \cdot 2^1) + (1 \cdot 2^0) = 9_{10}\\%_
  1010_2&=(1 \cdot 2^3) + (0 \cdot 2^2) + (1 \cdot 2^1) + (0 \cdot 2^0) = 10_{10}\\%_
  0010_2&=(0 \cdot 2^3) + (0 \cdot 2^2) + (1 \cdot 2^1) + (0 \cdot 2^0) = 2_{10}%_
\end{align}

Note that we usually write binary with all the leading $0$s intact.
This is because the length of the number is indicative of how much storage space we need to write that number in memory.

- Processor Registers/Cache
- Random Access Memory (RAM) 
- Disk Storage (Hard drive, SSD, flash drive, or similar)
