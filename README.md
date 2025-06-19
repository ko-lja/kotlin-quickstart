# kotlin-quickstart

Ever wanted to try kotlin but didn't want to read countless docs just to know how to get started? Well then this ~~post~~ is for you!
I will try to leave examples where i can, if you think i forgot something, don't hesitate to ping me!
## Properties
The `val` keyword declares a field as public final, meaning it cannot be reassigned after initialization
The `var` keyword declares a field as public and does allow you to reassign it anytime you please
```kt
val str1 = "Hello!" //public final String
str1 = "hi" //this will not work

var str2 = "Hello" //public String
str2 = "hi" //the value of str2 has been changed to "hi"
```
Don't want to initialize the field at declaration? You can easily do that by specifying the type. Tho, even if you're sure that your field is going to get initialized, the compiler is still going to throw an error since it itself does not know for sure that this is the case.
That's what the `lateinit` keyword is for. 
```kt
class Person(_name: String) {
    lateinit var name: String

    fun initialize() {
        this.name = _name
    }

    fun printName() {
        println(name)
    }
}

val samu = Person("Samu")
samu.printName() //This will throw UninitializedPropertyAccessException since we haven't initialized the name variable
samu.initialize() //Unreachable

val merlin = Merlin("Merlin")
merlin.initialize()
merlin.printName() //Merlin
```
## Functions
Methods are called functions in kotlin and declared by the `fun` keyword. The return type comes after the colon. Single-expression functions can skip the braces and return keyword. Default parameters let you call functions with fewer arguments.
```kt
//Basic function
fun greet(name: String): String {
    return "Hello, $name!"
}

//Single-expression function
fun add(a: Int, b: Int) = a + b

//Function with default parameters
fun introduce(name: String = "Samu", age: Int, origin: String) {
    println("I'm $name, $age years old and come from $origin")
}

//Usage
println(greet("Dawson"))
println(add(5, 3))
introduce(age = 19, origin = "Italy") // Uses default name so we only need to pass age and origin
introduce("Merlin", 57, "Netherlands") // Overrides default name
```
## String manipulation
String templates let you embed variables directly in strings using $variable or ${expression}, this is called String interpolation. Triple quotes create multiline strings that preserve formatting. #trimIndent() removes common leading whitespace.
```kt
val name = "Kotlin"
val version = 2.1

// String templates
val message = "Welcome to $name $version"
val formatted = "Result: ${2 + 3}"

// Multiline strings
val multiline = """
    This is a
    multiline string
    with preserved formatting
""".trimIndent()
```
## Collections
Kotlin distinguishes between read-only and mutable collections. listOf() creates an immutable list, while mutableListOf() allows changes, this can prevent accidental modifications.
The to keyword creates key-value pairs for maps. 
Sets are generic unordered collections of elements that do not support duplicate elements.
```kt
val readOnlyList = listOf("Samu", "Dawson", "Merlin")
val mutableList = mutableListOf("Penguin", "Simon")
mutableList.add("Stephen")

// Sets
val readOnlySet = setOf(1, 2, 3, 2)  // Duplicates will be removed
val mutableSet = mutableSetOf(1, 2, 3)

// Maps
val readOnlyMap = mapOf("name" to "Brandon", "age" to 89)
val mutableMap = mutableMapOf("name" to "Panav")
mutableMap["age"] = 30
```
## Control flow
### Conditionals
Unlike in Java, `if` is an expression in Kotlin - it returns a value that you can assign to a variable. `when` is like switch but more powerful: you can match multiple values, ranges (in 2..6), or complex conditions.
```kt
val grade = 85

// if-else as expression
val beating = if (grade >= 90) {
    "No"
} else if (grade >= 80) {
    "Maybe"
} else {
    "Yes"
}

// When expression (like switch)
val dayType = when (dayOfWeek) {
    1, 7 -> "Weekend"
    in 2..6 -> "Weekday"
    else -> "Invalid day"
}
```
### Loops
Loops in kotlin use `in` to iterate over ranges or collections. Ranges like `1..5` include both ends. `downTo` counts backwards, and `step` changes the increment. `while` loops work the same as in java
```kt
//For loops
val ranks = listOf("support", "specialist", "expert")

for (rank in ranks) {
    println(rank)
}

ranks .forEach { println(it) } //Does the same as the for loop

for (i in 1..5) {
    println("Number: $i") //Will print the numbers 1-5, both included
}

for (i in 10 downTo 1 step 2) {
    println(i)  // 10, 8, 6, 4, 2
}

//While loops
var x = 5
while (x > 0) {
    println(x)
    x--
}
```
Also you may have noticed that I used `it` in the `forEach`. Well, what is `it`? `it` is Kotlin's default name for the parameter when your lambda function has exactly one parameter. Instead of having to write out a parameter name, Kotlin lets you use it as a shorthand. Tho you can still use a parameter name
```kt
val ranks = listOf("support", "specialist", "expert")

//Using 'it' - the implicit parameter
ranks.forEach { println(it) }

//This is exactly the same as writing:
ranks.forEach { rank -> println(rank) }
```
## Classes and Objects
The constructor parameters are defined right in the class declaration. val parameters become read-only properties, var parameters become mutable properties. No need for separate field declarations or getter/setter methods.
```kt
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("Hi, I'm $name and I'm $age years old")
    }
    
    fun haveBirthday() {
        age++
        println("Happy birthday! Now I'm $age")
    }
}
//Usage
val person = Person("Samu", 19) //Need an instance of the class to call its methods
person.introduce()
person.haveBirthday()

//Since objects behave like static class they are not able to have constructors
object Samu {
    val name = "Samu"
    var age = 19
  
    fun introduce() {
        println("Hi, I'm $name and I'm $age years old")
    }
    
    fun haveBirthday() {
        age++
        println("Happy birthday! Now I'm $age")
    }
}
//Does not need an instance to call the methods as it behaves like a static class
Samu.introduce()
Samu.haveBirthday()
```
Want extra constructors like in java and run code when the class gets instantiated or the object initialized? Well then this is for you
```kt
class Person(val name: String, var age: Int) {
    init {
        println("Hello! I am $name and I'm $age years old")
    }
    
    constructor(val name: String, var age: Int, val origin: String) {
        println("Hello! I am $name, I'm $age years old and I'm from $origin")
    }
}
val samu = Person("Samu", 19) //This will only run the init code block
val merlin = Person("Merlin", 57, "Netherlands") //This will run both the init and constructor code blocks
```
### Data classes
Data classes are perfect for holding data. They are the equivalent of `record` classes in Java. Kotlin automatically generates equals(), hashCode(), toString(), and copy() methods, this removes the need to write boilerplate code. The copy() function creates a new instance with some properties changed - great for immutable programming.
```kt
data class Person(val name: String, var age: Int, val origin: String)

val samu = Person("Samu", 19, "Earth") //Person("Samu", 19, "Earth") 
val merlin = samu.copy(name = "Merlin", age = 57) //Person("Merlin", 57, "Earth") 
```
## Inheritance
Classes are final by default in Kotlin - you need open to allow inheritance. Use override to replace parent methods. The : Animal(name) syntax calls the parent constructor.
```kt
open class Person(val name: String, val amount: Double) {
    open fun payTaxes() {
        println("No")
    }
}

class Italian(name: String, amount: Double, currency: String) : Person(name) {
    override fun payTaxes() {
        println("$name didn't pay their taxes of $amount $currency")
    }
}
val samu = Italian("Samu", 578.63, "eur")
samu.payTaxes() //Samu didn't pay their taxes of 578.63 eur
```
## Null Safety
### Nullable Types
The ? after a type means it can hold null. The safe call operator ?. only calls the method if the value isn't null. The Elvis operator ?: provides a default value when something is null. This prevents most NPE's.
The Not-null assertion operator﻿ !! converts any value to a non-nullable type, this is particularly useful when you are confident that a value is not null and there’s no chance of getting an NPE, but the compiler cannot guarantee this due to certain rules. In such cases, you can use the !! operator to explicitly tell the compiler that the value is not null
```kt
var name: String = "Samu" //Cannot be null
var nullableName: String? = null //Can be null

//Safe call operator
println(nullableName?.length) //Prints null instead of crashing

//Elvis operator
val length = nullableName?.length ?: 0 //Use 0 if null

//Not-null assertion (use carefully!)
val definitelyNotNull = nullableName!!.length //Throws an exception if null
```
### Safe calls
The let function executes a block of code only if the value isn't null. Inside the block, you get the non-null value. This is a clean way to handle nullable values without nested if-statements.
```kt
val name: String? = //Some function that returns a Nullable String

// Execute block only if name is not null
name?.let {
    println("Hello $it!")
}
```
## Higher-Order Functions + Lambdas
### Basic Lambdas
Lambdas are anonymous functions written in braces. The `it` keyword as explained above refers to the parameter when there's only one. `filter` keeps elements that match a condition, `map` transforms each element, `first` finds the first match.
```kt
val numbers = listOf(1, 2, 3, 4, 5)

//Filter even numbers
val evenNumbers = numbers.filter { it % 2 == 0 }

//Map to squares
val squares = numbers.map { it * it }

//Find first number greater than 3
val firstBig = numbers.first { it > 3 }

println(evenNumbers) // [2, 4]
println(squares) // [1, 4, 9, 16, 25]
println(firstBig) // 4
```
### High order function
Higher-order functions take other functions as parameters. The syntax `(Int, Int) -> Int` means "a function that takes two Ints and returns an Int". You can pass different behaviors to the same function.
```kt
fun operate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

//Usage
val sum = operate(5, 3) { x, y -> x + y }
val product = operate(5, 3) { x, y -> x * y }

println(sum) // 8
println(product) // 1
```
## Extension functions
This is probably one of the features i like the most about kotlin, since it makes your life so much easier.
Extension functions basically let you add new methods to existing classes without modifying their source code. Inside the extension, this refers to the object you're extending. This is incredibly useful for adding utility functions.
```kt
//Adds functionality to existing classes
fun String.lengthSquared() = this.length * this.length //We don't need a return type since this is a one-line expression and kotlin infers the return type

fun <T> List<T>.biggerThan(i: Int) = this.size > i

//Usage
println("samu".lengthSquared()) // 16
println("merlin".lengthSquared()) // 36
println(listOf(1, 2, 3).biggerThan(4)) //false
```
## Other Stuff i find useful
### Interfaces
Interfaces define contracts that classes must follow - they specify what methods and properties a class should have without implementing them. Interfaces let you define shared behavior across different classes. A class can implement multiple interfaces, giving you flexible design patterns.
```kt
interface Person {
    fun eat() 
    fun getName(): String
    fun getAge(): Int
    //The functions above must all be implemented
    
    //Can have default implementations, same as the default keyword in java
    fun inform() {
        println("I am ${getName()} and I'm ${getAge()} years old!")
    }
}

class Samu(val name: String, val age: Int) : Person {
    override fun eat() {
        println("I love eating KitKat cereal")
    }
    override fun getName() = name
    override fun getAge() = age
    //We do not need to override the inform method as it already has a default implementation
}

//Usage
val samu = Samu("Samu", 19)
samu.eat() //I love eating KitKat cereal
samu.inform() //I am Samu and I'm 19 years old!
```

### Annotation classes
Annotation classes are metadata tags that provide information about your code to the compiler, frameworks, or tools - they don't affect runtime behavior directly. They help to automatically generate code, configure behavior, or provide compile-time checks without you having to write boilerplate code.
```kt
//Define an annotation
annotation class ApiEndpoint(val path: String, val method: String = "GET")

annotation class Author(val name: String, val date: String)

//Use annotations
@Author(name = "Samu", date = "2025-05-31")
@ApiEndpoint(path = "/users", method = "POST")
class UserController {
    
    @ApiEndpoint(path = "/users/{id}")
    fun getUser(id: Int) {
        //Method implementation
    }
}
```
