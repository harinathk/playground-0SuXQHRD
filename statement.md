The arrival of Java 9 brings many new features to Java’s Collections API, one of which being collection factory methods, which add syntactic sugar for creating small, unmodifiable Collection instances using new convenience factory methods as per JEP 269.

In this article, we will discuss their usage and implementation details.

# Motivation

Let’s start by looking at the problem that this is trying to solve by instantiation a list with a few String values:



```java runnable
// { autofold
import java.util.List;
import java.util.ArrayList;
public class Main {

public static void main(String[] args) {
// }

List<String> units = new ArrayList<>();
units.add(“One”);
units.add(“Two”);
units.add(“Three”);
units.add(“Four”);
units = Collections.unmodifiableList(units);
//{ autofold
}

}
//}
```

# Advanced usage

If you want a more complex example (external libraries, viewers...), use the [Advanced Java template](https://tech.io/select-repo/385)
