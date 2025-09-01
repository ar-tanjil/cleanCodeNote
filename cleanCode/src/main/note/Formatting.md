# Formatting

## Vertical Formatting

### 1. The Newspaper Metaphor

Think of a well-written newspaper article. You read it vertically. At the top you expect a
headline that will tell you what the story is about and allows you to decide whether it is
something you want to read. The first paragraph gives you a synopsis of the whole story,
hiding all the details while giving you the broad-brush concepts. As you continue downward,
the details increase until you have all the dates, names, quotes, claims, and other minutia.

### 2. Vertical Openness Between Concepts

Nearly all code is read left ot right and top to bottom. Each line represents an expression or
a clause, and each group of lines represents a complete thought. Those thoughts should be
separated from each other with blank lines.

### 3. Vertical Density

If openness separates concepts, then vertical density implies close association. So lines
of code that are tightly related should appear vertically dense.

### 4. Vertical Distance

Concepts that are closely related should be kept vertically close to each other.
Clearly this rule doesn’t work for concepts that belong in separate files. But then closely
related concepts should not be separated into different files unless you have a very good
reason. Indeed, this is one of the reasons that protected variables should be avoided.

- *Variable Declarations* - Variable should be declared as close to their usage a possible. Local variables should
  appear a th top of each function. Control variables for loops should usually be declared
  within the loop statement.
- *Instance Variables* - Instance variables should be declared at the top of the class.
- *Dependent Functions* - If one function calls another, they should be vertically close,
  and the caller should be above the callee, if at all possible.
- *Conceptual Affinity* - Certain bits of code want to be near other bits. They have a certain
  conceptual affinity. the stronger that affinity, the less vertical distance there should be
  between them.

### 5. Vertical Ordering

In general we want function call dependencies to point in the downward direction. That is,
a function that is called should be below a function that does the calling. This creates a
nice flow down the source code module from high level to low level.

## Horizontal Formatting

Line should be 100 to 120 characters long.

### 1. Horizontal Openness and Density

We use horizontal white space to associate things that are strongly related and disassociate
things that are more weakly related.

### 2. Horizontal Alignment

Horizontal Alignment has no benefit.

### 3. Indention

Do indention

### 4. Dummy Scopes

*Don't do this, use braces*

```java
public void method() {
    while (dis.read(buf, 0, readBufferSize) != -1)
        ;
}
```

## Team Rules

Everyone has favourite rule, but when working on team follow
the team rule.

