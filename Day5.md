# Collections and Wrapper Class

![Collections in Java - javatpoint](https://static.javatpoint.com/images/java-collection-hierarchy.png)





---

## Wrapper Class

A Wrapper class is a class whose object wraps or contains primitive data types. When we create an object to a wrapper
class, it contains a field and in this field, we can store primitive data types. In other words, we can wrap a primitive
value into a wrapper class object.

**Need of Wrapper Classes**

1. They convert primitive data types into objects. Objects are needed if we wish to modify the arguments passed into a
   method (because primitive types are passed by value).
2. The classes in java.util package handles only objects and hence wrapper classes help in this case also.
3. Data structures in the Collection framework, such as [ArrayList](https://www.geeksforgeeks.org/arraylist-in-java/)
   and [Vector](https://www.geeksforgeeks.org/vector-vs-arraylist-java/), store only objects (reference types) and not
   primitive types.
4. An object is needed to support synchronization in multithreading.

![Wrapper-Class-in-Java](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20200806191733/Wrapper-Class-in-Java.png)

**Autoboxing:** Automatic conversion of primitive types to the object of their corresponding wrapper classes is known as
autoboxing. For example – conversion of int to Integer, long to Long, double to Double etc.

**Unboxing:** It is just the reverse process of autoboxing. Automatically converting an object of a wrapper class to its
corresponding primitive type is known as unboxing. For example – conversion of Integer to int, Long to long, Double to
double, etc.

# Autoboxing and Unboxing

*Autoboxing* is the automatic conversion that the Java compiler makes between the primitive types and their
corresponding object wrapper classes. For example, converting an `int` to an `Integer`, a `double` to a `Double`, and so
on. If the conversion goes the other way, this is called *unboxing*.

Here is the simplest example of autoboxing:

```
Character ch = 'a';
```

Consider the following code:

```
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(i);
```

Although you add the `int` values as primitive types, rather than `Integer` objects, to `li`, the code compiles.
Because `li` is a list of `Integer` objects, not a list of `int` values, you may wonder why the Java compiler does not
issue a compile-time error. The compiler does not generate an error because it creates an `Integer` object from `i` and
adds the object to `li`. Thus, the compiler converts the previous code to the following at runtime:

```
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(Integer.valueOf(i));
```

Converting a primitive value (an `int`, for example) into an object of the corresponding wrapper class (`Integer`) is
called autoboxing. The Java compiler applies autoboxing when a primitive value is:

- Passed as a parameter to a method that expects an object of the corresponding wrapper class.
- Assigned to a variable of the corresponding wrapper class.

Consider the following method:

```
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i: li)
        if (i % 2 == 0)
            sum += i;
        return sum;
}
```

Because the remainder (`%`) and unary plus (`+=`) operators do not apply to `Integer` objects, you may wonder why the
Java compiler compiles the method without issuing any errors. The compiler does not generate an error because it invokes
the `intValue` method to convert an `Integer` to an `int` at runtime:

```
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i : li)
        if (i.intValue() % 2 == 0)
            sum += i.intValue();
        return sum;
}
```

Converting an object of a wrapper type (`Integer`) to its corresponding primitive (`int`) value is called unboxing. The
Java compiler applies unboxing when an object of a wrapper class is:

- Passed as a parameter to a method that expects a value of the corresponding primitive type.
- Assigned to a variable of the corresponding primitive type.

The [`Unboxing`](https://docs.oracle.com/javase/tutorial/java/data/examples/Unboxing.java) example shows how this works:

```
import java.util.ArrayList;
import java.util.List;

public class Unboxing {

    public static void main(String[] args) {
        Integer i = new Integer(-8);

        // 1. Unboxing through method invocation
        int absVal = absoluteValue(i);
        System.out.println("absolute value of " + i + " = " + absVal);

        List<Double> ld = new ArrayList<>();
        ld.add(3.1416);    // Π is autoboxed through method invocation.

        // 2. Unboxing through assignment
        double pi = ld.get(0);
        System.out.println("pi = " + pi);
    }

    public static int absoluteValue(int i) {
        return (i < 0) ? -i : i;
    }
}
```

The program prints the following:

```
absolute value of -8 = 8
pi = 3.1416
```

Autoboxing and unboxing lets developers write cleaner code, making it easier to read. 
