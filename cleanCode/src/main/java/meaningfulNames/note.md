# Meaningful Names

## 1. Use Intention-Revealing Names.

It is easy to say that names should reveal intent.
It should tell you why it exists, what it does, and how
it is used. If a name requires a comment, then the name does
not reveal its intent.

**Problem code**

```java
import java.util.ArrayList;
import java.util.List;

public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<>();
    for (int[] x : theList)
        if (x[0] == 4)
            list1.add(x);
    return list1;
}
```

**Solutions 1**

```java
import java.util.ArrayList;
import java.util.List;

public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<>();
    for (int[] cell : gameBoard)
        if (cell[0] == FLAGGED)
            flaggedCells.add(x);
    return flaggedCells;
}
```

**Solutions 2**

```java
import java.util.ArrayList;
import java.util.List;

public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCell = new ArrayList<>();
    for (Cell cell : gameBoard)
        if (cell.isFlagged())
            flaggedCell.add(cell);
    return flaggedCell;
}
```

## 2. Avoid Disinformation

We should avoid words whose entrenched meanings vary from our
intended meaning. For example:

1. Do not refer to a grouping of accounts as an `accountList`
   unless it's actually a `List`. The word list means something specific
   to programmers.
2. Beware of using names which vary in small ways.
   Like `XYZControllerForEfficientHandlingOfStrings` And
   `XYZControllerForEfficienStorageOfStrings`.
3. Spelling similar concepts similarly is information, Using inconsistent
   spellings is dis-information.
4. Use of lower-case L or uppercase O as variable names, especially in combination.

## 3. Make Meaningful Distinctions

1. Don't make distinctions by changing name arbitrary way, for example
   by misspelling one.
2. Don't add number series or noise words - they are
   non-informative; they provide no clue to the author's intention.

```java
public static void copyChars(char[] a1, char[] a2) {
    for (int i = 0; i < a1.length; i++)
        a2[i] = a1[i];
}
```

*The function reads much better when `source` and `distination` are used
for argument names.*

3. Noise words are another meaningless distinctions. Imagine that you
   have a `Product` class. If you have another called `ProductInfo` or `ProductData`,
   you have made the names different without making them mean anything different.
   `Info` And `Data` are indistinct noise words like `a, an` and `the`.
   There is nothing wrong with using prefix conventions so long they make
   a meaningful distinction.
4. The word `variable` should never appear in a variable name.
5. The word `table` should never appear in a table name.
6. No need to use `NameString`, because `name` is a String. If you make
   it number it is dis-information.
7. Do not name one class `Customer` and other is `CustomerObject`.

## 4. Use Pronounceable Names.

If you can't pronounce it, you can't discuss it without sounding
like an idiot.

**Compare**

```java
import java.util.Date;

class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
}
```

**To**

```java
import java.util.Date;

class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
}
```

## 5. Use Searchable Names

Single letter names can ONLY be used as local variables inside short methods.
*The length of a name should correspond to the size of its scope.* If variable
might be seen or used in multiple places in a body of code, it is imperative to
give it a search friendly name.

**Compare**

```java
public void method() {
    for (int j = 0; j < 34; j++) {
        s += (t[j] * 4) / 5;
    }
}
```

**To**

```java
public void method() {
    int realDaysPerIdealDay = 4;
    final int WORK_DAYS_PER_WEEK = 5;
    int sum = 0;
    for (int j = 0; j < NUMBER_OF_TASK; j++) {
        int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
        int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
        sum += realTaskWeeks;
    }
}
```

## 6. Avoid Encodings Or Hungarian Notation

Don't encode type or scope information into names.

## 7. Member Prefixes

Don't use prefix member variables with m_ anymore.

```java
public class Part {
    private String m_dsc; // The textual description

    void setName(String name) {
        m_dsc = name;
    }
}
```

```java
public class Part {
    String description;

    void setDescription(String description) {
        this.description = description;
    }
}
```

## 8. Interfaces and Implementations

These are sometimes a special case for encodings. For example,
say you are building an ABSTRACT FACTORY for the creation of shapes.
This factory will be an interface and will be implemented by a concrete class.
What should you name them? IShapeFactory and ShapeFactory?
I prefer to leave interfaces unadorned. The preceding I, so common in
today’s legacy wads, is a distraction at best and too much information at worst.
I don’t want my users knowing that I’m handing them an interface.
I just want them to know that it’s a ShapeFactory.
So if I must encode either the interface or the implementation, I choose
the implementation. Calling it ShapeFactoryImp, or even the hideous CShapeFactory,
is preferable to encoding the interface.

## 9. Avoid Mental Mapping

Readers shouldn’t have to mentally translate your names into other names
they already know. Like you give a single letter, if scope is little you
can do it (traditional).

## 10. Class Names.

Classes and objects should have noun or noun phrase names like Customer, WikiPage,
Account, and AddressParser. Avoid words like Manager, Processor, Data,
or Info in the name of a class. A class name should not be a verb.

## 11. Method Names.

Methods should have verb or verb phrase names like postPayment, deletePage, or save.
Accessors, mutators, and predicates should be named for their value and prefixed with get,
set, and is according to the javabean standard.
Use static factory method instead of constructors overloading.

## 12. Don't Be Cure.

Like naming something `HolyHandGrenade` instead of `DeleteItem` or `whack()`
instead of `kill()`. *Say what you mean. Mean what you say.*

## 13. Pick One Word per Concept.

Pick one word for one abstract concept and stick with it. For instance, it’s confusing to
have fetch, retrieve, and get as equivalent methods of different classes

## 14. Don't Pun.

Avoid using the same word for two purpose. Using the same term for two different ideas
is essentially a pun. Like using `add` for insert in list and also for creating a new
value by summing/concatenating two existing value.

## 15. Use Solution Domain Names.

Name already familiar to programmer Like `employeeList, JobQueue`.

## 16. Use Problem Domain Names.

When there is no “programmer-eese” for what you’re doing, use the name from the problem domain.
At least the programmer who maintains your code can ask a domain expert what it means.
Separating solution and problem domain concepts is part of the job of a good programmer and designer.
The code that has more to do with problem domain concepts should have names drawn from the problem domain.

## 17. Add Meaningful Context.

**Variables with unclear context**
```java
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;

    if (count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }

    String guessMessage = String.format("There %s %s %s%s",
            verb, number, candidate, pluralModifier);
   System.out.println(guessMessage);
}
```

**Variable with context**
```java
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;
    
    public String make(char candidate, int count){
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s",
               verb, number, candidate, pluralModifier);
    }

   private void createPluralDependentMessageParts(int count) {
      if (count == 0) {
         thereAreNoLetters();
      } else if (count == 1) {
         thereIsOneLetter();
      } else {
         thereAreManyLetters(count);
      }
   }
   private void thereAreManyLetters(int count) {
      number = Integer.toString(count);
      verb = "are";
      pluralModifier = "s";
   }
   private void thereIsOneLetter() {
      number = "1";
      verb = "is";
      pluralModifier = "";
   }
   private void thereAreNoLetters() {
      number = "no";
      verb = "are";
      pluralModifier = "s";
   }
    
}
```

## 18. Don't Add Gratuitous Context.
