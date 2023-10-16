# JavaSE-Supplier

In Java, a Supplier is a functional interface part of the java.util.function package. 

It represents a supplier of results, meaning it doesn't take any arguments but produces a result.

The result is obtained by calling the get() method.

```java
import java.util.function.Supplier;

public class SupplierExample {
    public static void main(String[] args) {
        // Example 1: Supplier without lambda expression
        Supplier<String> supplier1 = new Supplier<String>() {
            @Override
            public String get() {
                return "Hello, Supplier!";
            }
        };
        System.out.println(supplier1.get());

        // Example 2: Supplier with lambda expression
        Supplier<String> supplier2 = () -> "Hello, Lambda Supplier!";
        System.out.println(supplier2.get());
    }
}
```

In this example, Supplier is a generic interface, and you specify the type of result it will supply (in this case, String). 

The get() method is called on the supplier object to obtain the result.

In the second example, a lambda expression is used to create a more concise representation of the Supplier. 

The lambda expression () -> "Hello, Lambda Supplier!" is equivalent to the anonymous class in the first example.

In both cases, when you call get() on the Supplier, it produces the specified result.

It's commonly used in scenarios where you need to defer the computation of a value, especially when the computation might be expensive, and you only want to perform it when necessary.


# More examples to showcase the use of java.util.function.Supplier:

## Random Number Generator:

```java
import java.util.Random;
import java.util.function.Supplier;

public class RandomNumberGenerator {
    public static void main(String[] args) {
        Supplier<Integer> randomIntegerSupplier = () -> new Random().nextInt(100);
        System.out.println("Random Number: " + randomIntegerSupplier.get());
    }
}
```

## Date Supplier:

```java
import java.time.LocalDateTime;
import java.util.function.Supplier;

public class DateSupplier {
    public static void main(String[] args) {
        Supplier<LocalDateTime> currentDateTimeSupplier = LocalDateTime::now;
        System.out.println("Current Date and Time: " + currentDateTimeSupplier.get());
    }
}
```

## Default Value Supplier:

```java
import java.util.function.Supplier;

public class DefaultValueSupplier {
    public static void main(String[] args) {
        String nullString = null;
        Supplier<String> defaultValueSupplier = () -> {
            if (nullString == null) {
                return "Default Value";
            } else {
                return nullString;
            }
        };
        System.out.println("Value: " + defaultValueSupplier.get());
    }
}
```

## Lazy Initialization:

```java
import java.util.function.Supplier;

public class LazyInitialization {
    private Supplier<String> expensiveInitialization = () -> {
        // Perform expensive initialization here
        return "Initialized Value";
    };

    public String getValue() {
        return expensiveInitialization.get();
    }

    public static void main(String[] args) {
        LazyInitialization lazyObject = new LazyInitialization();
        // Expensive initialization is performed only when needed
        System.out.println("Value: " + lazyObject.getValue());
    }
}
```

These examples illustrate various use cases for Supplier, from generating random numbers to lazy initialization of values. 

It's a versatile interface that can be applied in different scenarios where deferred or lazy evaluation is beneficial.

# More advance topics about "Supplier" in Java

Let's delve into more advanced topics related to Supplier in Java:

## Method References:
Instead of using lambda expressions, you can leverage method references when working with Supplier. For example:

java
Copy code
import java.util.function.Supplier;

public class MethodReferenceExample {
    public static String generateString() {
        return "Hello, Method Reference!";
    }

    public static void main(String[] args) {
        Supplier<String> supplier = MethodReferenceExample::generateString;
        System.out.println(supplier.get());
    }
}
Here, MethodReferenceExample::generateString refers to the static method generateString in the class.

Chaining Suppliers:
You can chain multiple Supplier instances using the andThen method. It allows you to create a pipeline of operations:

java
Copy code
import java.util.function.Supplier;

public class ChainingSuppliers {
    public static void main(String[] args) {
        Supplier<String> firstSupplier = () -> "Hello";
        Supplier<String> secondSupplier = () -> " World";

        Supplier<String> chainedSupplier = firstSupplier.andThen(secondSupplier);

        System.out.println(chainedSupplier.get()); // Outputs: Hello World
    }
}
The chainedSupplier will first get the result from firstSupplier and then apply secondSupplier to the result.

Memoization:
Memoization is a technique where you cache the result of a costly operation and return the cached result when the same inputs occur again. You can implement memoization using a Supplier:

java
Copy code
import java.util.function.Supplier;

public class MemoizationExample {
    private Supplier<Integer> expensiveComputation = () -> {
        int result = performExpensiveComputation();
        // Cache the result
        expensiveComputation = () -> result;
        return result;
    };

    private int performExpensiveComputation() {
        // Simulating an expensive computation
        return 42;
    }

    public int getResult() {
        return expensiveComputation.get();
    }

    public static void main(String[] args) {
        MemoizationExample memoizationExample = new MemoizationExample();

        System.out.println("Result: " + memoizationExample.getResult()); // Performs the computation
        System.out.println("Result: " + memoizationExample.getResult()); // Returns the cached result
    }
}
In this example, the expensive computation is performed only once, and subsequent calls return the cached result.

These advanced topics showcase the flexibility and power of the Supplier interface in Java, from method references to creating complex pipelines and implementing optimization techniques like memoization.
