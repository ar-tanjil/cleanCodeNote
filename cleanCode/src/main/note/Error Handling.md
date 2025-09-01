# Error Handling

## Use Exceptions Rather Than Return Codes

To handle or report error throw exception. Don't return
error flag or error code.

## Write Your Try-Catch-Finally Statement First.

Your catch has to leave your program in a
consistent state, no matter what happens in the try. For this reason it is good practice to
start with a try-catch-finally statement when you are writing code that could throw
exceptions.

## Use Unchecked Exceptions

Checked exceptions can sometimes be useful if you are writing a critical library: You
must catch them. But in general application development the dependency costs outweigh
the benefits.

## Provide Context with Exceptions

Create informative error messages and pass them along with your exceptions.
Mentions the operations that failed and the type of failure. If you are logging
in your applications, pass along enough information to be ale to log the error
in you catch.

## Define Exception Classes in Terms of a Caller's Needs.

Wrapping third-party APIs is a best practice. Its minimize your dependencies.
Often a single exception class is fine for a particular area of code. The information
sent with the exception can distinguish the errors. Use different classes only if there are
times when you want to catch one exception and allow the other one to pass through.

```java
// Poor exceptions

public void method() {
    ACMEPort port = new ACMEPort(12);
    try {
        port.open();
    } catch (DeviceResponseException e) {
        reportPortError(e);
        logger.log("Device response exception", e);
    } catch (ATM1212UnlockedException e) {
        reportPortError(e);
        logger.log("Unlock exception", e);
    } catch (GMXError e) {
        reportPortError(e);
        logger.log("Device response exception");
    } finally {
        //-- 
    }
}
```

```java
public void method() {
    LocalPort port = new LocalPort(12);
    try {
        port.open();
    } catch (PortDeviceFailure e) {
        reportError(e);
        logger.log(e.getMessage(), e);
    } finally {
        // --
    }
}
```

```java
public class LocalPort {

    private ACMEPort innerPort;

    public LocalPort(int portNumber) {
        innerPort = new ACMEPort(portNumber);
    }

    public void open() {
        try {
            innerPort.open();
        } catch (DeviceResponseException e) {
            throw new PortDeviceFailure(e);
        } catch (ATM1212UnlockedException e) {
            throw new PortDeviceFailure(e);
        } catch (GMXError e) {
            throw new PortDeviceFailure(e);
        }
    }

}
```

## Define the Normal Flow

Use SPECIAL CASE PATTERN to minimize exceptions. You create a class or configure an
object so that it handles a special case for you. When you do, the client code doesn’t have
to deal with exceptional behavior. That behavior is encapsulated in the special case object.

## Don't Return Null

Returning null invite errors. If you are tempted to return null from
a method, consider throwing an exception or returning a SPECIAL CASE object instead. If
you are calling a null-returning method from a third-party API, consider wrapping that
method with a method that either throws an exception or returns a special case object.

```java
// Bad code
public void registerItem(Item item) {
    if (item != null) {
        ItemRegistry registry = persistentStore.getItemRegistry();
        if (registry != null) {
            Item existing = registry.getItem(item.getId);
            if (existing.getBillingPeriod().hasRetailOwner()) {
                existing.register(item);
            }
        }
    }
}
```

## Don't Pass Null

Returning null from methods is bad, but passing null into methods is worse.
Unless you are working with an API which expects you to pass null, you should
avoid passing null in your code whenever possible.
In most programming languages there is no good way to deal with a null that is
passed by a caller accidentally. Because this is the case, the rational approach is to forbid
passing null by default. When you do, you can code with the knowledge that a null in an
argument list is an indication of a problem, and end up with far fewer careless mistakes.