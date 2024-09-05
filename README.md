<h1 align="center">
Kotlin Interview Questions 
  <img src="https://img.shields.io/badge/-Kotlin-7c6fe1?style=flat&logo=kotlin&logoColor=white">
</h1> 
It is a repo where I watched the lessons broadcast by Gökhan Özürk on the [Kekod channel](https://www.youtube.com/@KeKod/streams) and shared notes of possible questions that may come up in the interview. 

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
