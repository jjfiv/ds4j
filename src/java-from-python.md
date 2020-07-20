+++
title = "Java from Python"
weight = 2
[extra]
math = true
show_toc = true
+++

When students first come to Java from Python, they are usually a little overwhelmed. There's so much more to type to get anything done!

One day, the benefits of learning Java will catch up to you. Some programmers never quite forgive Java for their instruction in it or the sometimes large amounts of code they must write to explain simple things to the computer.

However, I promise you: someday you will return to code that you (or someone else) have written late at night for a deadline. You may not have seen this code in months or even years. On that day, languages that are more explicit, like Java are going to be easier to read than languages like Python. Until then, come with me on this journey into being more specific.

## Java requires classes and objects

In Python, we could just drop code into a ``.py`` file and get going. Any statements we wrote would be executed when we run the file or import it. Maybe later in our classes we were taught to organize our code like the following:

```python
# in main.py:
def main():
   # do the real work here
   ...

if __name__ == '__main__':
  main()
```

In python code, you can develop methods called ``pineapple``, ``apple``, and ``orange`` and put them in the ``apple.py`` file. Nobody's stopping you, even though you should really put it in a file called ``fruit.py``.

Java is a little more strict. If we want to have a class called ``Pineapple``, it must be in a file called ``Pineapple.java``.

```java
// In Pineapple.java
public class Pineapple {
    public static void main(String[] args) {
        System.out.println("Pineapple is a fruit (I hope).");
    }
}
```

For now, we will just think of everything above as a template:

```java
// In MYCLASS.java
public class MYCLASS {
    public static void main(String[] args) {
        // Do MYCLASS stuff.
    }
}
```

Every time we run code, we want to wrap it in a template like this. Although not required, we always want to pick a name for ***MYCLASS*** that is capitalized -- this is a "convention" in Java, so whenever you see a capitalized word, you can be pretty sure it is a class and not another kind of variable or a method.

## Variables have types

In python, we were able to introduce variables at any point, with any name, and for any kind of value.

```python
x = 7
print(x)
x = "Seven"
print(x)
```

Java will get mad at us for trying to do the same sequence of statements: it won't let us confuse ourselves (or our readers!) by having multiple different kinds of values in the same variable. Because ``x`` is declared to have an integer (or ``int``) in it, we can only put other integers in it!

```java
// We need to put the type in front of our name:
int x = 7;
System.out.println(x);
x = "Seven"; // error: incompatible types: String cannot be converted to int.
System.out.println(x);
x = 8; // This will work!
System.out.println(x);
```

### Types go before variable names.

```java
// When creating classes:
VariableType variableName = new VariableType();
// When dealing with primitives, you often don't use the new keyword.
int intName = 7;
double doubleName = 3.5;
String message = "Hello World!";
```

## Numeric Types: ``int`` vs. ``double``

Going back to our example of ``int x``, there is a distinction between types of numbers that Java calls to our attention. 

```java
// We need to put the type in front of our name:
int x = 7;
System.out.println(x);
x = 8; // This will work!
System.out.println(x);
x = 9.1; // error: incompatible types: possible lossy conversion from double to int
System.out.println(x);
```

In python, we could seamlessly move between numbers with decimals, and those without, but we had to be careful with division. Java makes us be careful when we create the variable.

### So what's a ``double`` and what's an ``int``?

For a short answer, a ``double`` is where we store fractional numbers and those with fractional components, and it even supports exponential notation:``1.5, 0.0, 1e7, -2e3`` (this is the same as in Python).

An ``int`` or integer, on the other hand, only stores whole numbers: ``0, 1, 10, -57, 65000``. When you divide integer values together, the decimal remainder disappears and the number is rounded down:

```java
System.out.println(7/2); // you'd think: 3.5, but prints: 3
System.out.println(1/2); // you'd think: 0.5, but prints: 0
```

So if you care about fractions, use a ``double``. If you don't care, use an ``int``. We will mostly only see ``double`` values when we do graphics.


