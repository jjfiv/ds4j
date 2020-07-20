# Comparison in Java

We sort and compare items as humans because it helps us find things faster. 

Consider the following objects:

```java
List<Book> books = new ArrayList<>();
books.add(new Book("Pride and Prejudice", "Austen, Jane", 1813));
books.add(new Book("Emma", "Austen, Jane", 1815));
books.add(new Book("Frankenstein", "Shelley, Mary", 1818));
```

They are of class ``Book``, which looks roughly like this:

```java
public class Book {
    String title;
    String author;
    int publicationYear;
    public Book(String title, String author, int publicationYear) {
        this.title = title;
        this.author = author;
        this.publicationYear = publicationYear;
    }

    // ... methods ...
}
```

### Print these books

To move along our example, we'll need to be able to print an object of the ``Book`` class, so we'll override Java's ``toString()`` method.

```java
@Override
public String toString() {
    return "**" + this.title + "**" +
           " by " + this.author + 
           " (" + this.publicationYear + ")";
}
```

Now if we wrap up our ``main`` method with a ``System.out.println(books);``, we'll get the following output:

``
[**Pride and Prejudice** by Austen, Jane (1813), **Emma** by Austen, Jane (1815), **Frankenstein** by Shelley, Mary (1818)]
``

### Sort these books

If I ask you to sort these books, perhaps you do so by ``author``, or by ``title``, or by ``publicationYear``. All the fields of this class are sortable. Maybe you ask me a follow-up question to determine how I want them sorted.

In fact, that's exactly what Java does. If we add a sort statement before printing, it tells us it doesn't know how we want to ``compare`` book objects.

```java
Collections.sort(books);
```

The output is not particularly friendly.

```
Comparison.java:30: error: no suitable method found for sort(List<Book>)
        Collections.sort(books);
                   ^
    method Collections.<T#1>sort(List<T#1>) is not applicable
      (inference variable T#1 has incompatible bounds
        equality constraints: Book
        lower bounds: Comparable<? super T#1>)
    method Collections.<T#2>sort(List<T#2>,Comparator<? super T#2>) is not applicable
      (cannot infer type-variable(s) T#2
        (actual and formal argument lists differ in length))
  where T#1,T#2 are type-variables:
    T#1 extends Comparable<? super T#1> declared in method <T#1>sort(List<T#1>)
    T#2 extends Object declared in method <T#2>sort(List<T#2>,Comparator<? super T#2>)
1 error
```

Basically, this boils down to needing a ``Comparator<T>`` or a ``Comparable<T>``. But what are those?

## Java classes for Sorting

### What is a ``Comparable<T>``?

``Comparable`` is a Java ***interface*** which can be implemented on a class to define the default order of a class. Maybe you have a ``User`` class in your application, and you always want to sort them by their ``String username``.

```java
public class User implements Comparable<User> {
    String username;
    // ... et. cetera ...

    /** Compare this User to another. */
    int compare(User other) {
        return this.username.compareTo(other.username);
    }
}
```

Often ``Comparable<T>`` makes sense for your objects. Here, the ``T`` is not a class inside this class (as it is in ``List<T>``) but actually it is the name of the class you are making ``Comparable``.


<div class="Tangent" markdown="1">

Note that there are some advanced situations where you might make a class ``Comparable`` to another class besides itself, but it would be a ``super``-class of the current class.
</div>

With this definition in place, we can call Java's sorting methods:

```java
List<User> users = //...
Collections.sort(listOfUsers);
```

### What is a ``Comparator<T>``?

Sometimes there is not an obvious default order for a class. Let's go back to our book example.

```java
public class Book {
    String title;
    String author;
    int publicationYear;
    // ... snip ...
}
```

We may want to sort by ``title`` at one moment, then later by ``author``, and then later by ``publicationYear``. With only ``Comparable``, we would need to have 3 different ``Book`` classes and we would need to shuffle data in and out.

Instead, we can make three ``Comparator`` classes that go with the ``Book`` class.

