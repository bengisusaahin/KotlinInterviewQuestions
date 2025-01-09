<h1 align="center">
Kotlin Interview Questions 
  <img src="https://img.shields.io/badge/-Kotlin-7c6fe1?style=flat&logo=kotlin&logoColor=white">
</h1> 

It is a repo where I watched the lessons broadcast by Gökhan Özürk on the [Kekod channel](https://www.youtube.com/@KeKod/streams) and shared notes of possible questions that may come up in the interview. 

Additionally, I’ve shared the Turkish version of these notes as a series on [Medium](https://medium.com/@bengisusaahin). 

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary><h2 style="display: inline-block">Table of Contents</h2></summary>
  <ol>
    <li>
      <a href="#basic-types">Basic Types</a>
    </li>
    <li><a href="#control-flows">Control Flows</a></li>
    <li><a href="#loops-and-funtions">Loops and Functions</a></li>
  </ol>
</details>

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

## 17- What is Smart Casting?
Smart Casting is a very useful feature in Kotlin. This feature helps the compiler automatically understand the type of a variable under certain conditions. In other words, when you check whether a variable is of a certain type, if that check is successful, the Kotlin compiler accepts that the variable is of that type and allows you to access features specific to that type.

For example:
```kotlin 
fun handleInput(input: Any) {
    if (input is String) {
        // Here we know that input is a String
        println("Length: ${input.length}") // We can access the length property thanks to Smart Casting
    } else {
        println("Not a string")
    }
}
```
In this example, we check if the input variable is a String. If it is, Kotlin automatically accepts that this variable is a String, allowing us to immediately access the length property. This makes the code cleaner and more readable.

## 18- The `||` and `&&` operators have a lazy evaluation mechanism. What does this mean? (Lazily)
If the left side of the `||` operator is true, there’s no need to check the right side, as the expression will be considered true regardless.

Similarly, if the left side of the `&&` operator is false, the right side won't be evaluated because the result will be false either way.

## 19- Are arrays mutable or immutable?

Arrays are mutable because their values can be changed based on their indices. When we say mutable, it means that even if the array is declared with val, we are changing the reference of its values. This leads to new memory being allocated on the heap. In this sense, we are not actually modifying the data itself but replacing it with new data, which technically makes the array immutable in terms of its reference. However, the values within the indices of the array can always be modified, so the array is mutable in that regard.

To summarize:

If the array is declared with var, it is mutable because we can assign a new array to it.
If it is declared with val, it is immutable because we can't assign a new array, but we can still modify the values at each index.
Regardless of whether the array is declared with val or var, the values at each index can be modified, so the array is mutable in terms of its elements. However, when we perform an operation that seems to modify the array, like adding elements, a new array is created and assigned, which makes it immutable in that specific context.

## 20- Can the == operator be used to compare two arrays?
Using the == operator to compare two arrays compares their memory references, not their contents. In other words, the == operator checks whether the two arrays point to the same memory location. Even if two different arrays contain the same elements, == will return false because each has a different reference. For example:
```kotlin 
val array1 = arrayOf(1, 2, 3)
val array2 = arrayOf(1, 2, 3)

println(array1 == array2) // false
```
In this case, `array1` and `array2` occupy different memory addresses. The reason behind this is that arrays in Kotlin are reference types. Reference types represent a memory address, while value types represent the values directly. Therefore, comparisons made using the `==` operator only check for reference equality.

To compare contents, functions like `contentEquals` or `contentDeepEquals` should be used. These functions allow us to perform an actual equality check by comparing the elements within the two arrays. For example:

```kotlin 
println(array1.contentEquals(array2)) // true
```
This approach prevents unexpected results in your program and enhances the reliability of your code. Accurately comparing array contents is critical for ensuring data integrity.

<h2 align="start">
Control Flows
</h2> 

## 1- When do we use the functional forms of operators?

When one of the two numbers being compared is `nullable`, we prefer to use the functional forms of operators instead of the standard operators. This is because when one of the operators used in the comparison is `nullable`, the standard comparison operators (<, >, <=, >=, ==, !=) do not perform `null` checks, which can lead to unexpected results. This situation may cause errors during the application's execution.

## 2- How do we use nullable expressions with operators?

When working with operators, we can utilize nullable expressions in the following ways:

?.let: Scope Function
The ?.let function allows us to execute a code block only if the value is not null. It provides a safe way to work with nullable types, ensuring that the code inside the let block is executed only when the preceding expression is not null.

Smart Casting: 
```kotlin 
if (grade == null) { return }
```
In this approach, we can check if a variable (e.g., grade) is null and return early from a function if it is. If the check passes, the IDE understands that the grade is not null for the subsequent code, allowing us to use it without additional null checks safely.

## 3- Is writing if, if, if more performant, or else if?
Using `else if` is more performant because once a condition is met, the other conditions are not checked. If you write separate if statements, each condition will be evaluated independently, leading to each one being checked in turn. With `else if`, once a true condition is found, the rest of the conditions are skipped, making the process more efficient.

## 4- What is a destructuring declaration?

One of the coolest features of data classes is the `destructuring declaration`. This allows us to extract the values inside a `data class` in a single step, rather than accessing each property individually. It’s a handy way to make your code cleaner and more readable.

For example:
```kotlin 
data class Person(val name: String, val age: Int)

val person = Person("Bengisu", 25)

// Destructuring Declaration
val (name, age) = person

println("Name: $name, Age: $age")
// Output: Name: Bengisu, Age: 25

```
As you can see here, instead of accessing `name` and `age` separately, we can assign them in one line using `destructuring`. This makes it a quick and efficient way to work with class properties. It’s especially useful when working with functions or collections!

## 5- What is the difference between Unit and Nothing?

In Kotlin, `Unit` and `Nothing` are two important types that represent different scenarios.

`Unit`: Represents functions that do not return anything. In other words, when an operation is performed, it does not provide any value upon return. For example, consider a function that prints a message to the screen; this function does not return anything, it simply performs the task. Therefore, its return type is Unit. `Unit` is similar to `void` in Java.

`Nothing`: Used to indicate that a function will never successfully complete or will always throw an error. If a function returns a value of type `Nothing`, it means that it will not return at any point in time. Instead of returning an empty value, `Nothing` indicates that the function will never finish with a normal result.

In summary, while `Unit` represents a situation with no return value, `Nothing` signifies that an operation will never complete, meaning it will never return.

## 6- What is a Default (None) Argument in functions?

In Kotlin, when we provide a default value for a function parameter, we eliminate the necessity of explicitly specifying that parameter every time. It's like offering a backup option! This feature allows us to perform function overloading without writing multiple versions of the same function.

Thanks to default values, let's consider a function that sends a message; we can call this function without specifying the recipient. If we skip mentioning the recipient, the default value we defined will be used automatically. This makes our function calls cleaner and more readable.

Additionally, we can utilize named arguments to clearly indicate which parameters we want to set. This helps us easily understand which argument corresponds to what when calling our function.

Now, let's look at an example:

```kotlin 
fun sendMessage(message: String, recipient: String = "default@example.com") {
    println("Sending: '$message' - Recipient: $recipient")
}

// Calling with default recipient
sendMessage("Hello!")

// Calling with specified recipient
sendMessage("Hi!", recipient = "user@example.com")

```

As you can see, when we don't specify a recipient, the default recipient takes effect. This makes our function more practical and user-friendly.

## 7- What are the performance effects and differences of using Vararg?
In Kotlin, `vararg` is a useful feature that allows you to pass a variable number of arguments to a function. However, there are some important points to consider when using this feature.

What is Vararg?
`vararg` represents a list of parameters. You can use it in situations where you don't know how many arguments you will pass. However, using `vararg` just because you're passing "a lot of arguments" can unnecessarily complicate your code. This kind of usage can be referred to as "ugly code"!

Performance Difference
Depending on where it is used, there can be a performance difference between `vararg` and `Array`. When you use `vararg`, Kotlin creates an `Array` in the background. This means an additional cost and can lead to differences in decompiled code. For example, the background code generated when using `vararg` may be more complex than directly using an `Array`. Therefore, you should consider your performance requirements when deciding which method to use.

## 8- In Kotlin, how do you use break, continue, return, and return@label statements to manage control flow within loops and functions? What are the differences between these statements?

break: Used to exit from a loop (for, while). The `break` statement immediately terminates the loop and allows you to exit from it.

continue: Allows you to skip the current iteration of the loop when a specific condition is met. The `continue` statement prevents the remaining code in the loop from being executed and moves to the next iteration.

return: Used to return a result from a function. The `return` statement specifies the return value of the function and exits from it.

return@label: Enables you to return from specific labeled loops or nested functions in more complex loop structures. The `return@label` allows you to specify which function or loop you want to exit from.

<h2 align="start">
Loops and Functions
</h2> 

## 1- What is the performance difference between passing a String array (vararg) versus passing individual Strings?

In Kotlin, there are two ways to pass multiple parameters to a function:

Using vararg, you can pass parameters as an Array<String>.
You can pass parameters individually, one by one.
There is generally no noticeable performance difference. When using vararg, the parameters are internally converted into an Array object. This conversion is very fast for small data sets and is typically imperceptible. However, for large data sets or frequently called functions, using vararg could introduce a very small performance overhead because an extra Array is created with each function call.

That being said, the difference is usually negligible and does not affect the overall performance of the program. Performance concerns are generally more significant in areas such as algorithms or I/O operations.
