# Design Principles

## 1. KISS - Keep it simple stupid

`A simple solution is better than a complex one, even if the solution looks stupid.`

#### Description:

- The KISS principle is about striving for simplicity. Modern programming languages, frameworks and APIs have powerful means to create sophisticated solutions for various kinds of problems. Sometimes developers might feel tempted to write “clever” solutions that use all these complex features.
- The KISS principle states that a solution is better when it uses less inheritance, less polymorphism, fewer classes, etc.
- A solution that follows the KISS principle might look boring or even “stupid” but simple and understandable. - The KISS principle states that there is no value in a solution being “clever” but in one being easily understandable.
- This does not mean that features like inheritance and polymorphism should not be used at all. Rather they should only be used when they are necessary or there is some substantial advantage

### Reasoning:
-  A simpler solution is better than a complex one because simple solutions are easier to maiantain.
- This includes increased readability, understandability and changeabilitiy.
- The advantage of simplicity is even bigger when the person who maintains the software is not the one who once wrote it.
- One reason to create more complex code is to make it more flexible to accommodate further requirements. But one cannot know in what way to make it flexible or if that flexibility will be ever needed.
- “When you make your code more flexible or sophisticated than it needs to be, you over-engineer it. Some do this because they believe they know their system’s future requirements. They reason that it’s best to make a design more flexible or sophisticated today, so it can accommodate the needs of tomorrow. That sounds reasonable, if you happen to be a psychic.” - Refactoring To Patterns - Joshua Kerievsky


### Strategies:

This is a very general principle so there is a large variety of possible strategies to adhere more to this principle largely depending on the given design problem:

Avoid inheritance, polymorphism, dynamic binding and other complicated OOP concepts. Use delegation and simple if-constructs instead.

Avoid low-level optimization of algorithms especially when involving Assembler, bit-operations, and pointers. Slower implementations will work just fine.

Use simple brute-force solutions instead of complicated algorithms. Slower algorithms will work in the first place.

Avoid numerous classes and methods as well as large code blocks.

For slightly unrelated but rather small pieces of functionality use private methods instead of an additional class.

Avoid general solutions needing parameterization. A specific solution will suffice.

### Evidence:

 Simpler solutions are faster to implement.

 Simpler solutions yield fewer implementation faults (which reduces testing effort).

 Simpler solutions are easier to maintain, i.e. detecting and correcting defects is more effective and efficient.

 Simpler solutions yield more reliable software, i.e. fewer defects show up after releasing the software.

---

## 2. YAGNI - You ain't gonna need it

` Don't implement a feature until there is need for it. `

### Description:

When faced with a decision what to implement, implement only the features that are necessary at the moment. Features which are regarded as useful but don't seem necessary at the moment are better left for the time when they are deemed necessary. That way the necessary features will be developed faster. In addition no time will be wasted for features that never become necessary.

---