### Beware of fractional numbers and equality!

Never ever ever use equality on fractional numbers (``double`` variables). This is not an issue specific to Java, but it is possible you were never shown this in Python.

```python
if 0.1 * 3 == 0.3:
    print("Math works correctly!")
else:
    print("Math does not work!", 0.3, 0.1 * 3)

# Sadly, this prints: Math does not work 0.3 0.30000000000000004
```

The same problem exists in Java:

```java
if (0.1 * 3 == 0.3) {
    System.out.println("Math works correctly!");
} else {
    // Hmm: we use + to combine strings here in print, rather than a comma:
    System.out.println("Math does not work! 0.1 * 3 = " + (0.1 * 3));
}
```

In all cases the way to correct this check is to use math expressions. Instead of comparing \\(x = y\\) directly, do something like this:

$$ |x-y| < \epsilon $$

Where you need to pick a small number, e.g., \\(\epsilon = 0.0001\\) that represents the level of error you will tolerate.


### Exponents, powers, and Math

Switching from Python to Java has a few little traps. One of them is the power operator.

In python, you can raise a number to a power using the ``**`` operator. In both Python and Java, the ``^`` operator does not perform the power operation:

Power works in Python, caret does not.

```python
print(2 ** 8) # 256
print(2 ^ 8) # 10
```

Java doesn't have a power operator, but a static method that exists on the Math class:

```java
System.out.println(Math.pow(2, 8)); // 256.0
// This is not a power!
System.out.println(2 ^ 8); // 10
```

<details>
<summary>What's XOR?</summary>

Both Python and Java agree: ``2 ^ 8 == 10`` and that's not what we expected from calculators and math class. But what is it?

It turns out it is ***binary exclusive or*** or the XOR operator. To understand it's behavior we need to know our binary numbers:

| Decimal Number | Binary Number |
|----------------|---------------|
|  0 | 0000 |
|  1 | 0001 |
|  2 | 0010 |
|  3 | 0011 |
|  4 | 0100 |
|  5 | 0101 |
|  6 | 0110 |
|  7 | 0111 |
|  8 | 1000 |
|  9 | 1001 |
| 10 | 1010 |
| 11 | 1011 |
| 12 | 1100 |
| 13 | 1101 |
| 14 | 1110 |
| 15 | 1111 |
| 16 | 10000 |
| .. | .. |

```java
// In binary:
0b0010 ^ 0b1000 == 0b1010;
// In decimal:
2 ^ 8 == 10;
```

XOR looks at each "bit" set and creates a result where the bits are set if either of the inputs have a 1 but not both. (either-OR, but not both is why we call it ***eXclusive OR***)
</details>

There are other methods on Java's ``Math`` class that you may find handy, much like Python's ``math``. A full list can be found as on the [java.lang.Math JavaDoc](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)

```java
System.out.println(Math.abs(-7)); // prints: 7
System.out.println(Math.round(9.1)); // prints: 9
System.out.println(Math.round(9.5)); // prints: 10
System.out.println(Math.round(9.6)); // prints: 10
```

## Methods have input and output types

We used to call these functions, but since they must be in a class in Java, we call them ***methods***.

### Method syntax

In python, making a method that multiplies a number by seven is fairly short:
```python
def multiply_by_seven(x):
    return x * 7
```

Strangely, the above method works on all sorts of things:
```python
print(multiply_by_seven(3)) # prints: 21
print(multiply_by_seven("<3")) # prints: <3<3<3<3<3<3<3
print(multiply_by_seven(3.1)) # prints: 21.7
```

In Java, we must mark the return types, and each of those functions would have to be written separately.

```java
// For integers:
public static int multiplyBySevenA(int x) {
    return x * 7;
}
// For doubles:
public static double multiplyBySevenB(double x) {
    return x * 7.0;
}
// For strings:
public static String multiplyBySevenC(String input) {
    String output = "";
    for (int i=0; i<7; i++) {
        output += input;
    }
    return output;
}
```

