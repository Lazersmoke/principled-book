# Reusability

The reason we write code is to reduce the overall effort we expend on getting things done.
Instead of manually working each digit of $\pi$, we simply write some code to crunch the numbers for us, press a button, and $3.1415 \dots$ is printed out.
The reason this is helpful in the first place is that we didn't have to write a different piece of code to compute each digit of $\pi$; we just had to write a single algorithm (probably some sort of [spigot algorithm](https://en.wikipedia.org/wiki/Spigot_algorithm), in this case) and it worked for every digit of $\pi$.
In other words, we reduced a problem where we needed to write down $O(n)$ things (digits of $\pi$) to a problem where we needed to write down $O(1)$ things (algorithms to compute $\pi$).

This is the essence of the idea of *reusable code*.
We want to reduce the effort that users of the code must expend to make use of it.
This sounds simple, but there are actually many different ways to expend effort:

- If we write a library, the effort for other developers to write code using our library.
- If we are writing software meant for general consumption, the effort for users to put our software to productive use.
- If we are writing any code at all, the effort for us to maintain that code.
- If we are writing code that might eventually be published literally anywhere, the effort for other developers to understand what it is doing.
- If we want our code to be useful in more than a month, the effort for other people to maintain and extend that code.

These are all goals that are achievable with foresight, but difficult to apply in retrospect.
While you are in the process of writing code, it is very fresh in their mind, so you can explain exactly what is going on with ease.
If you wait until you are done with a large chunk of the work, you have already started to forget the details of whatever it was you wrote first.
In this way, it is actually easier to do things right the first time through, in terms of total effort required.

Note also that the solution to writing reusable code isn't always to write better quality code.
Writing useful and relevant documentation to clarify complex parts of the code is oftentimes just as important as writing good code in the first place.
This doesn't apply just to comments in code, either.
End users should *always* be a primary consideration, and it's important to keep in mind that they need documentation too.
Even the best software is useless without a good User's Guide.

## What non-reusable code looks like

That was all a bit wishy-washy, so let's look at something concrete.
Here's some code that would make me reconsider my decision to wake up in the morning:

```python
# Code shamelessly poached from https://stackoverflow.com/a/46614483
def count_even_chars(s, low, high):
  if low >= high:
    return 0
  elif s[low].islower():
    return 1 + count_even_chars(s, low + 1, high)
  else:
    return count_even_chars(s, low + 1, high)

def even_or_odd_lowercase(s, low, high):
  return count_even_chars(s, low, high) % 2 == 0
```

Let's make a first pass to apply the traditional "comment everything" advice to this snippet, and see if that helps to improve the overall code quality.
Remember, we are trying to help other people understand what is going on in this method, so adding lots of comments should always help, right?

```python
# Counts the number of lowercase characters in a string
def count_even_chars(s, low, high):
  # If the low index has passed the high index
  if low >= high:
    # Then return 0
    return 0
  # Otherwise, if the string at the low index is lowercase
  elif s[low].islower():
    # Then return one plus the number of lowercase 
    # characters in the remaining string
    return 1 + count_even_chars(s, low + 1, high)
  # Otherwise, if it wasn't lowercase
  else:
    # Return the number of lowercase
    # characters in the remaining string
    return count_even_chars(s, low + 1, high)

# Checks if the number of lowercase characters is even or odd
def even_or_odd_lowercase(s, low, high):
  # Use % 2 to check for even or odd, and return the result
  return count_even_chars(s, low, high) % 2 == 0
```

Well, we've certainly added a lot of volume to the code.
However, I'd argue that this code is *less maintainable* after these comments were added.
Merely reiterating what the code already says is completely useless to everyone; the reader has to dig through more verbose repetitions, the writer has to write more verbose repetitions, and the compiler has to discard more verbose repetitions.

If comments inane comment spam can't get us out of the woods, what can we try next?
The main issue with the original code is that it uses an unconventional way to recurse.
It carries around the indexes `low` and `high`, in addition to doing the logic of checking if the characters are lowercase.
This gives us two options:

- Write a short paragraph on top of our function explaining what these mysterious arguments do, and why they are needed.
- Use a conventional convention.

We'll do the second one, obviously. 
The usual way for iterating over a string is Python is a `for ... in` loop instead of recursion, so we'll use that. (Yes, I know the original Stack Overflow question required recursion, but it is just not pythonic)
We will also leave in the function header comments, since those are useful regardless of anything else.
They allow other developers to read a "summary" of the function without digging through the code.
They also provide a "reference implementation" that other developers can compare with the code of the function if they suspect it contains a bug, so they can determine if it is intended behavior or not.
Lastly, we'll rename `count_even_chars` to a name that is at least technically correct, and rename `even_or_odd_lowercase` to something more descriptive.
To the code:

```python
# Counts the number of lowercase characters
def count_lowercase_chars(s):
  count = 0
  for c in s:
    if c.islower():
      count += 1
  return count

# Checks if the number of lowercase characters in a string is even
def is_number_of_lowercase_even(s):
  return count_lowercase_chars(s) % 2 == 0
```

Wow!
That looks much better.
By adopting a standard convention, we have simultaneously reduced the length, complexity, and obfuscation over our code.
Notice especially that users would have to call the other code as `count_even_chars("asdf",0,3)`, with ugly numbers in the call.
Now, they need only say `count_lowercase_chars("asdf")`, and their bidding will be done.

We aren't done yet, though.
This code is woefully monomorphic; it counts a very specific property of a very specific item (characters) in a very specific structure (a string).
This just screams for generalization; we can make this function work on any property of any item of any (iterable) structure very easily:

```python
# Counts the number of items satisfying a specific property
def count_satisfy(prop,xs):
  count = 0
  for x in xs:
    if prop(x):
      count += 1
  return count

# Checks if the number of lowercase characters in a string is even
def is_number_of_lowercase_even(s):
  return count_satisfy(lambda c: c.islower(),s) % 2 == 0
```

This may seem like we've shifted more work onto the end user, (they have to write a lambda now) but it turns out to be for the greater good!
Look what we can write now:

```python
count_satisfy(lambda report: report.street == "Dank Ave" && report.house_number == 123,policeBlotter)
```

We just got a *very* generic function for (very close to) free simply by generalizing a little bit.
As a library author, we could not have foreseen the need to count occurrences of an address in a police blotter, but someone was able to use our function to solve their problem anyway.
Neat!

To summarize: We've transformed a snippet of convoluted, hard to follow code into a simple 5-line function requiring almost no explanation by simply using pre-existing idioms and best practices.
We didn't need a line or two of comments for every line of code because our code was clear and plainly stated..
