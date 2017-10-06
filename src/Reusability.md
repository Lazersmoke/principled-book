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

Note also that making the solution to writing reusable code isn't always to writer better code.
Writing useful and relevant documentation to clarify complex parts of the code is oftentimes just as important as writing good code.
This doesn't apply just to comments in code, either.
End users should *always* be a primary consideration, and it's important to keep in mind that they need documentation too.
Even the best software is useless without a good User's Guide.

## What non-reusable code looks like

That was all a bit wishy-washy, so let's look at something concrete.
Here's some code that would make me reconsider my decision to wake up in the morning:
