# Functions

The word "function" means different things to different people.
Ask an algebra student what a function is, and they will say it is an real-valued expression like $f(x) = 7\sqrt{x} + 5$, where the input number $x$ gets square-rooted, multiplied by $7$, and then has $5$ added to it.
To a front-end web developer, a function looks like this:

```javascript
function doStuff(x){
  console.log("I'm doing stuff!");
  return x + 30;
}
```

This function prints a message to the console, then adds $30$ to the input value, then returns it to you.
Set theorists (mathematicians who study sets) have their own definition of functions, which you can skip if that sounds useless to you.

## Technical Digression: Set Theoretic Functions

A relation $f : D \to C$ is a set $D$, (the domain) a set $C$, (the codomain, or range) and a subset $M \subset D \times C$ of the cartesian product of the domain and the codomain.
A function is a relation that associates every value from its domain to **exactly one** value in its codomain:

$$
  \forall x \in D\ldotp \exists f(x) \in C\ldotp (x,f(x)) \in M \land \not\exists y \in C\ldotp (y \neq f(x) \land (x,y) \in M)
$$

## Finding Common Ground

Those all sound like very different and contradictory definitions.
One reason this is so convoluted is that these definitions are all from different fields, so they use functions in different ways.
This doesn't mean that there is no hope for a unified understanding of what a function is, however.
We can still capitalize on the common elements between these different interpretations.

A function has some notion of *input*, and only works for some inputs.
The set of inputs that a function works for is called that function's *domain*.
The domain of the real-valued function $f(x) = 7\sqrt{x} + 5$ is the set of real positive real numbers, $\{x \ge 0\mid x \in \mathbb{R}\}$, since $\sqrt{x}$ is imaginary when $x < 0$.
The domain of the developer's function `doStuff` is "things that can have $30$ added to them," since adding $30$ to the input is one of the things that the function does.
It couldn't accept a kitten as input, for example, since you can't add $30$ to an adorable fur ball. (Trust me, I tried)

A function also has some notion of *output*, and it will only ever give certain values as output.
The set of outputs that a function can possibly give is called that function's *codomain* or *range*.
The codomain of $f(x)$ is real numbers greater than or equal to $5$, $\{x \ge 5\mid x \in \mathbb{R}\}$, since $\sqrt{x}$ is always positive.

The developer's function `doStuff` provides a sequence of *statements* to be carried out in order.
The first statement is a command to print `I'm doing stuff!`, and the second statement is to return $x + 30$.
From a theoretical perspective, `doStuff`'s output, or codomain, is this sequence of actions.
This is usually not particularly useful wording because developers are most often interested in the value that the function returns, $x + 30$.
For this reason, most developers would call $x + 30$ the output of `doStuff`, even though that is technically just the return value.
The *side-effects*, or the things that the function does other than computing and returning the output (in this case, printing `I'm doing stuff!`) are usually ignored since they are only relevant to the person writing the function, the the people calling the function.

What a function *does* is convert a given input value (from the domain) into an output value (from the codomain).
$f(x)$ does this by performing a series of mathematical operations on the input to arrive at an output value, while `doStuff` does this by constructing a sequence of statements based on the input value. Note that "running" or "calling" a function in a program means the side-effects are being run, and the return value is being computed, not that the sequence of statements is being constructed.

## Haskell's Platonic Functions

Compare these functions:

```haskell
plusFiveHaskell :: Int -> Int
plusFiveHaskell x = x + 5
```

```javascript
function plusFiveJS(x){
  return x + 5;
}
```

These two functions do fundamentally the same thing, namely add $5$ to their input.
These functions are both *pure*, meaning they don't do any side-effects, they just return a value.
In fact, Haskell specifies that all functions must be pure, so just like the mathematical function $f(x) = x + 5$, they don't do anything other than compute a value and return it.
If we wanted to implement `doStuff` in Haskell, we must specify that it does input/output actions by adding `IO` to the type, to say that we are constructing a series of input/output statements instead of just computing a new `Int`:

```haskell
doStuff :: Int -> IO Int
doStuff x = do
  putStrLn "I'm still doing stuff, but this time it's in the type!"
  return (x + 30)
```

This added layer of assurance that functions will only perform side-effects if they explicitly say they do allows us to do some pretty cool things.
We could reasonably consider `plusFiveHaskell` to have a codomain of `Int` rather than of program statements, which makes reasoning about it much easier to do, since we can apply all our mathematical intuition (think $f(x)$) to figure out what they do.
Unfortunately, we have no such luck in JavaScript.
Even though *we* know `plusFiveJS` is pure, we don't have a way to tell that to JavaScript or to other developers, since all functions in JavaScript are assumed to be of the *impure*, side-effect producing variety.
We must therefore consider all JavaScript functions to have the mathematically unsatisfying codomain of "program statements."
