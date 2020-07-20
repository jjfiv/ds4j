# Using Java's Lists

This section will go over a few core methods of a list. Our goal is not to deeply study Java's implementations of things, but the concept of a List. That general concept is applicable outside of programming Java or any particular programming language. We say that a list is an ***Abstract Data Type*** -- it's something that's existed before computers and Java and will exist as a tool for problem solving regardless of what programming language or paradigm you will study in the future.

The official documentation for Java's List is [available online](https://docs.oracle.com/javase/8/docs/api/java/util/List.html). If you don't have the link for a particular class, you can search for ``javadoc list`` online or access such pages from within your editor. If you code in Java, you will grow to rely on these documents for precise technical information.

Java's ``List`` provides a lot of functionality; not only is there an ``add`` method that takes a single item, but an ``addAll`` method that takes another ``List``. We can already write a simplified version of Java's ``addAll`` method, for any list type. 

```java
// for a List<Anything> or List<T>:
public void addAll(List<Anything> items) {
    for (Anything item : items) {
        this.add(item);
    }
}
```

So beyond Java's definition of what makes a class a ``List<T>`` which involves numerous other methods, there are a few operations that really define a what an abstract ``List`` can do, and you'll find them in any language with lists.


## What's an Anything or a T?

When we refer to the ``List`` abstract data type, particularly in java, we sometimes refer to it as a ``List<T>``. This means that it might be a ``List<Fish>`` a ``List<String>`` or any other particular class.

## Core Methods to Know

Adding, viewing, and removing items are core to what a list is, but the most important thing a list does is maintain a sequence of items in the order you put them in.

### int size()

To ask how many items are in a list, we can call the size method.

```java
List<String> abc = new ArrayList<>();
System.out.println(abc.size()); // prints: 0
abc.add("A");
System.out.println(abc.size()); // prints: 1
abc.add("B");
System.out.println(abc.size()); // prints: 2
abc.add("C");
System.out.println(abc.size()); // prints: 3
```

We have already seen that ``size`` is useful in loops.


### boolean add(T item)

A list isn't much use to us unless we can add things to it. Notice how we can't really use ``add`` without causing ``size`` to change. These two methods will always be related!

```java
List<String> abc = new ArrayList<>();
System.out.println(abc); // prints: []
abc.add("A");
System.out.println(abc); // prints: [A]
abc.add("B");
System.out.println(abc); // prints: [A,B]
abc.add("C");
System.out.println(abc); // prints: [A,B,C]
```

This method is configured to return a ``boolean`` -- that is, when you call ``add`` in Java a data structure has the opportunity to tell you it wasn't possible, and return ``false``. This is meaningful when we get to discussing ``Set`` datastructures.

### T get(int index)

Java, like Python and many other languages, indexes lists starting at zero and going up to the length of that list (not inclusively).

```java
List<String> abc = new ArrayList<>();
abc.add("A");
abc.add("B");
abc.add("C");
System.out.println(abc.get(0)); // prints: A
System.out.println(abc.get(1)); // prints: B
System.out.println(abc.get(2)); // prints: C
System.out.println(abc.get(3)); // crashes!
System.out.println(abc.get(-1)); // crashes!
```

When you go off the end of a ``List`` or an array in Java, an error will be thrown. You will see something like the following:

```
java.lang.IndexOutOfBoundsException thrown: Index 3 out-of-bounds for length 3
       at Preconditions.outOfBounds (Preconditions.java:64)
       at Preconditions.outOfBoundsCheckIndex (Preconditions.java:70)
       at Preconditions.checkIndex (Preconditions.java:248)
       at Objects.checkIndex (Objects.java:372)
       at ArrayList.get (ArrayList.java:440)
       at JavaListExperiments.main (JavaListExperiments.java:25)
```

