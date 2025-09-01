# Functions

### Rule for Function

## 1. Small!

The first rule of functions is that they should be small.
The second rule of functions is that they should be smaller
than that. Lines should not be 150 characters long. Functions
should not be 100 lines long. Functions should hardly ever be
20 lines long.

## 2.Blocks and Indenting

This implies that the blocks within if statements, else statements, while statements, and
so on should be one line long. Probably that line should be a function call. Not only does
this keep the enclosing function small, but it also adds documentary value because the
function called within the block can have a nicely descriptive name.

## 3. Functions should do one thing, They should do it will, They should do it only.

A way to know that a function is doing more than “one thing” is if you can
extract another function from it with a name that is not merely a restatement of its implementation.

## 4. One Level of Abstraction per Function.

In order to make sure our functions are doing “one thing,” we need to make sure that the
statements within our function are all at the same level of abstraction.

## 5. Reading Code from Top to Bottom: The Stepdown Rule

We want the code to read like a top-down narrative. We want every function to be followed by those
at the next level of abstraction so that we can read the program, descending
one level of abstraction at a time as we read down the list of functions. I call this The Stepdown Rule.

## 6. Switch Statements.

It’s hard to make a small switch statement. Even a switch statement with only two cases is
larger than I’d like a single block or function to be. They can be tolerated if they appear
only once, are used to create polymorphic objects, and are hidden behind an inheritance relationship
so that the rest of the system can’t see them.

```java
/*
There are several problems with this function. First, it’s large, and when new
employee types are added, it will grow. Second, it very clearly does more than one thing.
Third, it violates the Single Responsibility Principle7 (SRP) because there is more than one
reason for it to change. Fourth, it violates the Open Closed Principle8 (OCP) because it
must change whenever new types are added. But possibly the worst problem with this
function is that there are an unlimited number of other functions that will have the same
structure. 
 */
public Money calculatePay(Employee employee) {
    switch (employee.type) {
        case COMMISSIONED:
            return calculateCommisionedPay(employee);
        ommissionedPay(employee);
        case HOURLY:
            return calculateHourlyPay(employee);
        case SALARIED:
            return calculateSalariedPay(employee);
        default:
            throw new IllegaArgumentException();
    }
}
```

**Solutions**

```java
public interface Employee {
    boolean isPayday();

    Money calculatePay();

    void deliveryPay(Money pay);
}

public interface EmployeeFactory {
    Employee makeEmployee(EmployeeRecord record) throws InvalidEmployeeTyepe;
}

public class EmployeeFactoryImpl implements EmployeeFactory {
    @java.lang.Override
    public Employee makeEmployee(EmployeeRecord record) throws InvalidEmployeeTyepe {
        switch (record.type) {
            case COMMISSIONED:
                return new CommisionEmployee(record);
            ommissionedPay(employee);
            case HOURLY:
                return new HourlyEmployee(record);
            case SALARIED:
                return new SalariedEmployee(record);
            default:
                throw new InvalidEmployeeTyepe();
        }
    }
}
```

## 7. Use Descriptive Names.

Don't be afraid to make a name long. A long descriptive name is better than a short
enigmatic name. A long descriptive name is better than a long descriptive comment.

## 8. Function Arguments

The ideal number of arguments for a function is
zero (niladic). Next comes one (monadic), followed
closely by two (dyadic). Three arguments (triadic)
should be avoided where possible. More than three
(polyadic) requires very special justification—and
then shouldn’t be used anyway.

## 9. Have No Side Effects

Side effects are lies. In any case if your function must change
the state of something, have it change the state of its owning object.
*Do not break Function should do one thing principle*

```java
public class UserValidator {
    private Cryptographer cryptographer;

    public boolean checkPassword(String userName, String password) {
        User user = UserGetway.findByName(userName);
        if (user != null) {
            String codedPhrase = user.getPhraseEncodeByPassword();
            String phrase = cryptographer.decrypt(codedPhrase, password);
            if ("Valid Password".equals(phrase)) {
                Session.intialize(); // side effect
                return true;
            }
        }
        return false;
    }
}
```

## 10. Command Query Separation

Functions should either do something or answer something, but not both.
Either your function should change the state of an object, or it should
return some information about that object.

## 11. Prefer Exceptions to Returning Error Codes.

Returning error codes from command functions is subtle violation of command
query separation.

## 12. Extract Try/Catch Blocks

Try/catch blocks are ugly in their own right. They confuse the structure of the code and
mix error processing with normal processing. So it is better to extract the bodies of the try
and catch blocks out into functions of their own.

```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    } catch (Exception e) {
        logError(e);
    }
}

private void deletePageAndAllReferences(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
    logger.log(e.getMessage());
}
```

## 13. Error Handling Is One Thing

Functions should do one thing. Error handing is one thing. Thus, a function that handles
errors should do nothing else. This implies (as in the example above) that if the keyword
try exists in a function, it should be the very first word in the function and that there
should be nothing after the catch/finally blocks.

## 14. Don't Repeat Yourself.

Duplication may be the root of all evil in software. Many principles and practices have
been created for the purpose of controlling or eliminating it.







