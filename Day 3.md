### HashMap and LinkedHashMap

- Hashmap implements Map interface
- Internally hasmap uses hashing of the keys. keys must have consistent implementations of hashCode() and equals()
- LinkedHashMap is very similar to HashMap, but it adds awareness to the order at which items are added (or accessed),
  so the iteration order is the same as insertion order (or access order, depending on construction parameters).
- Hashmap does not preserve the order of insertion of entries into the map
- `HashMap` has multiple buckets or bins which contain a head reference to a singly linked list. That means there would
  be as many linked lists as there are buckets. Initially, it has a bucket size of 16 which grows to 32 when the number
  of entries in the map crosses the 75%. (That means after inserting in 12 buckets bucket size becomes 32)

Note: Java **HashMap** is not thread-safe and hence it should not be used in multithreaded application. For the
multi-threaded application, we should use **ConcurrentHashMap** class.

#### Working of a hashmap

- HashMap uses
  its [static inner class ](https://medium.com/javarevisited/do-you-know-nested-and-inner-classes-in-java-cc5647f46e07)`**Node<K,V>**` for storing map entries. That means each entry in hashMap is a `**
  Node**`. Internally HashMap uses a **hashCode** of the key Object and this hashCode is further used by the **hash
  function** to find the index of the bucket where the new entry can be added.
- HashMap uses multiple buckets and each bucket points to a `**Singly Linked List**` where the entries (nodes) are
  stored.
- Once the bucket is identified by the hash function using hashcode, then hashCode is used to check if there is already
  a key with the same hashCode or not in the bucket(singly linked list).
- If there already exists a key with the **same** **hashCode**, then the `**equals()**` method is used on the `**keys**`
  . If the equals method returns **true**, that means there is already a node with the same key and hence the value
  against that key is **overwritten** in the entry(node), otherwise, a new node is created and added to this Singly
  Linked List of that bucket.
- If there is no key with the same hashCode in the bucket found by the hash function then the new Node is added into the
  bucket found

![img](https://miro.medium.com/max/2058/1*k4ilVbqzXph8B_km184hMA.png)





---

### Working of linkedHashMap

- linkedhashmap preserves the order of insertion by maintaining reference to the before and after nodes.
- it internally uses a doubly linked list

> ```java
>static class Entry extends HashMap.Entry{
>  Entry before, after;  
>  Entry(int hash, K key, V value, HashMap.Entry next){
>   super(hash,key,value,next);
>  }
> }
>```
>
>

![Java LinkedHashMap Internal Implementation | by Anmol Sehgal | Medium](https://miro.medium.com/max/1012/1*BD8et6WM_o0XIqypGkf4HA.png)

Consider we have created a LinkedHashMap by using default constructor and inserted 4
elements[(1,"Nikhil"),(2,"ranjan"),(3,"Oracle"),(4,"java")]. Following diagram depicts how it will be stored
internally :(Assuming index is calculated and it comes out as 5 for key 1, 9 for key 2, and so on)

![Graphical depiction of how elements are stored in array and linkedlist](https://3.bp.blogspot.com/-L9qpWyF0cso/VQIAVLo45GI/AAAAAAAAINI/fTIIvyzIC1s/s1600/LHSp.JPG)



---

### hashCode() and Equals

The general contract of `hashCode` is:

- Whenever it is invoked on the same object more than once during an execution of a Java application, the `hashCode`
  method must consistently return the same integer, provided no information used in `equals` comparisons on the object
  is modified. This integer need not remain consistent from one execution of an application to another execution of the
  same application.
- If two objects are equal according to the `equals(Object)` method, then calling the `hashCode` method on each of the
  two objects must produce the same integer result.
- It is *not* required that if two objects are unequal according to
  the [`equals(java.lang.Object)`](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object))
  method, then calling the `hashCode` method on each of the two objects must produce distinct integer results. However,
  the programmer should be aware that producing distinct integer results for unequal objects may improve the performance
  of hash tables.


- Indicates whether some other object is "equal to" this one.

The `equals` method implements an equivalence relation on non-null object references:

- It is *reflexive*: for any non-null reference value `x`, `x.equals(x)` should return `true`.
- It is *symmetric*: for any non-null reference values `x` and `y`, `x.equals(y)` should return `true` if and only
  if `y.equals(x)` returns `true`.
- It is *transitive*: for any non-null reference values `x`, `y`, and `z`, if `x.equals(y)` returns `true`
  and `y.equals(z)` returns `true`, then `x.equals(z)` should return `true`.
- It is *consistent*: for any non-null reference values `x` and `y`, multiple invocations of `x.equals(y)` consistently
  return `true` or consistently return `false`, provided no information used in `equals` comparisons on the objects is
  modified.
- For any non-null reference value `x`, `x.equals(null)` should return `false`.

The `equals` method for class `Object` implements the most discriminating possible equivalence relation on objects; that
is, for any non-null reference values `x` and `y`, this method returns `true` if and only if `x` and `y` refer to the
same object (`x == y` has the value `true`).

Note that it is generally necessary to override the `hashCode` method whenever this method is overridden, so as to
maintain the general contract for the `hashCode` method, which states that equal objects must have equal hash codes.



---

### String Pool

**String pool** is nothing but a storage area in [Java heap](https://www.javatpoint.com/java-heap) where string literals
stores. It is also known as **String Intern Pool** or **String Constant Pool**. It is just like object allocation. By
default, it is empty and privately maintained by the **[Java String](https://www.javatpoint.com/java-string)** class.
Whenever we create a string the string object occupies some space in the heap memory. Creating a number of strings may
increase the cost and memory too which may reduce the performance also.

The JVM performs some steps during the initialization of string literals that increase the performance and decrease the
memory load. To decrease the number of String objects created in the JVM the String class keeps a pool of strings.

When we create a string literal, the JVM first check that literal in the String pool. If the literal is already present
in the pool, it returns a reference to the pooled instance. If the literal is not present in the pool, a new String
object takes place in the String pool.

![What is Java String Pool? - JournalDev](https://www.journaldev.com/wp-content/uploads/2012/11/String-Pool-Java1.png)

- When we create a String object using the new() operator, it always creates a new object in heap memory. On the other
  hand, if we create an object using \*String\* literal syntax e.g. “Baeldung”, it may return an existing object from
  the String pool, if it already exists. Otherwise, it will create a new String object and put in the string pool for
  future re-use.

- At a high level, both are the *String* objects, but the main difference comes from the point that *new()* operator
  always creates a new *String* object. Also, when we create a *String* using literal – it is interned.

- the JVM can optimize the amount of memory allocated for them by **storing only one copy of each literal \*String\* in
  the pool**. This process is called *interning*

- **If found, the Java compiler will simply return a reference to its memory address, without allocating additional
  memory.**

> String first = "Baeldung";
>
>  String second = "Baeldung";
>
>  System.out.println(first == second); // True



> String third = new String("Baeldung");
>
>String fourth = new String("Baeldung");
>
>System.out.println(third == fourth); // False

- We can manually intern a *String* in the Java String Pool by calling the *intern()* method on the object we want to
  intern.

- Manually interning the *String* will store its reference in the pool, and the JVM will return this reference when
  needed.

```java
String constantString="interned Baeldung";
        String newString=new String("interned Baeldung");

        assertThat(constantString).isNotSameAs(newString);

        String internedString=newString.intern();

        assertThat(constantString)
        .isSameAs(internedString);
```

- Before Java 7, the JVM **placed the Java String Pool in the PermGen space, which has a fixed size — it can't be
  expanded at runtime and is not eligible for garbage collection**.

- The risk of interning *Strings* in the *PermGen* (instead of the *Heap*) is that we can get an OutOfMemory error from
  the JVM if we intern too many *Strings*.

From Java 7 onwards, the Java String Pool is **stored in the Heap space, which is garbage collected** by the JVM*.* The
advantage of this approach is the reduced risk of OutOfMemory error because unreferenced *Strings* will be removed from
the pool, thereby releasing memory.



---

## A Definition of Java Garbage Collection

Java garbage collection is the process by which Java programs perform automatic memory management. Java programs compile
to bytecode that can be run on a Java Virtual Machine, or JVM for short. When Java programs run on the JVM, objects are
created on the heap, which is a portion of memory dedicated to the program. Eventually, some objects will no longer be
needed. The garbage collector finds these unused objects and deletes them to free up memory.

### How Java Garbage Collection Works

Java garbage collection is an automatic process. The programmer does not need to explicitly mark objects to be deleted.
The garbage collection implementation lives in the JVM. Each JVM can implement garbage collection however it pleases;
the only requirement is that it meets the JVM specification. Although there are many JVMs, Oracle’s HotSpot is by far
the most common. It offers a robust and mature set of garbage collection options.

While HotSpot has multiple garbage collectors that are optimized for various use cases, all its garbage collectors
follow the same basic process. In the first step, [unreferenced objects](https://www.javatpoint.com/Garbage-Collection)
are identified and marked as ready for garbage collection. In the second step, marked objects are deleted. Optionally,
memory can be compacted after the garbage collector deletes objects, so remaining objects are in a contiguous block at
the start of the heap. The compaction process makes it easier to allocate memory to new objects sequentially after the
block of memory allocated to existing objects.

All of HotSpot’s garbage collectors implement a generational garbage collection strategy that categorizes objects by
age. The rationale behind generational garbage collection is that most objects are short-lived and will be ready for
garbage collection soon after creation.

![Java Garbage Collection Heaps](https://stackify.com/wp-content/uploads/2017/05/Java-Garbage-Collection.png)

The heap is divided into [three sections](https://plumbr.eu/handbook/garbage-collection-in-java):

- **Young Generation**: Newly created objects start in the Young Generation. The Young Generation is further subdivided
  into an Eden space, where all new objects start, and two Survivor spaces, where objects are moved from Eden after
  surviving one garbage collection cycle. When objects are garbage collected from the Young Generation, it is a minor
  garbage collection event.
- **Old Generation:** Objects that are long-lived are eventually moved from the Young Generation to the Old Generation.
  When objects are garbage collected from the Old Generation, it is a major garbage collection event.
- **Permanent Generation:** Metadata such as classes and methods are stored in the Permanent Generation. Classes that
  are no longer in use may be garbage collected from the Permanent Generation.

During a full garbage collection event, unused objects in all generations are garbage collected.



