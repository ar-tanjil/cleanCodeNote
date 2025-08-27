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