Note that it tells you that ArrayList.get was the problem in a class called ``JavaListExperiments`` and that index 3 was out-of-bounds for length 3. When you get an error like this, look for class names you recognize (we won't be discussing ``Objects`` or ``Preconditions`` in this book), so ``ArrayList`` and ``JavaListExperiments`` are most relevant to our code, and then also read the message that it gives you. Thanks to this error, we know exactly what line caused our problem (it was line 25 for me!).

### boolean add(int index, T item)

There is another method named ``add`` on Java's list class, which supports adding an item to a particular position.

```java
// Set up our ABC list.
List<String> abc = new ArrayList<>();
abc.add("A");
abc.add("B"); 
abc.add("C");
System.out.println(abc); // prints: [A,B,C]

// Now start inserting in the middle.
abc.add(1, "Z");
System.out.println(abc); // prints: [A,Z,B,C]
abc.add(0, "Y");
System.out.println(abc); // prints: [Y,A,Z,B,C]
abc.add(abc.size()-1, "X");
System.out.println(abc); // prints: [Y,A,Z,B,X,C]
```

Once again, this version of ``add`` also returns a ``boolean``, but it will always return ``true``.

### T remove(int index)

Removing an item by its index is made possible by the ``remove`` method. It takes an ``int`` referring to the index to delete, and returns the value that was there, while modifying the list.

```java
// Set up our ABC list.
List<String> abc = new ArrayList<>();
abc.add("A");
abc.add("B"); 
abc.add("C");
System.out.println(abc); // prints: [A,B,C]

// Remove B.
System.out.println(abc.remove(1)); // prints: B
// Remove C.
System.out.println(abc.remove(1)); // prints: C
// Remove A.
System.out.println(abc.remove(0)); // prints: A
```

<p class="Think">
Why does calling <code>remove(1)</code> return different <code>String</code>s each time we call it?
</p>

### Copying a list

The two list classes, ``ArrayList`` and ``LinkedList`` -- as well as most other data structures Java gives you access to, can make a copy of another list using a special constructor.

```java
List<String> abc = new ArrayList<>();
abc.add("A");
abc.add("B");
abc.add("C");
// Copy abc.
List<String> copy = new ArrayList<>(abc);
// Delete all elements from ABC.
abc.clear();
System.out.println(abc); // prints: []
System.out.println(copy); // prints: [A, B, C]
```

## Some Additional Helpful Methods

The following methods are available on Java's List and you'll want to use them from time to time. However, they can all be built out of the methods we already have.

### boolean contains(Object item)

We can quickly check if a value is in a ``List`` using the contains method.

```java
List<String> ingredients = new ArrayList<>();
ingredients.add("Milk");
ingredients.add("Eggs"); 
ingredients.add("Flour");

if (ingredients.contains("Flour")) {
    System.out.println("We have flour!");
}
if (!ingredients.contains("Cheese")) {
    System.out.println("We need cheese!");
}
```

We can imagine that this method is implemented as follows, given a list ``lookThrough`` and the item we're looking for ``findMe``.

```java
public static boolean listContains(List<T> lookThrough, T findMe) {
    for (T actual : lookThrough) {
        if (findMe.equals(actual)) {
            return true;
        }
    }
    return false;
}
```

Notice that we get to ``return true`` immediately, as soon as we find a match, but in the worst case, we check every item in the list, and then ``return false`` because we don't find it. This method will work because a ``return`` statement immediately exits the method.

Note as well that we use the ``.equals`` operation to compare our two items. Remember that in Java that ``==`` only works on primitives: ``boolean``, ``int``, etc. and asks whether two objects are the exact same object for any class type.

<p class="Question">
Why does this method take an <code>Object</code> but all the others take a <code>T</code>?
</p>
<p class="Answer">
This method is ancient! The earliest versions of Java did not have the ability to write <code>List&lt;T></code> but you could use <code>List</code> which functions as a <code>List&lt;Object></code>. Original methods, like this <code>contains</code> therefore asked for <code>Object</code>-type variables. They could not change it to <code>T</code> when they invented it (later) because it would break all the Java code already written to use it, and so it remains.
</p>

Beware of type mismatches on ``contains`` because of this history:
```java
List<String> ingredients = new ArrayList<>();
ingredients.add("Milk");
ingredients.add("Eggs"); 
ingredients.add("Flour");

if (ingredients.contains(7)) {
    System.out.println("There will never be numbers in ingredients!");
}
```
We would expect Java to warn us about asking for ``7`` in the ``ingredients`` list, because ``7`` is not a ``String`` and ``ingredients`` is a ``List<String>``: therefore it will always return ``false``, but it can't because the ``contains`` method is defined to take an ``Object`` and will therefore accept anything!


### int indexOf(T item)

This method helps us find an item in our list. If it is in the list at all, we return the first position of that item. If we cannot find it, we get back ``-1``.

```java
List<String> ingredients = new ArrayList<>();
ingredients.add("Milk");
ingredients.add("Eggs"); 
ingredients.add("Flour");
ingredients.add("Eggs"); 

System.out.println(ingredients.indexOf("Milk")); // prints: 0
System.out.println(ingredients.indexOf("Eggs")); // prints: 1
System.out.println(ingredients.indexOf("Bacon")); // prints: -1
```

Again, we can write a version of this method given the ones we already know. It looks a lot like ``listContains``!

```java
public static boolean listFindIndex(List<T> lookThrough, T findMe) {
    for (int index=0; index<lookThrough.size(); index++) {
        if (findMe.equals(actual)) {
            return index;
        }
    }
    return -1;
}
```

Note that we had to change which style of for-loop we used in order to write this method.

### boolean remove(T item)

We can also remove items by value; that is: we search through the list, and if we find the item, we remove it. This time, we implement it using ``indexOf`` and the other ``remove``.

```java
boolean listRemoveByItem(List<T> lookThrough, T removeMe) {
    int index = lookThrough.indexOf(removeMe);
    if (index >= 0) {
        return lookThrough.remove(index);
    }
    return false;
}
```

<p class="Challenge">
Write and test this method without using <code>indexOf</code>.
</p>

## Non-Member Methods

Not all list functionality that comes with Java lives on the ``List`` class. A lot of the static methods on [``java.util.Collections``](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html) may be very useful. Here are some of the most useful.

### Collections.sort(List<T> modify)

Although we will discuss sorting later in more detail, sorting is an incredibly useful tool for solving problems. Most of the time we want to use Java's sorting algorithm to do that work for us, unless we have a special case where we need to write it ourselves.

```java
// Set up our ABC list, but backwards.
List<String> cba = new ArrayList<>();
cba.add("C");
cba.add("B"); 
cba.add("A");
System.out.println(cba); // prints: [C,B,A]

// Sort it!
Collections.sort(cba);
System.out.println(cba); // prints: [A,B,C]
```

This method, like all others, modifies a list in place, so we might need to make a copy of a list first, if we want it both sorted and not-sorted.

### Collections.reverse(List<T> modify)

We can also reverse a list, no matter what it is.

```java
List<String> racer = new ArrayList<>();
racer.add("R");
racer.add("A"); 
racer.add("C");
racer.add("E");
racer.add("R");
System.out.println(racer); // prints: [R,A,C,E,R]
Collections.reverse(racer);
System.out.println(racer); // prints: [R,E,C,A,R]
```

<p class="Think">
Why would the word "Racecar" be a bad choice for testing the above code?
</p>

### Collections.shuffle(List<T> modify)

You can also shuffle a list in Java. Try the following example a few times to see what it prints. The output should change each time you run it.

```java
// Set up our ABC list, but backwards.
List<String> abc = new ArrayList<>();
abc.add("A"); abc.add("B"); 
abc.add("C"); abc.add("D");
abc.add("E"); abc.add("F");
System.out.println(abc); // prints: [A,B,C,D,E,F]

Collections.shuffle(abc);
System.out.println(abc); // Try this a few times!
```

If you're interested in how shuffling works, check out the [Fisher-Yates Shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle). One approach to this problem is to assign random numbers to every entry in your list, and sort it. The "Fisher-Yates Shuffle" is more efficient.

## Some Differences from Python

Python's list is a built-in data type that provides some of the same abstract operations, under different names. If you previously studied python, it may be interesting or helpful for you to review some of the differences.

In python, ``size()`` was available as a special operation: ``len``, not a method.

```python
abc = ["A","B","C"]
print(len(abc)) # prints: 3
```

In python, ``add`` is called ``append``. Adding at a particular index was called ``insert``. 

```python
abc = []
abc.append("A")
abc.append("B")
abc.append("C")
print(abc) # prints: [A,B,C]
```

There is no ``get`` method, just square brackets. Python also supported negative indices, but in Java you must use size to get the last element.

```python
print(abc[0]) # prints: A
print(abc[1]) # prints: B
print(abc[-1]) # prints: C

# negative indexing is a short-hand for using the size
print(abc[len(abc)-1]) # prints: C
```

```java
// Equivalent to Python's abc[-1]:
System.out.println(abc.get(abc.size()-1));
```

## Summary of Methods

| Java Method | Java Usage | Closest Python | Java Description |
|----|----|----|----|
| ``int size()`` | ``xs.size()`` | ``len(xs)`` | returns the number of items |
| ``boolean add(T)`` | ``xs.add(x)`` | ``xs.append(x)`` | adds an item to the back of the list |
| ``T get(int)`` | ``xs.get(i)`` | ``xs[i]`` | get an item at the index |
| ``boolean add(int, T)`` | ``xs.add(i, x)`` | ``xs.insert(i, x)`` | inserts an item before the index |
| ``boolean contains(T)`` | ``xs.contains(x)`` | ``x in xs`` | returns true if the list has the item. |
| ``int indexOf(T)`` | ``xs.indexOf(x)`` | ``xs.index(x)`` | returns the position of the item, or -1 if not found. |
| ``boolean remove(T)`` | ``xs.remove(x)`` | ``xs.remove(x)`` | returns true if the item was found and deleted. |
| ``T remove(int)`` | ``xs.remove(i)`` | ``del xs[index]`` | returns the item at the given position and removes it. |

## Challenges

<h3 class="Challenge">Pick a Random Letter</h3>

Using a list of all the letters in the dictionary, e.g.,

```java
List<Character> letters = new ArrayList<>();
for (char c = 'A'; c <= 'Z'; c++) {
    letters.add(c);
}
```

Write code to choose a letter at random.

<h3 class="Challenge">Remove the Last Item from a List</h3>

Using the following starter-code, finish the definition of ``removeBack`` so it finds the last item from a list and returns it. 

```java
public static String removeBack(List<String> input) {
    // TODO: return the last item (and remove it!).
}

// ...

public static void main(String[] args) {
    List<String> abc = new ArrayList<>();
    abc.add("A"); abc.add("B"); abc.add("C");
    System.out.println(removeBack()); // should print: C
    System.out.println(abc); // should print: [A, B]
}
```

HINT: what index is the last?