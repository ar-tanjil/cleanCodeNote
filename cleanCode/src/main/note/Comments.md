# Comments

Nothing can be quite so helpful as a well-paced comment.
Nothing can clutter up a module more than frivolous comments.
The proper use of comments is to compensate for our failure to
express ourself in code. And comments lie, because code evolve
but comment does not change.

## 1. Comments Do Not Make Up for Bad Code.

Don't comment bad code - rewrite it, or clean it.
Rather than spend your time writing the comments that
explain the mess you've made, spend it cleaning it.

## 2. Explain Yourself in Code

```java
// Check  to see if the employee is eligible for full benefits
public void method() {
    if ((employee.flags && HOURLY_FLAGS) &&
            (employee.age > 65)) {

    }
    // or this
    if (employee.isEligibleForFullBenefits()) {
    }
}
```

# Good Comments.

Some comments are necessary or beneficial. Keep in mind, however,
that the only truly good comment is the comment you found a way not
to write.

## 1. Legal Comments.

For example, copyright and authorship statements are necessary and reasonable
things to put into a comment at the start of each source file.

## 2. Informative Comments.

It is sometimes useful to provide basic information with a comment. For Example

```java
// format matched kk:mm:ss EEE, MMM dd, yyyy
Pattern timeMatcher = Pattern
                .compile("\\d*:\\d*:\\d* \\w*, \\w* \\d*, \\d*");
```

## 3. Explanation of Intent.

Sometimes a comment goes beyond just useful information about the implementation and
provides the intent behind a decision. For Example

```java
public int compareTo(Object o) {
    if (o instanceof WikiPagePath) {
        WikiPagePath p = (WikiPagePath) o;
        String compressedName = StringUtil.join(names, "");
        String compressedArgumentName = StringUtil.join(p.names, "");
        return compressedName.compareTo(compressedArgumentName);
    }
    return 1; // we are greater because we are the right type.
}
```

```java
public void testConcurrentAddWidgets() throws Exception {
    WidgetBuilder widgetBuilder =
            new WidgetBuilder(new Class[]{BoldWidget.class});
    String text = "'''bold text'''";
    ParentWidget parent =
            new BoldWidget(new MockWidgetRoot(), "'''bold text'''");
    AtomicBoolean failFlag = new AtomicBoolean();
    failFlag.set(false);

    //This is our best attempt to get a race condition
    //by creating large number of threads.
    for (int i = 0; i < 25000; i++) {
        WidgetBuilderThread widgetBuilderThread =
                new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
        Thread thread = new Thread(widgetBuilderThread);
        thread.start();
    }
    assertEquals(false, failFlag.get());
}
```

## 4. Clarification

Sometimes it is just helpful to translate the meaning of some obscure argument or return
value into something that’s readable. In general it is better to find a way to make that
argument or return value clear in its own right; but when its part of the standard library, or in
code that you cannot alter, then a helpful clarifying comment can be useful.

```java
public void testCompareTo() throws Exception {
    WikiPagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA.PageB");
    WikiPagePath b = PathParser.parse("PageB");
    WikiPagePath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB.PageB");
    WikiPagePath ba = PathParser.parse("PageB.PageA");

    assertTrue(a.compareTo(a) == 0); // a == a
    assertTrue(a.compareTo(b) != 0); // a != b
    assertTrue(ab.compareTo(ab) == 0); // ab == ab
    assertTrue(a.compareTo(b) == -1); // a < b
    assertTrue(aa.compareTo(ab) == -1); // aa < ab
    assertTrue(ba.compareTo(bb) == -1); // ba < bb
    assertTrue(b.compareTo(a) == 1); // b > a
    assertTrue(ab.compareTo(aa) == 1); // ab > aa
    assertTrue(bb.compareTo(ba) == 1); // bb > ba
}
```

## 5. Warning of Consequences

Sometimes it is useful to warn other programmers about certain consequences.
For example

```java
// Don't run unless you
// have some time to kill.
public void _testWithReallyBigFile() {
    writeLinesToFile(10000000);
    response.setBody(testFile);
    response.readyToSend(this);
    String responseString = output.toString();
    assertSubString("Content-Length: 1000000000", responseString);
    assertTrue(bytesSent > 1000000000);
}
```

```java
public static SimpleDateFormat makeStandardHttpDateFormat() {
    //SimpleDateFormat is not thread safe,
    //so we need to create each instance independently.
    SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
    df.setTimeZone(TimeZone.getTimeZone("GMT"));
    return df;
}
```

## 6. TODO Comments

It is sometimes reasonable to leave “To do” notes in the form of //TODO comments.

```java
//TODO-MdM these are not needed
// We expect this to go away when we do the checkout model
protected VersionInfo makeVersion() throws Exception {
    return null;
}
```

## 7. Amplification

A comment may be used to amplify the importance of something that may otherwise seem
inconsequential.

