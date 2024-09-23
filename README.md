<h1 align="center">
Kotlin Interview Questions 
  <img src="https://img.shields.io/badge/-Kotlin-7c6fe1?style=flat&logo=kotlin&logoColor=white">
</h1> 

It is a repo where I watched the lessons broadcast by Gökhan Özürk on the [Kekod channel](https://www.youtube.com/@KeKod/streams) and shared notes of possible questions that may come up in the interview. 

Additionally, I’ve shared the Turkish version of these notes as a series on [Medium](https://medium.com/p/88d0408eb3cc). 

<h2 align="start">
Basic Types
</h2> 

## 1- Why Kotlin?
- Oracle and Google had a legal dispute over the use of paid APIs in Java, leading Google to embrace Kotlin as a more flexible alternative.
- Kotlin allows developers to take advantage of functional programming features, making code more concise and expressive.
- Kotlin offers optimizations that are specific to the Android runtime, improving performance on certain devices.
- Kotlin and Java produce the same bytecode, ensuring compatibility with existing Java libraries and frameworks.
## 2- What are the different ways of using an object?
- Singleton: An object that only has one instance in the entire application lifecycle. In Kotlin, you can easily create a singleton using the object keyword.
- Companion Object: A special type of object that is associated with a class and allows for the creation of static members, similar to static methods and properties in Java.
- Expression: Objects can be used as expressions in Kotlin, meaning they can be passed around as values, making it possible to treat functions and objects more flexibly.
## 3- What is the difference between `val` and `var`?
- `val` (short for "value") is used for read-only variables. Once a `val` is assigned a value, it cannot be changed. It's essentially a constant reference.
- `var` (short for "variable") is used for mutable variables, meaning their values can be reassigned.

However, it’s important to note that `val` is read-only and not immutable. This means that while the reference itself cannot be changed, the underlying data it points to can still be modified if it’s a mutable object.

In the example below:
```kotlin
class Box {
    var width: Int = 20
    var height: Int = 40
    var length: Int = 50
    var usedSpace: Int = 0

    val availableSpace: Int
        get() {
            return (width * height * length) - usedSpace
        }
}

```
Here, availableSpace is defined with `val`, meaning it is read-only—you cannot directly assign a new value to it. However, its value can still change indirectly because it depends on mutable properties like width, height, length, and usedSpace, all of which are `var` and can be reassigned. So, while the availableSpace itself is read-only, the calculated result can change as the other mutable variables change.

This example illustrates how `val` is not entirely immutable, but only read-only in terms of direct assignment.

## 4- What is "Type Inference"? In what cases is it essential to specify the type?
Type inference is when the compiler automatically deduces the type of a variable based on its assigned value. For example, val name = "Bengisu" is inferred as a String by the compiler.
Explicit type declaration is required in cases where the type cannot be determined from the initialization, such as in function signatures or when declaring a variable without an immediate assignment.
## 5- Is there any performance difference between a `val` and a `var`? Which one is more performant or costly?
There is almost no performance difference between `val` and `var`. However, `val` can be slightly more costly because it involves an internal check to ensure immutability.
In a basic scenario, `var` might be more performant since it doesn’t have the extra overhead of immutability checks.
However, in real-world applications, especially in multi-threaded environments, `val` can offer better performance by ensuring thread-safety and reducing the need for synchronization.
## 6- How can a `var` be used as a `val` without using the `val` keyword? Why would you want to do this? Could you provide an example scenario?
Every variable in Kotlin has implicit get and set functions. You can create a `var` that behaves like a `val` by making the setter private. This means the variable can only be modified within the class it is declared in, but it will be read-only from outside. This can be useful when you want to restrict external modification of a variable while still allowing internal changes.
```kotlin
class Example {
    var value: Int = 10
        private set // Setter is private

    fun changeValue(newValue: Int) {
        if (newValue > 0) {
            value = newValue
        }
    }
}

fun main() {
    val example = Example()
    println(example.value) // 10
    example.changeValue(20) // valid because it's within the class
    println(example.value) // 20
    // example.value = 30 // This will cause a compile error
}

```

## 7- Is there no primitive type in Kotlin?
In Kotlin, there are no direct primitive types, but this doesn't lead to any performance drawbacks. In Kotlin, everything is represented as a class. For example, `Int`, `Boolean`, and `Double` are all classes in Kotlin. However, Kotlin converts these data types into primitive types during compile time in the resulting Java bytecode. This allows Kotlin to retain the flexibility of its class-based system while also benefiting from the performance optimizations of primitive types in Java.

For example:
```kotlin
val number: Int = 10
```
Here, `Int` is a Kotlin class, but when the Kotlin compiler translates this to Java bytecode, it automatically converts it into the primitive int type in Java. So, even though everything in Kotlin is represented as a class, at runtime, these classes are converted to primitive types when necessary.

Java's advantages of lower memory usage and faster access due to primitive types are preserved. This approach allows Kotlin to offer high-level programming features (like class-based structures) without sacrificing performance.

In summary, while Kotlin doesn’t directly use primitive types, during compile time, these types are converted to primitives in Java bytecode, ensuring that Kotlin is on par with Java in terms of performance.

## 8- If a variable is assigned a `null` value and no type is specified, how does Kotlin interpret this variable?
If a variable is assigned a `null` value without specifying its type, Kotlin interprets it as `Nothing?` during type inference. The reason for this is that Kotlin cannot determine what type the variable should hold since `null` doesn't belong to any specific type. In this case, Kotlin uses the most generic representation of an "empty" state, which is `Nothing?`. The `Nothing?` type is typically used in scenarios like exception-throwing or unresolvable situations.

## 9- !! vs ?.
Kotlin's `null safety` mechanism helps prevent errors that can arise when working with null values. To reduce the frequent occurrence of `NullPointerException (NPE)` errors commonly encountered in Java, Kotlin has enhanced this mechanism by making a clear distinction between nullable and non-nullable types. In this context, the `!!` and `?.` operators offer different uses regarding null safety.

`?.`: The safe call operator is used in situations where we want to avoid crashing the code in less critical scenarios. It skips the operation when the value is `null` and does not throw an error.

`!!`: The non-null assertion operator is used to assert that a variable is definitely not null. If the value is `null`, it throws an error and halts the program.

## 10- === vs ==
`===`: Checks for reference equality. This means it checks whether two objects point to the same reference in memory.

`==`: Checks for value equality. This means it checks whether the contents (values) of two objects are equal.
Example:
```kotlin
val a = String("Hello") // Creates a new String object
val b = String("Hello") // Creates another new String object
val c = a

println(a === b) // false, because a and b are different objects
println(a === c) // true, because c references the same object as a
```
Another Example:
```kotlin

val a = "Hello" // A string created with string interning
val b = "Hello" // Another string with the same content

println(a === b) // true, because both a and b reference the same interned string in memory

```
## 11- What happens when a primitive variable is made nullable and reference equality is checked with ===?
Kotlin allows `Byte` variables with the same value to be stored in memory as a single reference. This prevents the unnecessary creation of multiple objects in memory and optimizes memory usage. If a value outside the byte range of -128 to +127 is assigned, it will return false. However, if the value is within the byte range, it will share the same memory address, returning true. This means that different nullable variables with the same value within the byte range point to the same memory address.

```kotlin
val number: Int = 127 //int
val boxedNumber: Int? = number
val anotherBoxedNumber: Int? = number
println(boxedNumber === anotherBoxedNumber) //true

val number2: Int = 128 //int
val boxedNumber2: Int? = number2
val anotherBoxedNumber2: Int? = number2
println(boxedNumber2 === anotherBoxedNumber2) //false
```

## 12- What is the difference between val and const val?
`val` is a read-only variable. Once it is assigned, its reference cannot be changed. Its value can be assigned at **runtime**, meaning it can be calculated while running the program.
```kotlin
val currentTime = System.currentTimeMillis()  // Assigned at runtime
val list = mutableListOf(1, 2, 3)  // The list can be modified, but the reference cannot be reassigned
```
`const val` represents a value that is constant at compile time. This constant must be of a primitive type or String. Its value can only be assigned to constants known during **compile time**, meaning it is completely immutable. `const val` can only be defined at the top level or inside an object/companion object.
```kotlin
const val PI = 3.14  // Compile-time constant
const val APP_NAME = "MyApp"  // A constant string
```
## 13- Explain the concept of **"Type Safety"**.
Type Safety is an important feature of a programming language that defines how data types are used. Essentially, it ensures that variables and functions are compatible with expected types, reducing the likelihood of encountering type mismatches while writing code.

In Kotlin, type safety is enforced at compile time. This means that when we declare a variable, we assign it a specific type, and if we try to assign a value of a different type, the compiler throws an error.

For example, when we define a variable like 
```kotlin 
val name: String = "Bengisu"
```
we cannot assign a value of another type to the `name` variable. This helps us detect errors earlier in the development process.

## 14- Explain the concept of **"Null Safety"**.
Null Safety is a concept that ensures variables cannot hold null values by default, providing us with the ability to write reliable code. In Kotlin, if we want to indicate that a variable can be null, we need to append a ? to its type.

For example, by declaring a variable like
```kotlin 
var nullableName: String? = null
```
we specify that this variable can hold a null value. This feature helps prevent null reference errors(NullPointerExceptions), which are a common source of bugs in many programming languages.

## 15- What is the difference in memory management between a nullable variable having a value and being null?
When a nullable variable has a value or is null, there is a difference in memory management. When we make primitive types nullable, we convert them to reference types. This means that making a variable null does not imply that no memory is allocated for that variable. Instead, memory is allocated for the variable itself, but not for its value.

For example, when we declare
```kotlin 
 val age: Int? = null
```
a reference space is allocated in memory for age. However, when this variable’s value is null, no memory is allocated to store that value. In other words, memory is still reserved for the variable itself, but not for its value. This distinction is important for memory efficiency.

## 16- Why do primitive-type variables work faster than reference-type variables?
Primitive type variables are generally stored in stack memory. The stack is very fast for memory management because data is organized in a last-in, first-out (LIFO) manner. This allows for quick allocation and deallocation of memory space.

On the other hand, reference type variables are typically stored in heap memory. The heap has a more complex memory management structure; here, data allocation and deallocation can be slower because finding space and managing memory chunks in the heap is more complicated and time-consuming.
