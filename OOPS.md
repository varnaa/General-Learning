# OOPS 



![features of OOPs](https://intellipaat.com/mediaFiles/2018/12/j4-1-460x304.png)



---

### Classes and Object

#### Object: 

An Object is comprised of three parts: state, behaviour and identity. An object is a logical or physical entity that tells about the state, behaviour and identity.

Example:

![classes and objects](https://intellipaat.com/mediaFiles/2018/12/Capture-460x214.png)



#### Class:

- In object-oriented programming, a class is an extensible program-code-template for creating objects, providing initial values for state and implementations of behavior. 

- A *class* is the blueprint from which individual objects are created. ( Oracle Definition )


---



### List Of OOP Concepts in Java



- Abstraction
- Encapsulation
- Inheritance
- Polymorphism
  - Method overloading
  - Method overriding

---

### Abstraction

- The process of hiding the implementation and showing the functionality is called abstraction.
- Two ways to achieve abstraction:
  - Interface
  - Abstract class



#### Abstract Class:

- Abstract class is used when you currently don’t know the implementation of a class or method, i.e. hidden part that user can’t see.
- abstract keyword is used to make abstract class.
- If a method is abstract, then the class having abstract method must also be abstract. But if a class is abstract, then it is not compulsory that it contains only abstract methods.
- You cannot create objects for abstract classes.
- Abstract class can contain constructors, static methods and final methods.

```java
abstract class Example{
    abstract void testMethod();
    void show();
}
```



- In an object-oriented drawing application, you can draw circles, rectangles, lines, Bezier curves, and many other graphic objects. These objects all have certain states (for example: position, orientation, line color, fill color) and behaviors (for example: moveTo, rotate, resize, draw) in common. Some of these states and behaviors are the same for all graphic objects (for example: position, fill color, and moveTo). Others require different implementations (for example, resize or draw). All `GraphicObject`s must be able to draw or resize themselves; they just differ in how they do it. This is a perfect situation for an abstract superclass. You can take advantage of the similarities and declare all the graphic objects to inherit from the same abstract parent object (for example, `GraphicObject`) as shown in the following figure.

![Classes Rectangle, Line, Bezier, and Circle Inherit from GraphicObject ](https://docs.oracle.com/javase/tutorial/figures/java/classes-graphicObject.gif)



- Classes Rectangle, Line, Bezier, and Circle Inherit from GraphicObject



#### When an Abstract Class Implements an Interface

In the section on [`Interfaces`](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html), it was noted that a class that implements an interface must implement *all* of the interface's methods. It is possible, however, to define a class that does not implement all of the interface's methods, provided that the class is declared to be `abstract`. For example,

```java
abstract class X implements Y {
  // implements all but one method of Y
}

class XX extends X {
  // implements the remaining method in Y
}
```

In this case, class `X` must be `abstract` because it does not fully implement `Y`, but class `XX` does, in fact, implement `Y`.

##### Class Members

An abstract class may have `static` fields and `static` methods. You can use these static members with a class reference (for example, `AbstractClass.staticMethod()`) as you would with any other class.

---

#### Interface:

An *interface* is a reference type, similar to a class, that can contain *only* constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. Interfaces cannot be instantiated—they can only be *implemented* by classes or *extended* by other interfaces. - (Oracle Definition)

- **Note**: Static methods in interfaces are never inherited.

```java
public interface OperateCar{
    // constant declartions if any
    final String carName;
    
    // method signatures
    int changeLanes(Direction direction, double startSpeed, double endSpeed);
    void startCar();
    
}
```



##### Modifiers

The access specifier for an overriding method can allow more, but not less, access than the overridden method. For example, a protected instance method in the superclass can be made public, but not private, in the subclass.

You will get a compile-time error if you attempt to change an instance method in the superclass to a static method in the subclass, and vice versa.

The following table summarizes what happens when you define a method with the same signature as a method in a superclass.

|                          |   Superclass Instance Method   |    Superclass Static Method    |
| :----------------------: | :----------------------------: | :----------------------------: |
| Subclass Instance Method |           Overrides            | Generates a compile-time error |
|  Subclass Static Method  | Generates a compile-time error |             Hides              |

**Note:** In a subclass, you can overload the methods inherited from the superclass. Such overloaded methods neither hide nor override the superclass instance methods—they are new methods, unique to the subclass.



---

## Abstract Classes Compared to Interfaces

Abstract classes are similar to interfaces. You cannot instantiate them, and they may contain a mix of methods declared with or without an implementation. However, with abstract classes, you can declare fields that are not static and final, and define public, protected, and private concrete methods. With interfaces, all fields are automatically public, static, and final, and all methods that you declare or define (as default methods) are public. In addition, you can extend only one class, whether or not it is abstract, whereas you can implement any number of interfaces.

Which should you use, abstract classes or interfaces?

- Consider using abstract classes if any of these statements apply to your situation:
  - You want to share code among several closely related classes.
  - You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).
  - You want to declare non-static or non-final fields. This enables you to define methods that can access and modify the state of the object to which they belong.
- Consider using interfaces if any of these statements apply to your situation:
  - You expect that unrelated classes would implement your interface. For example, the interfaces [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) and [`Cloneable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html) are implemented by many unrelated classes.
  - You want to specify the behavior of a particular data type, but not concerned about who implements its behavior.
  - You want to take advantage of multiple inheritance of type.

An example of an abstract class in the JDK is [`AbstractMap`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractMap.html), which is part of the Collections Framework. Its subclasses (which include `HashMap`, `TreeMap`, and `ConcurrentHashMap`) share many methods (including `get`, `put`, `isEmpty`, `containsKey`, and `containsValue`) that `AbstractMap` defines.



---

### Encapsulation:

Wrapping up data and methods into a single unit is known as encapsulation. It provides the security that keeps data and methods safe from unwanted changes. A [class](https://intellipaat.com/blog/tutorial/java-tutorial/classes-objects/) is an example of encapsulation that binds fields and members into a single unit.

To get encapsulation:

- Declare the variables of a class as private
- Provide public getter and setter methods to change and sight the [variable’s](https://intellipaat.com/blog/tutorial/java-tutorial/data-types-in-java/) values

---

### Polymorphism

- Polymorphism is one name-many forms.
- Polymorphism is the ability of an object to behave differently at different situations.

**Example:**

- Power button has a polymorphic behaviour.
- When you press once, it will switch on the device.
- When you press it again, device will be switched off.
- Plus + operator has a polymorphic behaviour.
- When operands are numeric then it will perform arithmetic addition.
- When the operands are of String type, then it will perform String Concatenation.

Types of Polymorphism:

- Static polymorphism
- Dynamic Polymorphism

#### Static Polymorphism/Compile Time Polymorphism/Early Binding

- Static Polymorphism can be achieved using method overloading.
- It can be achieved with both instance and static methods.
- Static polymorphism is also called as compile time polymorphism or early binding because java compiler is responsible to bind the method calls with actual method at compile time.
- Java compiler will verify the method name first then method parameters to verify the matching method.
- Return type will not be considered to achieve static polymorphism.

#### Dynamic Polymorphism/Run time Polymorphism/Late Binding

- Dynamic Polymorphism can be achieved using both method overriding or dynamic dispatch.
- It is also called as run time polymorphism or late binding because JVM is responsible to decide which method will be invoked based on actual object available in reference variable.
- It can be achieved only with instance method and can’t be achieved using static method.

**Dynamic Dispatch**

- The process of assigning subclass object to super class reference variable is called as dynamic dispatch.



```java
class A{}
class B extends A{}
class C extends A{}
class D extends B{}
class E extends B{}

// Dynamic Dispatch
A ob=new A();   //valid
A ob=new B();   //valid
A ob=new C();   //valid
B ob=new B();   //valid
B ob=new D();  //valid
B ob=new E();  //valid
C ob=new A();   //valid
B ob=new A(); // invalid
D ob=new A(); //invalid
B ob=new C(); // invalid
```

### Method Overloading

- You can write multiple methods inside the class with the same name by changing parameters. This process is called as method overloading.
- When you are overloading methods then
  - Method name must be same
  - Method parameter must be changed in terms of number of parameters, type of parameters and order of parameters.
  - Method return type can be anything.

**Why method overloading?**
Method overloading helps in code re-usability.
Suppose you want to perform addition in java and if overloading was not possible, then you would have wrote addition methods as number of times as you want to perform addition.

```java
class Intellipaat{
    void add(int a, int b){
    	System.out.println(a+b);
    }
    void add(float a, float b){
    	System.out.println(a+b);
    }	
}
class OverLoadTest{
    public static void main(String args[]){
        Intellipaat ob=new Intellipaat ();
        ob.add(10,20);
        ob.add(20.5,20.5);
    }
}
```

**Output:**
30
41



**Ambiguity Error in overloading**

```java
class Intellipaat{
    void add(long a, int b){
    	System.out.println(a+b);
    }
    void add(int a, long b){
    	System.out.println(a+b);
    }
  
}
class OverLoadTest{
	public static void main(String args[]){
        Intellipaat ob=new Intellipaat ();
        ob.add(10,20);
        ob.add(1000,100);
}	
```

- In the above program, JVM will not be able to decide which method to choose to pass the parameters as int is compatible to long(for details refer type casting topic).
- In that case, it will show an ambiguity error.
  Overloading of main method

```java
class OverLoadTest{
public static void main(String args[]){
System.out.println(“String args[] main()”);
OverLoadTest.main(Intellipaat);
}
public static void main(String args){
System.out.println(“String argsmain():”+” ”+args);
}
public static void main(String args1, String args2){
System.out.println(“String two argsmain():”+” ”+args1+” “+args2);
}
}
```

It will only call the main method with` String[](String array) ` as an argument. But we can call the main method explicitly.
**Output:**
String args[] main()
String argsmain(): Intellipaat

### Method Overriding

- Method overriding is a process of implementing super class methods in sub class with the same signature.

```java
class Intellipaat{
	void show(){
		System.out.println(“Super Intellipaat”);
	}
}
class OverLoadTest extends Intellipaat{
    void show(){
    	System.out.println(“Sub Intellipaat”);
    }
	public static void main(String args[]){
        Intellipaat ob=new Intellipaat();
        ob.show();
        Intellipaat ob1=new OverLoadTest (); // overridden method will be called
        ob1.show();
	}
}
```

**Output:**
Super Intellipaat
Sub Intellipaat

**Why to override the method?**

- According to Object Oriented best practices, “Classes are closed for modification”, i.e. if you want to modify the functionality of existence class then you should not disturb the existing class.
- It is always better to write a sub class and provide the required implementations in sub class.
- Sub class can be used to add new functionality, to modify existing functionality and to inherit existing functionality.

Rules to override the method:

- Sub class method name must be same as super class method name.
- Sub class method parameters(in terms of type, numbers and order) must be same as super class method parameters.
- Sub class return type name must be same as super class method return type.

Note:
If super class method return type is class type, then sub class return type cab be either super class or sub class itself as return type.

```java
class Hey{
	A method1(){}
}
class Intellipaat extends Hey{
    A method1(){}
    B method1(){} //valid from Java 5
    C method1(){} //invalid
}
class A{}
class B extends A{}
class C{}
```

- Sub class access modifier must be either same or higher than the super class modifier.

| **Super Class** | **Sub Class**                       |
| --------------- | ----------------------------------- |
| public          | public                              |
| protected       | protected, public                   |
| default         | default, protected, public          |
| private         | private, default, protected, public |

- When super class method is instance method then you have to override in sub class as instance only.
- Final methods cannot be overridden.



---

