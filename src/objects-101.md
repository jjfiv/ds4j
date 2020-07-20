# Objects & Classes

Object-Oriented Programming (OOP) is a way of organizing data (aka State) and code (aka methods, functions, Behavior) into classes and objects. 

Classes are like stamps or templates, in that you can create many instances of the class, called objects.

## A Tale of two Wallets

```java
// I did not have a lot of money to spend in College.
OneCard me = new OneCard("Myself", 3.00);
// But I had friends with lots of money. Like, $50.
OneCard myRichFriend = new OneCard("Friend", 50.00);


// To make it worse, they could add money to their card all the time, like $20!
myRichFriend.deposit(20.00);

// I can't afford $60 worth of anything.
System.out.println(me.canAfford(60.00)); // prints: false

// But my friend could.
System.out.println(myRichFriend.canAfford(60.00)); // prints: true

// That is, until they spent $13.87 on a fancy coffee of some kind.
myRichFriend.spend(13.87);
// Now, they can't afford a $60 thing.
System.out.println(myRichFriend.canAfford(60.00)); // prints: false
```

Running this code will give the following output:
```
false
true
false
```

In this previous example -- there are two objects of type ``OneCard`` which models a person's wallet. We could have written a similar sassy story using variables of type ``double`` but numbers don't have nice methods like ``canAfford``, ``deposit`` and ``spend``. Also, in a bigger example, we'd want to associate names with accounts and maybe keep a list of transactions.

1. To build this class, I would first create a new file, named ``OneCard.java``.
2. Then I would add fields to this class, a ``double balance`` and maybe a ``String owner``.
3. Then we need a constructor -- something that will create a new object when we call ``new OneCard(...)``.
4. Then we would design the methods: ``canAfford``, ``deposit`` and ``spend``.

### Creating the OneCard class: fields and a constructor

```java
public class OneCard {
  /**  Who owns this card? */
  String owner;
  /** How much money is stored here? */
  double balance;
  
  public OneCard(String person, double money) {
    this.owner = person;
    this.balance = money;
  }

  // ... methods later go here ...
}
```

Fields live on the class itself, and by convention, we put them at the top, although we could put them anywhere.

This code is the constructor. Note that it's not quite a normal method definition -- we have ``public`` but the name and the return-type have been merged to be ``OneCard`` the name of the class. This marks the method as being special: a constructor.

```java
public OneCard(String person, double money) {
    this.owner = person;
    this.balance = money;
}
```

This method has two parameters: ``String person`` and ``double money``. In the longer code example, ``person`` is first ``"Myself"`` and then ``"Friend"``, and ``money`` changes similarly.

We could create yet another instance of a ``OneCard`` by running the following code:
```java
OneCard empty = new OneCard("unknown", 0.00);
```
Then, we have effectively created ``empty.balance = 0.00;`` and ``empty.owner = "unknown";`` by calling the constructor via the ``new`` keyword.

### Methods of OneCard

#### ``deposit`` and ``canAfford``

The following is a possible definition of both ``deposit`` and ``canAfford`` on our ``OneCard`` class. Notice that nothing on this class uses the ``static`` keyword -- this is important. We want access to ``owner`` and ``balance`` in these methods; but we need to go through the ``this`` keyword, which is a special variable that represents the current instance of the class. When we call something like ``me.canAfford(...)`` the variable ``me`` becomes ``this`` for the body of the method ``canAfford`` in this case, that gets called.

```java
public class OneCard {
  /**  Who owns this card? */
  String owner;
  /** How much money is stored here? */
  double balance;

  public OneCard(String person, double money) {
    this.owner = person;
    this.balance = money;
  }

  public void deposit(double amount) {
    this.balance += amount;
  }

  public boolean canAfford(double amount) {
    return this.balance > amount;
  }
 
  // we'll put spend here...
}
```

Here we get a clue to the meaning of ``void`` -- something that's part of the main method we've learned to type. ``void`` means nothing -- so when we put it in the return-value position for a method, it means that our method returns nothing. Adding money to a bank account or a ``OneCard`` should always succeed, so I don't know what we would return here (maybe the new balance?) but it's appropriate to return nothing.

#### spend (to demonstrate exceptions!)

Spend is an interesting operation -- because it should be able to fail if you don't have enough money. This will be true when we get to the ***data structures*** portion of the class (imagine asking to delete from an empty list, or asking for item number 5 of a 3 item list.).

```java
public void spend(double amount) {
  if (!this.canAfford(amount)) {
    throw new RuntimeException(this.owner + " can't afford "+amount);
  }
  this.balance -= amount;
}
```

There are many types of exceptions (or errors) in Java. We saw one in the GuessingGame. When we want to create our own error, and crash the program, we will use ``RuntimeException``. 