Although it may seem tedious; you probably didn't expect that our ``multiply_by_seven`` could be used in 3 ways (at least!). Java makes you be more precise and explicit. You've got to *say what you mean* and *mean what you say*.


## Loops Review

There's a children's "game" that goes as follows:

> Pete and Repeat walk into a bar. Pete leaves. Who's left?
>
> Repeat.
>
> Pete and Repeat walk into a bar. Pete leaves. Who's left?
>
> ...

Everytime your poor friend says "Repeat", the instigator would tell the story again! We've seen while and for loops in Python (and in Java in the previous sections), but let's look at them a little closer.

### While Loops

We saw a while-loop in the ``GuessingGame`` example, and these are typically used to construct infinite loops, or to loop until a condition is satisfied. They are very useful when asking for user input:

```java
int guess = 0;
while(guess != 13) {
    System.out.println("Type 13 to escape this loop: ");
    try {
        guess = scanner.nextInt();
    } catch (Exception err) {
        String whatYouSaid = scanner.nextLine().trim();
        System.out.println("Please enter a valid number! You said '" + 
                    whatYouSaid + "' but I don't understand.");
        // Continue takes us around the loop again.
        continue;
    }
}
System.out.println("Congratulations; you typed in the magic number!");
```

We can also use while loops to build the kinds of things we do in for-loops:

```java
int x = 0;
while (x < 10) {
    System.out.println(x);
    x += 1;
}
```
<div class="Think" markdown="1">

Does the the code snippet with the while loop above print 10?
</div>

### Numerical-For Loops

When we worked with Python, if we wanted to loop over a bunch of numbers, the ``range`` builtin was our friend:

```python
# forward, regular:
for i in range(10):
  print(i) # 0,1,2,3,4,5,6,7,8,9

# customize start=2 and step=3
for i in range(2,10,3):
  print(i) # 2,5,8

# go backwards:
for i in range(10,0,-1):
  print(i) # 10,9,8,7,6,5,4,3,2,1
```

Java gives us a for loop with three pieces: a start, a while, and a step. This is a parallel to the ``range`` we used to use in python:

```java
// forward, regular:
for (int i = 0; i < 10; i++) {
    System.out.println(i); // 0,1,2,3,4,5,6,7,8,9
}
// customize start=2, step=3:
for (int i = 2; i < 10; i += 3) {
    System.out.println(i); // 2,5,8
}
// go backwards:
for (int i = 10; i > 0; i -= 1) {
    System.out.println(i); // 10,9,8,7,6,5,4,3,2,1
}
```

### Collection-For Loops

Python has another kind of for-loop: the collection for-loop.

```python
for x in [1,7,4,2,1]:
    print(x)
```

And so does Java. We will talk about Java's collections in much more detail later, but:

```java
// import: java.util.Arrays so we can quickly make a list:
for (int x : Arrays.asList(1,7,4,2,1)) {
    System.out.println(x)
}
```

Instead of the keyword ``in`` that python used, Java uses the ``:`` and still requires a type for our loop variable (``x`` here).

## If-Statements and Decisions

We need to be able to make decisions based on user input, the output of computations or really anything else that comes up in a program. With enough if-statements, you can call your program artificial intelligence or AI.

```java
if (success) {
    System.out.println("Everything went as planned.");
} else {
    System.out.println("I've got some bad news.");
}
```

Things to notice: in Java, you need parentheses and the curly-braces in order to have if-statements.

### If, Else If, and Else

In python, we can nest else statements to handle a large number of values.

```python
if x == 1:
    print("One")
elif x == 2:
    print("Two")
elif x == 3:
    print("Three")
elif x == 4:
    print("Four")
else:
    print("Too big!")
```

Python has this weird ``elif`` keyword that's a [portmanteau](https://en.wiktionary.org/wiki/portmanteau_word#English) of ``else`` and ``if``. Java just uses the two words directly:

