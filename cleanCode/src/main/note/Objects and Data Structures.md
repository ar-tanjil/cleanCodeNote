# Objets and Data Structures

## Data Abstraction

We do not want to expose the details of our data.
Rather we want to express our data in abstract terms.

```java
// concrete Point 
public class Point {
    public double x;
    public double y;
}
```

```java
// Abstract Point
public interface Point {
    double getX();

    double getY();

    void setCartesian(double x, double y);

    double getR();

    double getTheta();

    void setPolar(double r, double theta);
}
```

## Data/Object Anti-Symmetry

Objects hide their data behind abstractions and expose
functions that operate on that data.
Data structure expose their data and have no meaningful functions.

*Procedural code (code using data structures) makes it easy to add new functions
without changing the existing data structures. OO code, on the other hand, makes
it easy to add new classes without changing existing functions.*

The Complement is also true:
*Procedural code makes it hard to add new data structures because all the functions
must change. OO code makes it hard to add new functions because all the classes must
change.*

In any complex system there are going to be times when we want to add new data
types rather than new functions. For these cases objects and OO are most appropriate. On
the other hand, there will also be times when we’ll want to add new functions as opposed
to data types. In that case procedural code and data structures will be more appropriate.

Mature programmers know that the idea that everything is an object is a myth. Sometimes
you really do want simple data structures with procedures operating on them.

```java
// Procedural Shape
public class Square {
    public Point topLeft;
    public double side;
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.141592653589793;

    public double area(Object shape) throws NoSuchShapeException {
        if (shape instanceof Square) {
            Square s = (Square) shape;
            return s.side * s.side;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.height * r.width;
        } else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return PI * c.radius * c.radius;
        }

        throw new NoSuchShapeException();
    }
}
```

```java
// Polymorphic Shapes

public interface Shape {
    double area();
}

public class Square implements Shape {
    public Point topLeft;
    public double side;

    @Override
    public double area() {
        return side * side;
    }
}

public class Rectangle implements Shape {
    public Point topLeft;
    public double height;
    public double width;

    @Override
    public double area() {
        return height * width;
    }
}

public class Circle implements Shape {
    public Point center;
    public double radius;

    public final double PI = 3.141592653589793;

    @Override
    public double area() {
        return PI * radius * radius;
    }
}
```

## The Law of Demeter

That says a module should not know about the innards of the objects it
manipulates. This means that an object should not expose its internal
structures through accessors.

More precisely, the Law of Demeter says that a method `f` of a class `C` should
only call the methods of these:

- `C`
- An object created by `f`
- An object passed as an argument to `f`
- An object held in an instance variable of `C`

The method should not invoke methods on objects that are returned by any
of the allowed functions. In other words, talk to friends, not to strangers.

## Train Wrecks

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

This kind of code is often a train wreck because it looks like a bunch of coupled
train cars, and should be avoided. Best to split them up as follows.

```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

## Hybrids

Hybrid structures that are half object and half data structure.
Avoid creating them.

## Hiding Structures

```java
import java.io.BufferedOutputStream;

BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

This allows ctxt to hide its internals and prevents the current function from
having to violate the Law of Demeter by navigating through objects it shouldn’t know about.

## Data Transfer Objects

The quintessential form of a data structure is a class with public variables and no functions.
This is sometimes called a data transfer object, or DTO. Very useful structures.
Making it beans seems to make some OO purists feel better but usually provides no other benefit.

```java
public class Address {
    private String street;
    private String streetExtra;
    private String city;
    private String state;
    private String zip;

    public Address(String street, String streetExtra,
                   String city, String state, String zip) {
        this.street = street;
        this.streetExtra = streetExtra;
        this.city = city;
        this.state = state;
        this.zip = zip;
    }

    public String getStreet() {
        return street;
    }

    public String getStreetExtra() {
        return streetExtra;
    }

    public String getCity() {
        return city;
    }

    public String getState() {
        return state;
    }

    public String getZip() {
        return zip;
    }
}
```

## Active Record

Active Records are special forms of DTOs. They are data structures with public
(or bean accessed) variables; but they typically have navigational methods like save and find.
Typically these Active Records are direct translations from database tables,
or other data sources.

Unfortunately we often find that developers try to treat these data structures as though
they were objects by putting business rule methods in them. This is awkward because it
creates a hybrid between a data structure and an object.

The solution, of course, is to treat the Active Record as a data structure and to create
separate objects that contain the business rules and that hide their internal data (which are
probably just instances of the Active Record).

## Conclusion

Objects expose behavior and hide data. This makes it easy to add new kinds of objects
without changing existing behaviors. It also makes it hard to add new behaviors to existing
objects. Data structures expose data and have no significant behavior. This makes it easy to
add new behaviors to existing data structures but makes it hard to add new data structures
to existing functions.

In any given system we will sometimes want the flexibility to add new data types, and
so we prefer objects for that part of the system. Other times we will want the flexibility to
add new behaviors, and so in that part of the system we prefer data types and procedures.
Good software developers understand these issues without prejudice and choose the
approach that is best for the job at hand.