| Language: | Response: |
|----|----|
| English | "You can’t do that!" |
| Python | ``raise ValueError(message)`` |
| Java | ``throw new RuntimeException(message);`` |

It may be a better design to make a spend that returns a true/false value, i.e.,

```java
public boolean spend(double amount) {
  if (!this.canAfford(amount)) {
    return false;
  }
  this.balance -= amount;
  return true;
}
```

Because then you can choose how to handle not being able to spend the money in the code that is interacting with teh ``OneCard`` class. 

### So then, what's a static method?

All of the methods presented above are ***instance methods***, which means they depend on a particular instance of a class. Static methods, like the ``public static void main(String[] args) { }``  we use to run code do not need or have access to a ``this`` special variable; and that can be a good thing!

For the ``OneCard`` example, imagine that we wanted a set of vendors that accepted the ``OneCard`` as money. This would not be different for each ``OneCard``, but we may want that set to be associated with the class anyway:

```java
public class OneCard {
  // everything we've seen.

  /** Supported set of vendors -- can't have any duplicates, so we use a set rather than a list. */
  public static Set<String> supportedStores = new HashSet<String>();
}
```

We will discuss sets further in the future -- think of them as lists that keep items unique and remove duplicates for you!


## Vocabulary

In this section, we try to define a lot of common terminology for classes.

### An Object is an instance of a Class.

Classes are the template, and objects are generated from the template.

### State and Behavior

There are many programming languages that use Object-Oriented programming concepts, and as a result, there are many names for some of these things! 

#### Behavior

We will refer to methods on a class as "methods" but there are ``static`` methods on a class, and so sometimes we will refer to them as "instance methods" to tell them apart from static methods.

- Method
- Instance Method
- Action
- Messages / Signals

#### State

There are many names for the variables associated with a class.

- Instance Variables
- Member Variables
- Members
- Properties
- Attributes
- Fields

### Constructors

Constructors run code on the ``new`` keyword. The parameters they ask for are used to set up the class (but they need not be instance variables.)

#### Parameters vs. Variables

In a hypothetical ``Dog`` class, we might want to store the age of a dog in so-called "dog-years" rather than "human-years."

```java
public class Dog {
  private int dogYears;
  public String name;

  public Dog(String name, int yearsOld) {
    this.name = name;
    this.dogYears = yearsOld * 7;
  }
  public int getDogYears() {
    return this.dogYears;
  }
  public int getHumanYears() {
    return this.dogYears / 7;
  }
}
```

By then writing methods for getting to our ``dogYears`` instance variable, users of this class don't need to know whether human years or dog years are stored, merely that they have access to both!

### Class keywords

There are a lot of other keywords that go with classes. I'll try to define them here.

#### public vs. private

For now, our definition of ``public`` is that it allows you to see variables, methods, and classes marked with it from other ``.java`` source code files. Therefore, when in doubt, we can lean toward making things ``public``.

For now, our definition of ``private`` is that it allows you to hide variables, methods, and classes marked with it from other ``.java`` source code files: in our earlier example about Parameters and Variables where we discussed the ``private int dogYears`` field, it made sense to hide it from the user and make them call the public methods to get either ``getHumanYears()`` or ``getDogYears()``.

Note that there are two other levels of visibility in Java: ``package`` and ``protected``. ``package``-level things can be seen from files and classes the same folder as the current class (this is what happens when you just write ``int dogYears`` on a field -- there's no ``package`` keyword despite it being a level). And ``protected`` makes things visible to classes that inherit directly from this class -- we will discuss inheritance later in the course.

#### static vs. instance

I have the opinion that all dogs are cute. Having a variable to represent a specific dog, e.g., ``buddy`` is not necessary for knowing whether a dog is cute.

| Signature | Has this reference? | How do I call it? |
|----|----|----|
| ``public static boolean isCute()`` | No ``this`` involved. | ``if (Dog.isCute()) { … }`` |
| ``public boolean isCute()`` | ``this=buddy`` or whichever object you call it on. | ``if (buddy.isCute()) { … }`` |

Methods that are in a class (because Java requires everything to be in a class) but don't really need to be are called ***static methods***. You can also have ***static variables***. Notably, you don't need to create an object in order to call them.

This is useful to Java as well as to us -- recall our main method:

```java
public static void main(String[] args) {
  // public
  //        lets us start the program from outside the file it is defined.
  // static 
  //        lets us run the code in main without creating the object that owns it.
  // void 
  //        means that it doesn't return anything.
  // String[] args 
  //               is an array of strings from the user to start this program; 
  //               although it will always be blank in this class.
}
```