```java
public void method() {
    String listItemContent = match.group(3).trim();
    // the trim is real important. It removes the starting
    // spaces that could cause the item to be recognized
    // as another list.
    new ListItemWidget(this, listItemContent, this.level + 1);
    return buildList(text.substring(match.end()));
}

```

## 8. Javadocs in Public APIs.

If you are writing a public API, then you should certainly write good javadocs for it.
But keep in mind the rest of the advice in this chapter. Javadocs can be just as misleading,
nonlocal, and dishonest as any other kind of comment.

# Bad Comments

Most comments fall into this category. Usually they are crutches or excuses for poor code
or justifications for insufficient decisions, amounting to little more than the programmer
talking to himself.

## 1. Mumbling

If you decide to write a comment, then spend the time necessary to make sure it is the best
comment you can write.

## 2. Redundant Comments

*The comment is probably takes longer to read than the code itself*

```java
// Utility method that returns when this.closed is true. Throws an exception
// if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    if (!closed) {
        wait(timeoutMillis);
        if (!closed)
            throw new Exception("MockResponseSender could not be closed");
    }
}
```

## 3. Misleading Comments.

Sometimes, with all the best intentions, a programmer makes a statement in his comments
that isn’t precise enough to be accurate.

## 4. Mandate Comments

It is just plain silly to have a rule that says that every function must have a javadoc,
or every variable must have a comment.

*This clutter adds nothing*

```java
/**
 *
 * @param title The title of the CD
 * @param author The author of the CD
 * @param tracks The number of tracks on the CD
 * @param durationInMinutes The duration of the CD in minutes
 */
public void addCD(String title, String author,
                  int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = duration;
    cdList.add(cd);
}
```

## 5. Journal Comments.

Sometimes people add a comment to the start of a module every time they edit it. These
comments accumulate as a kind of journal, or log, of every change that has ever been
made. I have seen some modules with dozens of pages of these run-on journal entries.

```java
/* Changes (from 11-Oct-2001)
* --------------------------
* 11-Oct-2001 : Re-organised the class and moved it to new package
* com.jrefinery.date (DG);
* 05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate
* class (DG);
* 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate
* class is gone (DG); Changed getPreviousDayOfWeek(),
* getFollowingDayOfWeek() and getNearestDayOfWeek() to correct
* bugs (DG);
* 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
* 29-May-2002 : Moved the month constants into a separate interface
* (MonthConstants) (DG);
* 27-Aug-2002 : Fixed bug in addMonths() method, thanks to N???levka Petr (DG);
* 03-Oct-2002 : Fixed errors reported by Checkstyle (DG);
* 13-Mar-2003 : Implemented Serializable (DG);
* 29-May-2003 : Fixed bug in addMonths method (DG);
* 04-Sep-2003 : Implemented Comparable. Updated the isInRange javadocs (DG);
* 05-Jan-2005 : Fixed bug in addYears() method (1096282) (DG);
        ........................
 */
```

## 6. Noise Comments.

Sometimes you see comments that are nothing but noise. They restate the obvious and
provide no new information.

```java
/**
 * Default constructor.
 */
protected AnnualDateRule() {
}
```

## 7. Scary Noise

Javadocs can also be noisy. What purpose do the following Javadocs (from a well-known
open-source library) serve? Answer: nothing. They are just redundant noisy comments
written out of some misplaced desire to provide documentation.

```java
/** The name. */
private String name;
/** The version. */
private String version;
/** The licenceName. */
private String licenceName;
/** The version. */
private String info;
```

## 8. Don't Use a Comment When You Can Use a Function or a Variable

```java
public void method() {
    // does the module from the global list <mod> depend on the
    // subsystem we are part of?
    if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem())) {

    }

    // This could be rephrased without the comment as 
    ArrayList moduleDependees = smodule.getDependSubsystems();
    String ourSubSystem = subSysMod.getSubSystem();
    if (moduleDependees.contains(ourSubSystem)) {

    }
}
```

## 9. Closing Brace Comments

Sometimes programmers will put special comments on closing braces, as in Listing 4-6.
Although this might make sense for long functions with deeply nested structures, it serves
only to clutter the kind of small and encapsulated functions that we prefer. So if you find
yourself wanting to mark your closing braces, try to shorten your functions instead.

```java
public class wc {
    public static void main(String[] args) {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        String line;
        int lineCount = 0;
        int charCount = 0;
        int wordCount = 0;
        try {
            while ((line = in.readLine()) != null) {
                lineCount++;
                charCount += line.length();
                String words[] = line.split("\\W");
                wordCount += words.length;
            } //while
            System.out.println("wordCount = " + wordCount);
            System.out.println("lineCount = " + lineCount);
            System.out.println("charCount = " + charCount);
        } // try
        catch (IOException e) {
            System.err.println("Error:" + e.getMessage());
        } //catch
    } //main
}
```

## 10. Commented-Out Code.

Few practices are as odious as commenting-out code. Don't do this!
Because others who see that commented-out code won't have the courage
to delete it.

## 11. HTML Comments.

