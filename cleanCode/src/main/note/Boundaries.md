# Boundaries

We seldom control all the software in our systems. Sometimes we buy third-party packages
or use open source. Other times we depend on teams in our own company to produce
components or subsystems for us. Somehow we must cleanly integrate this foreign code
with our own.

## Using Third-Party Code

We are not suggesting that every use of Map be encapsulated in this form. Rather, we
are advising you not to pass Maps (or any other interface at a boundary) around your
system. If you use a boundary interface like Map, keep it inside the class, or close family
of classes, where it is used. Avoid returning it from, or accepting it as an argument to,
public APIs.

```java
// Instead of using this 

import java.util.HashMap;
import java.util.Map;

Map<String, Sensor> sensorMap = new HashMap<>();
Sensor s = sensorMap.get(sensorId);
```

```java
import java.util.HashMap;
import java.util.Map;

// You can use this, so can protect boundary. which map options is you want to expose.
public class Sensors {
    private Map<String, Sensors> senors = new HashMap<>();

    public Sensors getById(String id) {
        return senors.get(id);
    }
}
```

## Exploring and Learning Boundaries.

To learn and use third party code we should test third party api that we are going to use
out code. It's called learning test.

## Learning Tests Are Better Than Free

Whether you need the learning provided by the learning tests or not, a clean boundary
should be supported by a set of outbound tests that exercise the interface the same way the
production code does. Without these boundary tests to ease the migration, we might be
tempted to stay with the old version longer than we should.

## Using Code That Does Not Yet Exist

To use code that does not exist. interface and adapter pattern.

## Clean Boundaries

Interesting things happen at boundaries. Change is one of those things. Good software
designs accommodate change without huge investments and rework. When we use code
that is out of our control, special care must be taken to protect our investment and make
sure future change is not too costly.
