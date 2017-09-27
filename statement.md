The arrival of Java 9 brings many new features to Java’s Collections API, one of which being collection factory methods, which add syntactic sugar for creating small, unmodifiable Collection instances using new convenience factory methods as per JEP 269.

In this article, we will discuss their usage and implementation details.

# Motivation

Let’s start by looking at the problem that this is trying to solve by instantiation a list with a few String values:

```java runnable
// { autofold
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;
public class Main {

public static void main(String[] args) {
// }
	List<String> units = new ArrayList<>();
		units.add("One");
		units.add("Two");
		units.add("Three");
		units.add("Four");
		units = Collections.unmodifiableList(units);
	System.out.println(units);
//{ autofold
}

}
//}
```
The same thing we can write using Java 5's Arrays.asList() like below:

```java runnable
// { autofold
import java.util.Collections;
import java.util.List;
import java.util.Arrays;
public class Main {

public static void main(String[] args) {
// }
    List<String> units= Arrays.asList("One", "Two", "Three", "Four");
    units = Collections.unmodifiableList(units);
	System.out.println(units);
//{ autofold
}

}
//}
```
This is much better than the above example, but when you try to add an element to the List, it will throw the UnsupportedOperationException. And List can’t be mutated. If we want to mutate the value, we need to iterate and change the values.

Still, this List creation is better than the constructor initialization. If we want to use Set, we may need to do like below:

```java runnable
// { autofold
import java.util.Collections;
import java.util.List;
import java.util.Arrays;
import java.util.Set;
import java.util.HashSet;
public class Main {

public static void main(String[] args) {
// }
    Set<String> units = new HashSet<>(Arrays.asList("One", "Two", "Three", "Four"));
    units = Collections.unmodifiableSet(units);
	System.out.println(units);
//{ autofold
}

}
//}
```
By using Java 8 Streams:

```java runnable
// { autofold
import java.util.Collections;
import java.util.Stream;
public class Main {

public static void main(String[] args) {
// }
   Stream.of("One", "Two", "Three", "Four")
          .collect(collectingAndThen(toSet(), Collections::unmodifiableSet));
	System.out.println(units);
//{ autofold
}

}
//}
```
The Java 8 version, though, is a one-line expression. This method can’t be used to create a Map, and it is still fairly verbose.

Creating such a small immutable Collection in Java using the above approach is very verbose.

#Collection Factories

By using Java 9 Collection factories, we can create immutable collections.

#List and Set

For example, if we want to create lists, we can do this:

```java runnable
// { autofold
import java.util.Collections;
import java.util.List;
public class Main {

public static void main(String[] args) {
// }
    List<String> list0 = List.of(); // List0
    List<String> list1 = List.of("One"); // List1
    List<String> list2 = List.of("One", "Two"); // List2
    List<String> list3 = List.of("One", "Two", "Three"); // List3
    List<String> list4 = List.of("One", "Two", "Three", "Four"); // ListN
    System.out.println(list0);
    System.out.println(list1);
    System.out.println(list2);
    System.out.println(list3);
    System.out.println(list4);
    System.out.println(list5);
    
//{ autofold
}

}
//}
```
We can create sets in a similar way:

```java runnable
// { autofold
import java.util.Collections;
import java.util.Set;
public class Main {

public static void main(String[] args) {
// }
    Set<String> set0 = Set.of(); // Set0
    Set<String> set1 = Set.of("One"); // Set1
    Set<String> set2 = Set.of("One", "Two"); // Set2
    Set<String> set3 = Set.of("One", "Two", "Three"); // Set3
    Set<String> set4 = Set.of("One", "Two", "Three" "Four"); // SetN
    
    System.out.println(set0);
    System.out.println(set1);
    System.out.println(set2);
    System.out.println(set3);
    System.out.println(set4);

//{ autofold
}

}
//}
```
The signature and characteristics of List and Set factory methods are the same:

static <E> List<E> of(E e1, E e2, E e3)
static <E> Set<E>  of(E e1, E e2, E e3)

As you can see, it’s very simple, short, and concise.

In the example, we have used the method that takes one to four elements as parameters and returns a List /Set of size 4. But there are 12 overloaded versions of this method and 11 with 0-10 parameters and one with var-args so that there is no fixed limit on the collection size.

#Map

The signature of a Map factory method is:
static <K,V> Map<K,V> of(K k1, V v1, K k2, V v2, K k3, V v3)

Example:
Map<String, String> map = Map.of("One", "1", "Two", "2", "Three", "3");

Similar to the List and Set, the of method is overloaded to have 0-10 key-value pairs.

In the case of Map, there is a different method for more than 10 key-value pairs:

static <K,V> Map<K,V> ofEntries(Map.Entry<? extends K,? extends V>... entries)

Example:

Map<String, String> map = Map.ofEntries(
  new AbstractMap.SimpleEntry<>("One", "1"),
  new AbstractMap.SimpleEntry<>("Two", "2"),
  new AbstractMap.SimpleEntry<>("Three", "3"));
  
#Characteristics

The collections created using the factory methods are not the most commonly used implementations. These implementations are internal and their constructors are not made public.

Immutable: Elements cannot be added or removed. Calling any mutator method will always cause UnsupportedOperationException to be thrown

No null Element Allowed: Attempts to create them with null elements result in NullPointerException. In the case of List and Set, no elements can be null. In the case of a Map, neither keys nor values can be null.

Value-Based Instances: If we create Lists with the same values, they may or may not refer to the same object on the heap.

Serialization:  They are serializable if all elements are serializable.

Iteration Order: The iteration order of elements is unspecified and is subject to change.

#Conclusion

Collection factories add some easier syntax to perform a common operation. They give us most of the benefits without any language changes. The addition of these factory methods in Java 9 provides Immutable collections.  

Thank you.

This article is originally published at harinathk.com