HTML in source code comments is an abomination, It makes the comments hard
to read in the one page where they should be easy to read.

## 12. Nonlocal Information.

If you must write a comment, then make sure it describe the code it appears near.
Don't offer systemwide information in the context of a local comment.

## 13. Too much Information.'

Don't put interesting historical discussions or irrelevant descriptions of details into
you comments.

## 14. In-obvious Connection.

Teh connection between a comment and the code it describes should be obvious.
It is a pity when a comment needs its own explanation.

## 15. Function Headers

Short functions don't need much description. A well-chosen name for a small function
that does one thing is usually better than a comment header.

## 16. Javadocs in Non-public Code.

As useful as javadocs are for public APIs, they are anathema to code
that is not intended for public consumption.

# Examples

**Problem**

```java
/**
 * This class Generates prime numbers up to user specified
 * maximum. The algorithm used is the Sieve of Eratosthenes.
 *
 * <p>
 *     Eratosthenes of Cyrene, b. c. 276BC, Cyrene, Libya -- 
 *     d. c. 194, Alexandria. The first man to calculate the 
 *     circumference of the Earth. Also know for working on 
 *     calendars with leap years and ran the library at Alexandria,
 * </p>
 * The algorithm is quite simple. Given an array of integers
 * starting at 2. Cross out all multiples of 2. Find the next
 * uncrossed integer, and cross out all of its multiples.
 * Repeat untilyou have passed the square of the maximum
 * value.
 *
 * @author Alphonse
 * @version 13 Feb atp
 */
public class GeneratePrimes {

    /**
     * @param maxValue is the generation limit
     */
    public static int[] generatePrimes(int maxValue) {
        if (maxValue >= 2) { // the only valid case

            // declarations
            int s = maxValue + 1; // size of array
            boolean[] f = new boolean[s];
            int i;

            // initialize array to true
            for (i = 0; i < s; i++) {
                f[i] = true;
            }

            // get rid of know non-primes
            f[0] = f[1] = false;

            //sieve
            int j;
            for (i = 2; i < Math.sqrt(s) + 1; i++) {
                if (f[i]) { // if i is uncrossed, cross its multiples
                    for (j = 2 * i; j < s; s += i) {
                        f[j] = false; // multiple is not prime
                    }
                }
            }

            // how many primes are there?
            int count = 0;
            for (i = 0; i < s; i++) {
                if (f[i]) {
                    count++; // bump count
                }
            }

            int[] primes = new int[count];

            // move the primes into the result
            for (i = 0, j = 0; i < s; i++) {
                if (f[i]) {// if prime
                    primes[j++] = i;
                }
            }


            return primes; // return primes
        } else { // maxvalue < 2
            return new int[0]; //return null array if bad input.
        }
    }
}
```

**Solution**

```java
/**
 * This class Generates prime numbers up to a user specified
 * maximum. The algorithm used is the Sieve of Eratosthenes.
 * Given an array of integers starting at 2: 
 * Find the first uncrossed integer, and cross otu all its
 * multiples. Repeat until there are no more multiples 
 * in the array.
 */
public class PrimeGenerator {
    private static boolean[] crossedOut;
    private static int[] result;
    
    public static int[] generatePrimes(int maxValue){
        if (maxValue < 2){
            return new int[0];
        } else {
            uncrossIntegerUpTo(maxValue);
            crossOutMultiples();
            putUncrossedIntegersIntoResult();
            return result;
        }
    }
    
    private static void uncrossIntegerUpTo(int maxValue){
        crossedOut = new boolean[maxValue + 1];
        for (int i = 2; i < crossedOut.length; i++){
            crossedOut[i] = false;
        }
    }
    
    private static void crossOutMultiples() {
        int limit = determineIterationLimit();
        for (int i = 2; i <= limit; i++) {
            if (notCrossed(i)){
                crossOutMultiplesOf(i);
            }
        }
    }
    
    private static int determineIterationLimit(){
        // Every multiple in the array has a prime factor that
        // is less than or equal to the root of the array size,
        // so we don't have to cross out multiples numbers
        // larger than that root.
        
        double iterationLimit = Math.sqrt(crossedOut.length);
        return (int)   iterationLimit;
    }
    
    private static void crossOutMultiplesOf(int i) {
        for (int multiple = 2 * i; 
        multiple < crossedOut.length; multiple += i){
            crossedOut[multiple] = true;
        }
    }
    
    private static boolean notCrossed(int i){
        return crossedOut[i] = false;
    }
    
    private static void putUncrossedIntegersIntoResult(){
        result = new int[numberOfUncrossedIntegers()];
        for (int j = 0, i = 2; i < crossedOut.length; i++){
            if (notCrossed(i)){
                result[j++] = i;
            }
        }
    }
    
    private static int numberOfUncrossedIntegers(){
        int count = 0;
        for (int i = 2; i < crossedOut.length; i++){
            if (notCrossed(i))
                count++;
        }
        return count;
    }
    
}
```
