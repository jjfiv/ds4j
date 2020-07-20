# Complexity

Before we get too far into the data structures section of our course, we need a new tool: we need a formal way of comparing the efficiency of different pieces of code. 

To say that another way, let's say we come up with two different ways to implement a list. Which one is better? How do we know? Better for which methods?

We call this new tool ***Complexity***. Another word for complexity might be ***difficulty***: how difficult is a particular solution. Note that we're entirely uninterested in how difficult it was for us humans, we only care about our computer. How difficult is it for the computer to run our code?

## Add Five to a Number

Let's say we have to write a method that adds five to its input and returns the output. We come up with three solutions to this problem: ``addFiveA``, ``addFiveB``, and ``addFiveC``. Take a moment to read all of them.

```java
// This option is about as short as we can get it.
public static int addFiveA(int x) {
    return x + 5;
}
// What if we introduce a variable for output?
public static int addFiveB(int x) {
    int output = x + 5;
    return output;
}
// Recall that output++; is a shorthand for output = output + 1;
public static int addFiveC(int x) {
    int output = x;
    output++;
    output++;
    output++;
    output++;
    output++;
    return output;
}
```

<div class="Think">Which solution do you prefer? Why?</div>
<div class="Tangent">Why did we decide to make these methods static?</div>

### A Strategy: Count Lines of Code

One thing you might have thought of is that ``addFiveA`` is only 1 line of code: a return statement, whereas ``addFiveB`` takes two lines of code and ``addFiveC`` is 7 lines of code.

As a counter-argument to this strategy, consider the following re-writing of ``addFiveA`` and ``addFiveB``.

```java
public static int addFiveA2(int x) {
    // did you know you can have semicolons alone on a line in Java?
    ;
    return x
     + 
     5;
}
public static int addFiveC2(int x) {
    int output = x; output++; output++; output++; output++; output++; return output;
}
```

I just cheated your measure. Now ``addFiveA2`` is five lines of code and ``addFiveC2`` is only one. The computer has to do the same amount of work as the original ``addFiveA`` but now we have a comment, a blank line, and a ``return`` statement that's all spread out, and it uses five times as many lines as ``addFiveC2``.

Lines of code is not a robust measure of coding complexity. If you ever have a manager who requires you to write a certain number of lines of code each day; now you can explain why that's not a fair measure!

### Strategy: Count Semicolons

"Eureka!", you say. "Your evil tricks have led me to the solution. What if we count semicolons instead of lines. Better than that: what if we count non-empty statements."

This turns out to be a pretty good strategy. Now we can compare these methods together. You diligently create the following table.

| Method Name | Useful Semicolons|
| --- | --- |
| ``addFiveA`` | 1 |
| ``addFiveB`` | 2 |
| ``addFiveC`` | 7 |

Clearly, ``addFiveA`` is the best of these methods, because it has the fewest useful semicolons.

## Count the sum of the numbers from 1 to N

Suppose you want to count all the numbers from 1 to N, including N. You quickly code up the following ***iterative*** solution:

```java
public static int sumSequence1(int n) {
    int sum = 0;
    for (int i=1; i<=n; i++) {
        sum += i;
    }
    return sum;
}
```

<div class="Think">How do we count the semicolons in a for-loop?</div>
<div class="Tangent">Is there a recursive solution? Can you write it?</div>

