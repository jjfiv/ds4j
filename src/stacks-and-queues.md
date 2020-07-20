# Stacks & Queues

It turns out that lists are very useful data structures. In fact, people use lists for all sorts of behaviors. Two behaviors are so common that we have given them their own names.
We will discuss these behaviors again when we are implementing our own data structures but here we discuss these "archetypal" data structures.

## Queues (FIFO)

Queues are type of list where you are always operating on opposite ends of a list. Imagine you're waiting in line for food at a dining hall or at a restaurant. You see a line of people, and so you add yourself to the back of the line. The dining hall serves people one at a time from the front of the line. You know that eventually you will reach the front.

Queue behavior is sometimes referred to as "First-Come First-Serve" (FCFS) or "First-In, First-Out" (FIFO).

Java has a ``Queue`` class, and ``LinkedList`` is a queue.

```java
Queue<String> diningHall = new LinkedList<>();
diningHall.add("A");
diningHall.add("B");
System.out.println(diningHall.remove()); // prints: A
diningHall.add("C");
diningHall.add("D");
System.out.println(diningHall.remove()); // prints: B
System.out.println(diningHall.remove()); // prints: C
System.out.println(diningHall.remove()); // prints: D
```

Note that we could have written this program with ``diningHall.remove(0)`` on a regular ``List<String>``. Choosing ``Queue`` over ``List`` typically means that you know exactly how you are going to use this data structure and it would help readers of your code to know that, too.

## Stacks (LIFO)

Stacks are a type of list where you are always operating on the same side. Imagine you take all your textbooks and pile them up on top of your desk. You realize that your computer science book is on the bottom, and your Spanish Language textbook is on top. You decide to do your Spanish homework first, so as not to knock over your stack of books. Stacks allow you to pile up work for later, and get back to it in a "most-recent" first manner.

Stack behavior is sometimes referred to as "Last-In, First-Out" (LIFO) in symmetry to Queues which are FIFO.

Java has a ``Stack`` class, but they don't recommend it. In fact they recommend the ``Deque`` interface, which is more powerful and we will discuss briefly in the next section.

You can remove from the back of any list by doing: ``list.remove(list.size()-1)``.

```java
List<String> books = new ArrayList<>();
books.add("CS");
books.add("Philosphy");
System.out.println(books.remove(books.size()-1)); // prints: Philosophy
books.add("Logic");
books.add("Spanish");
System.out.println(books.remove(books.size()-1)); // prints: Spanish
System.out.println(books.remove(books.size()-1)); // prints: Logic
System.out.println(books.remove(books.size()-1)); // prints: CS
```

If you really want a stack for your project, it may make sense to write a simple class to handle it:

```java
/** Design push/pop for more clear stack-code */
public class StackOfBooks {
    /** Using an arraylist to build a Stack. */
    private List<String> books = new ArrayList<String>();

    /** Add a book to the top of the stack */
    public void push(String book) {
        this.books.add(book);
    }
    /** Remove a book from the top of the stack */
    public String pop() {
        if (this.books.size() == 0) {
            throw new IllegalStateException("Out of books!");
        }
        return this.books.remove(this.books.size() - 1);
    }

    public static void main(String[] args) {
        StackOfBooks books = new StackOfBooks();
        books.push("CS");
        books.push("Philosphy");
        System.out.println(books.pop()); // prints: Philosophy
        books.push("Logic");
        books.push("Spanish");
        System.out.println(books.pop()); // prints: Spanish
        System.out.println(books.pop()); // prints: Logic
        System.out.println(books.pop()); // prints: CS 
        System.out.println(books.pop()); // throws: "Out of books!" 
    }
}
```

Then you get some control over what happens when you try to grab a book and it's not there; and you can add methods as your algorithm or data structure grows.

### The Stack: Method Calls and Recursion

Note that computer architecture has the notion of a stack -- in fact, we normally call that notion ***The Stack***. This is a stack in the data structure sense, but it is implemented in a special way and stores a whole bunch of mixed data.

In fact, when we call methods or functions, we are pushing that code onto ***The Stack***. When we issue a ``return`` statement, we are popping that code from the stack (and maybe pushing the return value!). Stacks are therefore fundamental to how most computer programming languages work -- and we couldn't have most recursive functions (functions that call themselves) without it.

## Double-Ended Queues (Deques)

The ``Deque<T>`` interface in Java wraps up data structures for which it is efficient to add or remove from the different sides of a list. ``LinkedList`` not only implements ``Queue`` but it also implements ``Deque``.

As a ``Deque<T>`` you get to call the methods:

| Front | Back | Returns |
| --- | --- | --- |
| ``addFirst`` | ``addLast`` | ``void``, nothing. |
| ``removeFirst`` | ``removeLast`` | ``T``, the item removed. |
| ``getFirst`` | ``getLast`` | ``T``, the first/last item. |

If you want a generic ``Queue``, you can either:

 - ``addLast`` and ``removeFirst`` or
 - ``addFirst`` and ``removeLast``

If you want a generic ``Stack``, you can either:

 - ``addLast`` and ``removeLast`` or
 - ``addFirst`` and ``removeFirst``

## Fancier Queues

This section exists as a caveat in case you decide to google around for queue data structures. There are a large class of data structures that are all, broadly, queues and not all of them are relevant to our notion of a queue.

### Priority Queues

Later in this course we will see so-called "Priority Queues" (typically implemented as a Heap), which allow you to maintain a queue with importances. For example, the queue to see a professor at office hours is likely to be maintained first by arrival time (FIFO) but students who need to go to another class may petition their classmates to cut in line. Therefore, some sense of urgency or importance is incorporated into the queue. 

Even using a priority queue is slightly more difficult because you need to be able to sort the elements you put in; therefore, a priority queue is not quite a list.

### Aside: Comparable for PriorityQueue

Another example of a Priority Queue in the real world is a hospital. To use Java's ``PriorityQueue<T>`` we need to implement an interface called ``Comparable<T>`` -- we haven't discussed interfaces in depth and we will return to ``Comparable`` later when we get to sorting.

```java
public class Patient implements Comparable<Patient> {
    long arrivalTime;
    boolean severe;
    String name;

    //... constructor and more methods ...

    @Override
    public int compareTo(Patient p) {
        // severe=true then false
        int cmp = -Boolean.compare(this.severe, p.severe);
        if (cmp != 0) return cmp;
        // earlier times better:
        cmp = Long.compare(this.arrivalTime, p.arrivalTime);
        if (cmp != 0) return cmp;
        // by name if all else equal
        return this.name.compareTo(p.name);
    }
}
```

### Concurrent or Threadsafe Queues

In a more advanced programming class or an operating systems class, more subtleties are added to queue design: what happens if two requests come in at the same time? 

In the dining hall metaphor, the two students arriving at the same time will sort out for themselves which order they should be added to the queue. Elements in our lists are not so intelligent, but some queues are smarter and work to deal with such conflicts without causing the line to end up in disarray.

A computer uses an abstraction called threads to run code at the same time, and therefore queues that are safe to use in this environment are called concurrent or threadsafe queues. Java has some of these in its standard library but we will not dicuss them in this course.