```java
public class Book {
    String title;
    String author;
    int publicationYear;
    // ... snip ...

    /** This class lets us sort Books by title. */
    static class TitleCmp implements Comparator<Book> {
        public int compare(Book left, Book right) {
            return left.title.compareTo(right.title);
        }
    }
}
```

With a ``Comparator`` defined, we can then sort a list of books with that specific ``Comparator`` object.

```java
List<Book> books = //...
Collections.sort(books, new Book.TitleCmp());
System.out.println(books);
```

We now get books alphabetically by title:

``[**Emma** by Austen, Jane (1815), **Frankenstein** by Shelley, Mary (1818), **Pride and Prejudice** by Austen, Jane (1813)]``

### Java 8 Lambdas (easier Comparators!).

In Java 8, a simpler way to create objects from classes that only define 1 method was introduced: ***lambda*** syntax.

For instance, we can print out all three orders concisely as:

```java
Collections.sort(books, 
  (left, right) -> left.author.compareTo(right.author));
System.out.println(books);
Collections.sort(books, 
  (left, right) -> left.title.compareTo(right.title));
System.out.println(books);
Collections.sort(books, 
  (left, right) -> Integer.compare(left.publicationYear, right.publicationYear));
System.out.println(books);
```

But we have to know that we're defining a ``Comparator`` and it's method takes two arguments. Nicely enough, Java will figure out the rest.


### Comparable vs. Comparator

TL;DR: Here's a table comparing them:

| Method | Left | Right | Comments |
|--------|------|-------|----------|
| ``int compare(T left, T right)`` | ``left`` | ``right`` | Comparators take in two arguments. |
| ``int compare(T other)`` | ``this``| ``other`` | Comparables take in one argument and compare to themselves. |

### Why do ``compare`` methods return an ``int``?

Compare methods are a replacement for ``<``, ``>``, and ``==`` for your new class, and therefore have three possible outputs.

| Situation | Output |
|-----------|--------|
| ``a < b`` | a negative number |
| ``a == b`` | 0 |
| ``a > b`` | a positive numbers |

If you think about integer subtraction, ``int compare(int a, int b) { return a - b; }`` performs appropriately, returning numbers that are interpretable as all three comparisons!

#### Sorting in the other direction:

Because it's an integer, we can also sort in the opposite direction, descending (a.ka., Z to A, biggest to smallest, etc.) by writing a ``Comparator`` or ``Comparable`` that takes the negative of an existing one. We also have access to ``Collections.reverse(list)``.

### Why is comparing Strings fairly slow?

When you have other types, you tend to be fancier. For instance, here's a sketch of how to compare two strings:

```java
class StrCmp implements Comparator<String> {
    int compare(String left, String right) {
        int N = left.length();
        // Can't be equal if they have different sizes.
        if (right.length() != N) {
            // Shorter strings first:
            return Integer.compare(N, right.length());
        }

        // If they have the same size...
        for (int i = 0; i < N; i++) {
            char lc = left.charAt(i);
            char rc = right.charAt(i);
            // If any character is different, exit immediately with which direction!
            if (lc != rc) {
                return Character.compare(lc, rc);
            }
        }
        // If we got to the end, they must be the same.
        return 0;
    }
}
```

Never use this code; trust that ``java.lang.String`` is a ``Comparable`` already and just call ``left.compareTo(right);`` as we've been doing earlier.


## Sorting IRL: How do I compare...?

| Class/Type | Method |
|---|---|
| ``byte`` | ``Byte.compare(x, y)`` |
| ``short`` | ``Short.compare(x, y)`` |
| ``int`` | ``Integer.compare(x, y)`` |
| ``long`` | ``Long.compare(x, y)`` |
| ``char`` | ``Character.compare(x, y)`` |
| ``String`` | ``x.compareTo(y)`` |
| ``Comparable<T>`` | ``x.compareTo(y)`` |
| Your class here. | Define it yourself. |