This is a classical problem, as it turns out, for which there is an easier algorithm. This is asking for a [Triangular Number](https://en.wikipedia.org/wiki/Triangular_number), and so given any value ``n`` we can compute this by directly calcuating it, rather than summing the intermediate numbers.

<div class="Tangent">Triangular numbers will come up again later when we study sorting. Imagine an list algorithm that solves 1 item at a time in a list, until it gets to all \\(n\\) items in the list.</div>

```java
// Formula for triangular numbers: https://en.wikipedia.org/wiki/Triangular_number
public static int sumSequence2(int n) {
    return (n * (n - 1)) / 2;
}
// Again, with steps broken out.
public static int sumSequence3(int n) {
    int result = n * (n - 1)
    return result / 2;
}
```

Writing a quick main method lets us check that the behavior indeed appears to be the same:

```java
// 1 + 2 + 3 == 6
System.out.println("S1(3) = "+sumSequence(3));
System.out.println("S2(3) = "+sumSequence2(3));
// 1 + 2 + .. + 10 == 55
System.out.println("S1(10) = "+sumSequence(10));
System.out.println("S2(10) = "+sumSequence2(10));
```

This causes some problems for our notion of complexity. When we simulate the for-loop from ``sumSequence1`` in our head, we find out that it depends on ``n``, the size of the input.

```java
    // The for-loop from sumSequence1, but spread-out with labels.
    for (
        int i=1;  // A: the initializer
        i<=n;     // B: the loop-check
        i++       // C: the step-statement
        ) {
        sum += i; // D: the loop body
    }
```

We then track each of the loop pieces independently:
- The initializer: A, runs only once.
- The loop-check: B, runs \\(n+1\\) times. It runs before the loop, and then at every step through the loop.
- The step-statment: C, runs \\(n-1\\) times. It only runs when the loop-check returns true after the first time through the loop body.
- The body of the loop: D, runs \\(n\\) times.

Woah. That's a lot of work. There are two other semicolons in the original method, so that brings us to: \\( (n+1) + (n-1) + n + 2 \\) or \\( 3n + 2 \\)

| Method Name | Useful Semicolons|
| --- | --- |
| ``sumSequence1`` | \\( 3n + 2 \\) |
| ``sumSequence2`` | \\( 1 \\) |
| ``sumSequence3`` | \\( 2 \\) |

In this instance, no matter what value \\( n \\) takes, we should always prefer ``sumSequence2``. But this gives us a hint for comparisons that really matter: ``sumSequence1`` is dramatically worse than ``sumSequence2`` (from a complexity point-of-view) because it depends on \\( n \\), the input itself. ``sumSequence3`` is worse than ``sumSequence2``, but not significantly, when \\(n\\) is large.

## Big-O notation: What does significant mean?

The ***time complexity*** of a method or procedure is always defined in terms of the ***size of its input***, which is notated as \\(n\\). And it is always a function, e.g., \\(f(n) = 3n + 2\\) or \\(f(n) = n^3 + n\\).

You may have noticed that actually counting the semicolons in a particular example is quite difficult and tedious. Little stylistic differences, (e.g., do you create a helper variable) appear to matter (``sumSequence2`` vs. ``sumSequence3``).

For this reason, we typically use so-called [Big-O Notation](https://en.wikipedia.org/wiki/Big_O_notation) when we discuss complexity. It allows us to remove the insignificant differences from our notation. 

If your Calculus background is strong (you are comfortable reading limits), enjoy the formal definition section of Big-O from Wikipedia. If not, consider the following examples and high-level discussion of what Big-O let's us do.

\\(O(f(n)) = g(n)\\) where \\(g(n)\\) captures the behavior of \\(f(n)\\) for really big \\(n\\).

When powers compete, the biggest power wins, and you ignore coefficients.
- \\(O(n^3 + n^2 + 7) = O(n^3)\\)
- \\(O(n^3 + 9000n^2) = O(n^3)\\)
- \\(O(n^3 + 9000n^{3.14}) = O(n^{3.14})\\)

It may help to remember that numbers, like \\(7\\) are equivalent to \\(7n^0\\), and so they have the lowest power, usually. You don't often see negative powers: \\(O(n^{-k})\\) ; that would be code that takes less time as the input gets bigger, which isn't very common.

Some functions you have seen before have an ordering, bigger on the left, smaller on the right.
$$O(n!) > O(2^n) > O(n^k) > O(n^2) > O(n\log(n)) > O(n) > O(\log(n)) > O(1)$$

<div class="Tangent">When we get back to sorting, we will consider triangle numbers and assign big-O notation to them.</div>

### What about calling methods?

We have defined complexity by counting semicolons, but what happens when you call another method?

<div class="Answer">Nothing changes: the complexity of a method hidden by a call is still part of the behavior of the method calling it -- those semicolons/statements are still used.</div>

### What about sometimes?

Assuming random number generation is \\(O(1)\\), what happens if sometimes we use a good algorithm and sometimes we use a bad algorithm?

```java
public static int sumSequenceMaybe(int n) {
    // 50% of the time, use the closed form, else use the loop.
    if (ThreadLocalRandom.current().nextBoolean()) {
        return (n * (n - 1)) / 2;
    } else {
        int sum = 0;
        for (int i=1; i<=n; i++) {
            sum += i;
        }
        return sum;
    }
}
```

50% of the time, we have an \\(O(1)\\) solution to this problem, and 50% of the time we have an \\(O(n)\\) solution to this problem. Giving that information precisely is the clearest way to describe this method.

But, usually computer scientists are concerned with the worst-case scenario: we would mark this method as \\(O(n)\\). If you're a glass-half-empty kind of person, this makes you happy. If not, sometimes we also discuss best-case or average-case complexity. 

Big-O notation can refer to worst-case, best-case, or average-case complexity. If not stated, we will be discussing worst-case complexity, but everyone has their own standards that matter. When in doubt, just ask!

## Conclusion

Theoretical Complexity is a broad topic and is still an open a research area. We have described complexity as being "like counting semicolons" along with Big-O notation to provide a general tool to discuss which functions or methods are most efficient, but we have done so quite informally here.