```java
if (x == 1) {
    System.out.println("One");
} else if (x == 2) {
    System.out.println("Two");
} else if (x == 3) {
    System.out.println("Three");
} else if (x == 4) {
    System.out.println("Four");
} else {
    System.out.println("Too big!");
}
```

Again, sprinkle parentheses and curly-braces to make it Java.

### Boolean Operators and Rules

Thinking about if statements tends to lead us to what can be said in if-statements. It turns out that there's a variable type: ``boolean`` that holds either ``true`` or ``false`` which is exactly like the ``True`` and ``False`` you would have seen in python.

In addition, there are operators just for boolean values: (NOT: ``!``), (AND: ``&&``), and (OR ``||``). If you've taken a logic class or seen these truth tables before, you know what's coming.

***Not*** provides us the opposite of a given value:``!true`` is the same as ``false``, and ``!false`` is the same as ``true``. Read the exclamation point as the word "not".

***And*** and ***or*** combine two boolean variables, and there are only four possible combinations, so we're just going to list them out:


| ``x`` | ``y`` | AND ``x && y`` | OR <code>x \|\| y</code> |
|----|----|----|----|
| ``false`` | ``false`` | ``false`` | ``false`` |
| ``true`` | ``false`` | ``false`` | ``true`` |
| ``false`` | ``true`` | ``false`` | ``true`` |
| ``true`` | ``true`` | ``true`` | ``true`` |


Think of AND as a pessimist: it's not happy unless both sides are ``true``. Think of OR as an optimist: as soon as it sees one ``true`` (in either position) it's pretty happy.

<div class="Think" markdown="1">

- What's ``(true || false) && false``?
- What's ``(true || false) || false``?
- What's ``!(true || false) || false``?
</div>

We can express numeric ranges as an AND. Legal guesses to our 1..100 guessing game (including 1, excluding 100) which we sometimes express in math class as:

$$ 1 \leq x < 100$$

Must be broken into two pieces in Java. All of the following statements are equivalent:

- ``guess >= 1 && guess < 100``
- ``guess > 0 && guess < 100``
- ``guess > 0 && guess <= 99``
- ``0 < guess && guess < 100``
- ``1 <= guess && guess < 100``

If we use an OR by accident, suddenly the statements are true no matter what value ``guess`` takes:

- ``guess >= 1 || guess < 100``

Just because (by virtue of ``guess`` actually being a number) it must be either greater than 1 (including 100, 101, 102, ...) or less than 100 (including 0, -1, -2, ...).

## Addition / Subtraction shortcuts

Something else you might have seen me do in Java is use a shortcut for addition or subtraction that isn't legal in Python.

In Python we can do either of the following to look up the value of ``x``, add 1 and store it back in ``x``:

```python
# full intention:
x = x + 1
# shortcut:
x += 1
```

It turns out, in Java (and C and a few others...), we can do one more shortcut that means the same thing:

```java
// full intention:
x = x + 1;
// shortcut:
x += 1;
// even shorter:
x++; // postfix
++x; // prefix
```

There are shortcuts for subtraction as well:


```java
// full intention:
x = x - 1;
// shortcut:
x -= 1;
// even shorter:
x--; // postfix
--x; // prefix
```

There are some subtle differences between prefix and postfix, but you need not worry about them in the official work of this class.

<details>
<summary>We can write a program to investigate...</summary>
<aside>

A Java program to distinguish between pre-and-post operations:
```java
// Start at any value:
int x = 7;

// Postfix uses the value before changing it:
System.out.println(x++); // prints: 7
System.out.println(x); // prints: 8
System.out.println(x--); // prints: 8
System.out.println(x); // prints: 7

// Prefix uses the value after changing it:
System.out.println(++x); // prints: 8
System.out.println(x); // prints: 8
System.out.println(--x); // prints: 7
System.out.println(x); // prints: 7
```

I consider these operators to be useful tricks: not to be preferred over comments and clear code. I use ``x++`` in for loops because it's shorter (``++x`` would work as well, too).

</aside>
</details>

## Exercises

- Write an absolute value method.
- Print lyrics to the "Happy Birthday" song.