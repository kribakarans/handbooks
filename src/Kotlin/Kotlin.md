# Kotlin Handbook

- Open source and Statically types language
- Supports functional and  OOPS
- Running on JVM
- Similar to Java
- Interoperable with Java
- Easy to convert between Java to Kotlin and vice versa
- Automatic Java-Kotlin converter built into Android studio
- Minimal code and Simple structure
- Fast compilation
- Server side applications
- We can develop Web, Desktop and Android Apps with Kotlin
- Kotlin works on different platforms (Windows, Mac, Linux, Raspberry Pi, etc.)
## JDK

[JVM JRE JDK JIT](https://javapapers.com/core-java/differentiate-jvm-jre-jdk-jit/)

![JVM_Workflow](https://javapapers.com/wp-content/uploads/2009/05/jvm-jre-jdk1.png)

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20210304102904/jitedit.jpg" width="520">

# Hello world

```
fun main() {
	println("Hello world!")
}
```

> In Kotlin, code statements do not have to end with a semicolon (`;`)

## Main Parameters
Before Kotlin version 1.3, it was required to use the `main()` function with parameters, like: `fun main(args : Array<String>)`. The example above had to be written like this to work:

```kotlin
fun main(args : Array<String>) {
  println("Hello World")
}
```

This is no longer required, and the program will run fine without it.

## Kotlin Output (Print)

The `println()` function is used to output values/print text:

### Example

```kotlin
fun main() {
  println("Hello World")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_output)

You can add as many `println()` functions as you want. Note that it will add a new line for each function:

### Example

```kotlin
fun main() {
  println("Hello World!")
  println("I am learning Kotlin.")
  println("It is awesome!")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_output2)

You can also print numbers, and perform mathematical calculations:

### Example

```kotlin
fun main() {
  println(3 + 3)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_output3)

## The print() function

There is also a `print()` function, which is similar to `println()`. The only difference is that it does not insert a new line at the end of the output:

### Example

```kotlin
fun main() {
  print("Hello World! ")
  print("I am learning Kotlin. ")
  print("It is awesome!")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_output_print)

## Kotlin Comments

## Single-line Comments

Single-line comments starts with two forward slashes (`//`).

Any text between `//` and the end of the line is ignored by Kotlin (will not be executed).

```kotlin
// This is a comment
println("Hello World")
```

```kotlin
println("Hello World")  // This is a comment
```

## Multi-line Comments

Multi-line comments start with `/*` and ends with `*/`.

Any text between `/*` and `*/` will be ignored by Kotlin.

```kotlin
/* The code below will print the words Hello World
to the screen, and it is amazing */
println("Hello World")
```

## Kotlin Variables

Variables are containers for storing data values.

To create a variable, use `var` or `val`, and assign a value to it with the equal sign (`=`):

### Syntax
```kotlin
var variableName = value
val variableName = value
```

### Example
```kotlin
var name = "John"
val birthyear = 1975

println(name)          // Print the value of name
println(birthyear)     // Print the value of birthyear
```

The difference between `var` and `val` is that variables declared with the `var` keyword **can be changed/modified**, while `val` variables **cannot**.

## Variable Type

Kotlin do not need to be declared with a specified _type_ (like "String" for text or "Int" for numbers, if you are familiar with those).

```kotlin
var name = "John"      // String (text)
val birthyear = 1975   // Int (number)

println(name)          // Print the value of name
println(birthyear)     // Print the value of birthyear
```

Kotlin is smart enough to understand that **"John"** is a `String` (text), and that **1975** is an `Int` (number) variable.

However, it is possible to specify the type if you insist:

```kotlin
var name: String = "John" // String
val birthyear: Int = 1975 // Int

println(name)
println(birthyear)
```

You can also declare a variable without assigning the value, and assign the value later. **However**, this is only possible when you specify the type:

### Example

This works fine:

```kotlin
var name: String
name = "John"
println(name)
```
### Example

This will generate an error:

```kotlin
var name
name = "John"
println(name)
```
## Notes on `val`

When you create a variable with the `val` keyword, the value **cannot** be changed/reassigned.

The following example will generate an error:

### Example

```kotlin
val name = "John"
name = "Robert"  // Error (Val cannot be reassigned)
println(name)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables5)

When using `var`, you can change the value whenever you want:

### Example

```kotlin
var name = "John"
name = "Robert"
println(name)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables_change)

#### So When To Use `val`?

The `val` keyword is useful when you want a variable to always store the same value, like PI (3.14159...):

### Example

```kotlin
val pi = 3.14159265359
println(pi)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables_pi)

## Display Variables

Like you have seen with the examples above, the `println()` method is often used to display variables.

To combine both text and a variable, use the `+` character:

### Example

```kotlin
val name = "John"
println("Hello " + name)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables_println)

You can also use the `+` character to add a variable to another variable:

### Example

```kotlin
val firstName = "John "
val lastName = "Doe"
val fullName = firstName + lastName
println(fullName)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables_println2)

For numeric values, the `+` character works as a mathematical operator:

### Example

```kotlin
val x = 5
val y = 6
println(x + y) // Print the value of x + y
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables_println3)

From the example above, you can expect:

- x stores the value 5
- y stores the value 6
- Then we use the `println()` method to display the value of x + y, which is **11**

## Variable Names

A variable can have a short name (like x and y) or more descriptive names (age, sum, totalVolume).

The general rule for Kotlin variables are:

- Names can contain letters, digits, underscores, and dollar signs
- Names should start with a letter
- Names can also begin with $ and _ (but we will not use it in this tutorial)
- Names are case sensitive ("myVar" and "myvar" are different variables)
- Names should start with a lowercase letter and it cannot contain whitespace
- Reserved words (like Kotlin keywords, such as `var` or `String`) cannot be used as names
### camelCase variables

You might notice that we used **firstName** and **lastName** as variable names in the example above, instead of firstname and lastname. This is called "camelCase", and it is considered as good practice as it makes it easier to read when you have a variable name with different words in it, for example "myFavoriteFood", "rateActionMovies" etc.

## Kotlin Data Types

In Kotlin, the _type_ of a variable is decided by its value:

### Example

```kotlin
val myNum = 5             // Int
val myDoubleNum = 5.99    // Double
val myLetter = 'D'        // Char
val myBoolean = true      // Boolean
val myText = "Hello"      // String
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_data_types)

However, you learned from the previous chapter that it is possible to specify the type if you want:

### Example

```kotlin
val myNum: Int = 5                // Int
val myDoubleNum: Double = 5.99    // Double
val myLetter: Char = 'D'          // Char
val myBoolean: Boolean = true     // Boolean
val myText: String = "Hello"      // String
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_data_types2)

Sometimes you have to specify the type, and often you don't. Anyhow, it is good to know what the different types represent.

You will learn more about **when you need** to specify the type later.

Data types are divided into different groups:

- Numbers
- Characters
- Booleans
- Strings
- Arrays

---
## Numbers

Number types are divided into two groups:

**Integer types** store whole numbers, positive or negative (such as 123 or -456), without decimals. Valid types are `Byte`, `Short`, `Int` and `Long`.

**Floating point types** represent numbers with a fractional part, containing one or more decimals. There are two types: `Float` and `Double`.

>If you don't specify the type for a numeric variable, it is most often returned as `Int` for whole numbers and `Double` for floating point numbers.

---
## Integer Types

### Byte

The `Byte` data type can store whole numbers from -128 to 127. This can be used instead of `Int` or other integer types to save memory when you are certain that the value will be within -128 and 127:

### Example

```kotlin
val myNum: Byte = 100
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_byte)

### Short

The `Short` data type can store whole numbers from -32768 to 32767:

### Example

```kotlin
val myNum: Short = 5000
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_short)

### Int

The `Int` data type can store whole numbers from -2147483648 to 2147483647:

### Example

```kotlin
val myNum: Int = 100000
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_int)

### Long

The `Long` data type can store whole numbers from -9223372036854775808 to 9223372036854775807. This is used when `Int` is not large enough to store the value. Optionally, you can end the value with an "L":

### Example

```kotlin
val myNum: Long = 15000000000L
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_long)

## Difference Between Int and Long

A whole number is an `Int` as long as it is up to 2147483647. If it goes beyond that, it is defined as `Long`:

### Example

```kotlin
val myNum1 = 2147483647  // Int
val myNum2 = 2147483648  // Long
```

---
## Floating Point Types

Floating point types represent numbers with a decimal, such as 9.99 or 3.14515.

The `Float` and `Double` data types can store fractional numbers:

### Float Example

```kotlin
val myNum: Float = 5.75F
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_float)

### Double Example

```kotlin
val myNum: Double = 19.99
println(myNum)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_double)

>Use `Float` or `Double`?
>
The **precision** of a floating point value indicates how many digits the value can have after the decimal point. The precision of `Float` is only six or seven decimal digits, while `Double` variables have a precision of about 15 digits. Therefore it is safer to use `Double` for most calculations.
>
Also note that you should end the value of a `Float` type with an "F".

### Scientific Numbers

A floating point number can also be a scientific number with an "e" or "E" to indicate the power of 10:

### Example

```kotlin
val myNum1: Float = 35E3F
val myNum2: Double = 12E4
println(myNum1)
println(myNum2)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_scientific)

---

## Booleans

The `Boolean` data type can only take the values `true` or `false`:

### Example

```kotlin
val isKotlinFun: Boolean = true
val isFishTasty: Boolean = false
println(isKotlinFun)   // Outputs true
println(isFishTasty)   // Outputs false
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans)

Boolean values are mostly used for conditional testing, which you will learn more about in a later chapter.

---
## Characters

The `Char` data type is used to store a **single** character. A char value must be surrounded by **single** quotes, like 'A' or 'c':

### Example

```kotlin
val myGrade: Char = 'B'
println(myGrade)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_char)

Unlike Java, you cannot use ASCII values to display certain characters. The value 66 would output a "B" in Java, but will generate an error in Kotlin:

### Example

```kotlin
val myLetter: Char = 66
println(myLetter) // Error
```

---
## Strings

The `String` data type is used to store a sequence of characters (text). String values must be surrounded by **double** quotes:

### Example

```kotlin
val myText: String = "Hello World"
println(myText)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_string)

You will learn more about strings in [the Strings chapter](https://www.w3schools.com/kotlin/kotlin_strings.php).

---
## Arrays

Arrays are used to store multiple values in a single variable, instead of declaring separate variables for each value.

You will learn more about arrays in [the Arrays chapter](https://www.w3schools.com/kotlin/kotlin_arrays.php).

---

## Type Conversion

Type conversion is when you convert the value of one data type to another type.

In Kotlin, numeric type conversion is different from [Java](https://www.w3schools.com/java/default.asp). For example, it is not possible to convert an `Int` type to a `Long` type with the following code:

### Example

```kotlin
val x: Int = 5
val y: Long = x
println(y) // Error: Type mismatch
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_conv)

To convert a numeric data type to another type, you must use one of the following functions: `toByte()`, `toShort()`, `toInt()`, `toLong()`, `toFloat()`, `toDouble()` or `toChar()`:

### Example

```kotlin
val x: Int = 5
val y: Long = x.toLong()
println(y)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_type_conv2)

## [Kotlin Operators](https://www.w3schools.com/kotlin/kotlin_operators.php)


## Kotlin Strings

Strings are used for storing text.

A string contains a collection of characters surrounded by double quotes:

### Example

```kotlin
var greeting = "Hello"
```

Unlike [Java](https://www.w3schools.com/java/default.asp), you do not have to specify that the variable should be a `String`. Kotlin is smart enough to understand that the greeting variable in the example above is a `String` because of the double quotes.

However, just like with other data types, you can specify the type if you insist:

### Example

```kotlin
var greeting: String = "Hello"
```

**Note:** If you want to create a `String` without assigning the value (and assign the value later), you must specify the type while declaring the variable:

### Example

This works fine:

```kotlin
var name: String
name = "John"
println(name)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables3)

### Example

This will generate an error:

```kotlin
var name
name = "John"
println(name)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_variables4)

## Access a String

To access the characters (elements) of a string, you must refer to the **index number** inside **square brackets.**

String indexes start with 0. In the example below, we access the first and third element in `txt`:

### Example

```kotlin
var txt = "Hello World"
println(txt[0]) // first element (H)
println(txt[2]) // third element (l)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_access)

> [0] is the first element. [1] is the second element, [2] is the third element, etc.

## String Length

A String in Kotlin is an object, which contain properties and functions that can perform certain operations on strings, by writing a dot character (`.`) after the specific string variable. For example, the length of a string can be found with the `length` property:

### Example

```kotlin
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
println("The length of the txt string is: " + txt.length)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_length)

---

## String Functions

There are many string functions available, for example `toUpperCase()` and `toLowerCase()`:

### Example

```kotlin
var txt = "Hello World"
println(txt.toUpperCase())   // Outputs "HELLO WORLD"
println(txt.toLowerCase())   // Outputs "hello world"
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_touppercase)

---

## Comparing Strings

The `compareTo(_string_)` function compares two strings and returns 0 if both are equal:

### Example

```kotlin
var txt1 = "Hello World"var txt2 = "Hello World"
println(txt1.compareTo(txt2))  // Outputs 0 (they are equal)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_compareto)

---

## Finding a String in a String

The `indexOf()` function returns the **index** (the position) of the first occurrence of a specified text in a string (including whitespace):

### Example

```kotlin
var txt = "Please locate where 'locate' occurs!"
println(txt.indexOf("locate"))  // Outputs 7
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_indexof)

Remember that Kotlin counts positions from zero.
0 is the first position in a string, 1 is the second, 2 is the third ...

---

## Quotes Inside a String

To use quotes inside a string, use single quotes (`'`):

### Example

```kotlin
var txt1 = "It's alright"
var txt2 = "That's great"
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_quotes)

---

## String Concatenation

The `+` operator can be used between strings to add them together to make a new string. This is called **concatenation**:

### Example

```kotlin
var firstName = "John"
var lastName = "Doe"
println(firstName + " " + lastName)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_conc)

Note that we have added an empty text (" ") to create a space between firstName and lastName on print.

You can also use the `plus()` function to concatenate two strings:

### Example

```kotlin
var firstName = "John "
var lastName = "Doe"
println(firstName.plus(lastName))
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_conc2)

---

## String Templates/Interpolation

Instead of concatenation, you can also use "string templates", which is an easy way to add variables and expressions inside a string.

Just refer to the variable with the `$` symbol:

### Example

```kotlin
var firstName = "John"
var lastName = "Doe"
println("My name is $firstName $lastName")
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_strings_template)

"String Templates" is a popular feature of Kotlin, as it reduces the amount of code. For example, you do not have to specify a whitespace between firstName and lastName, like we did in the concatenation example.

## Kotlin Booleans

Very often, in programming, you will need a data type that can only have one of two values, like:

- YES / NO
- ON / OFF
- TRUE / FALSE

For this, Kotlin has a `Boolean` data type, which can take the values `true` or `false`.

---

## Boolean Values

A boolean type can be declared with the `Boolean` keyword and can only take the values `true` or `false`:

### Example

```kotlin
val isKotlinFun: Boolean = true
val isFishTasty: Boolean = false
println(isKotlinFun)   // Outputs true
println(isFishTasty)   // Outputs false
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans)

Just like you have learned with other data types in the previous chapters, the example above can also be written without specifying the type, as Kotlin is smart enough to understand that the variables are Booleans:

### Example

```kotlin
val isKotlinFun = true
val isFishTasty = false
println(isKotlinFun)   // Outputs true
println(isFishTasty)   // Outputs false
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans_without)

---

ADVERTISEMENT

---

## Boolean Expression

A Boolean expression **returns** a Boolean value: `true` or `false`.

You can use a comparison operator, such as the **greater than** (`>`) operator to find out if an expression (or a variable) is true:

### Example

```kotlin
val x = 10
val y = 9println(x > y) // Returns true, because 10 is greater than 9
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans1)

Or even easier:

### Example

```kotlin
println(10 > 9) // Returns true, because 10 is greater than 9
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans2)

In the examples below, we use the **equal to** (`==`) operator to evaluate an expression:

### Example

```kotlin
val x = 10;
println(x == 10); // Returns true, because the value of x is equal to 10
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans3)

### Example

```kotlin
println(10 == 15); // Returns false, because 10 is not equal to 15
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_booleans4)

The Boolean value of an expression is the basis for all Kotlin comparisons and conditions.

You will learn more about conditions in the next chapter.

## Kotlin Conditions and If..Else

Kotlin supports the usual logical conditions from mathematics:

- Less than: a < b
- Less than or equal to: a <= b
- Greater than: a > b
- Greater than or equal to: a >= b
- Equal to a == b
- Not Equal to: a != b

You can use these conditions to perform different actions for different decisions.

Kotlin has the following conditionals:

- Use `if` to specify a block of code to be executed, if a specified condition is true
- Use `else` to specify a block of code to be executed, if the same condition is false
- Use `else if` to specify a new condition to test, if the first condition is false
- Use `when` to specify many alternative blocks of code to be executed

**Note:** Unlike Java, `if..else` can be used as a **statement** or as an **expression** (to assign a value to a variable) in Kotlin. See an example at the bottom of the page to better understand it.

## Kotlin if

Use `if` to specify a block of code to be executed if a condition is `true`.

### Syntax

```kotlin
if (condition) {
  // block of code to be executed if the condition is true
}
```

## Kotlin else

Use `else` to specify a block of code to be executed if the condition is `false`.

### Syntax

```kotlin
if (condition) {
  // block of code to be executed if the condition is true
} else {
  // block of code to be executed if the condition is false
}
```

## Kotlin else if

Use `else if` to specify a new condition if the first condition is `false`.

### Syntax

```kotlin
if (condition1) {
  // block of code to be executed if condition1 is true
} else if (condition2) {
  // block of code to be executed if the condition1 is false and condition2 is true
} else {
  // block of code to be executed if the condition1 is false and condition2 is false
}
```

## Kotlin If..Else Expressions

In Kotlin, you can also use `if..else` statements as expressions (assign a value to a variable and return it):

### Example

```kotlin
val time = 20
val greeting = if (time < 18) {
  "Good day."
} else {
  "Good evening."
}
println(greeting)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_if_else_exp)

When using `if` as an expression, you must also include `else` (required).

**Note:** You can ommit the curly braces `{}` when `if` has only one statement:

### Example

```kotlin
fun main() {
  val time = 20
  val greeting = if (time < 18) "Good day." else "Good evening."
  println(greeting)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_if_else_exp2)

**Tip:** This example is similar to the "ternary operator" (short hand if...else) in Java.

## Kotlin when

Instead of writing many `if..else` expressions, you can use the `when` expression, which is much easier to read.

It is used to select one of many code blocks to be executed:

### Example

Use the weekday number to calculate the weekday name:

```kotlin
val day = 4

val result = when (day) {
  1 -> "Monday"
  2 -> "Tuesday"
  3 -> "Wednesday"
  4 -> "Thursday"
  5 -> "Friday"
  6 -> "Saturday"
  7 -> "Sunday"
  else -> "Invalid day."
}
println(result)

// Outputs "Thursday" (day 4)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_when)

The `when` expression is similar to the `switch` statement in Java.

This is how it works:

- The `when` variable (**day**) is evaluated once
- The value of the **day** variable is compared with the values of each "branch"
- Each branch starts with a value, followed by an arrow (->) and a result
- If there is a match, the associated block of code is executed
- `else` is used to specify some code to run if there is no match
- In the example above, the value of `day` is `4`, meaning "Thursday" will be printed

## Loops

Loops can execute a block of code as long as a specified condition is reached.

Loops are handy because they save time, reduce errors, and they make code more readable.

---

## Kotlin While Loop

The `while` loop loops through a block of code as long as a specified condition is `true`:

### Syntax

```kotlin
while (condition) {
  // code block to be executed
}
```

In the example below, the code in the loop will run, over and over again, as long as the counter variable (i) is less than 5:

### Example

```kotlin
var i = 0
while (i < 5) {
  println(i)
  i++
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_while_loop)

---

## The Do..While Loop

The `do..while` loop is a variant of the `while` loop. This loop will execute the code block once, before checking if the condition is true, then it will repeat the loop as long as the condition is true.

### Syntax

```kotlin
do {
  // code block to be executed
}
while (condition);
```

The example below uses a `do/while` loop. The loop will always be executed at least once, even if the condition is false, because the code block is executed before the condition is tested:

### Example

```kotlin
var i = 0
do {
  println(i)
  i++
  }
while (i < 5)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_do_while_loop)

## Kotlin Break

The `break` statement is used to jump out of a **loop**.

## Kotlin Continue

The `continue` statement breaks one iteration (in the loop), if a specified condition occurs, and continues with the next iteration in the loop.

## Kotlin Arrays

Arrays are used to store multiple values in a single variable, instead of creating separate variables for each value.

To create an array, use the `arrayOf()` function, and place the values in a comma-separated list inside it:

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
```

---

## Access the Elements of an Array

You can access an array element by referring to the **index number**, inside **square brackets**.

In this example, we access the value of the first element in cars:

### Example

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
println(cars[0])
// Outputs Volvo
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_access)

**Note:** Just like with Strings, Array indexes start with 0: [0] is the first element. [1] is the second element, etc.

---

## Change an Array Element

To change the value of a specific element, refer to the index number:

### Example

```kotlin
cars[0] = "Opel"
```

### Example

```
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
cars[0] = "Opel"
println(cars[0])
// Now outputs Opel instead of Volvo
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_change)

---

## Array Length / Size

To find out how many elements an array have, use the `size` property:

### Example

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
println(cars.size)
// Outputs 4
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_size)

## Check if an Element Exists

You can use the `in` operator to check if an element exists in an array:

### Example

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
if ("Volvo" in cars) {
  println("It exists!")
} else {
  println("It does not exist.")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges_in2)

---

## Loop Through an Array

Often when you work with arrays, you need to loop through all of the elements.

You can loop through the array elements with the `for` loop, which you will learn even more about in the next chapter.

The following example outputs all elements in the **cars** array:

### Example

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
for (x in cars) {
  println(x)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_for_loop)

## Kotlin For Loop

Often when you work with arrays, you need to loop through all of the elements.

To loop through array elements, use the `for` loop together with the `in` operator:

### Example

Output all elements in the cars array:

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
for (x in cars) {
  println(x)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_for_loop)

You can loop through all kinds of arrays. In the example above, we used an array of strings.

In the example below, we loop through an array of integers:

### Example

```kotlin
val nums = arrayOf(1, 5, 10, 15, 20)
for (x in nums) {
  println(x)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_array_for_loop2)

---

## Traditional For Loop

Unlike Java and other programming languages, there is no traditional `for` loop in Kotlin.

In Kotlin, the `for` loop is used to loop through arrays, ranges, and other things that contains a countable number of values.

---
## Kotlin Ranges

With the [`for` loop](https://www.w3schools.com/kotlin/kotlin_for_loop.php), you can also create **ranges** of values with "`..`":

### Example

Print the whole alphabet:

```kotlin
for (chars in 'a'..'x') {
  println(chars)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges)

You can also create ranges of numbers:

### Example

```kotlin
for (nums in 5..15) {
  println(nums)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges2)

**Note:** The first and last value is included in the range.

---

## Check if a Value Exists

You can also use the `in` operator to check if a value exists in a range:

### Example

```kotlin
val nums = arrayOf(2, 4, 6, 8)
if (2 in nums) {
  println("It exists!")
} else {
  println("It does not exist.")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges_in)

### Example

```kotlin
val cars = arrayOf("Volvo", "BMW", "Ford", "Mazda")
if ("Volvo" in cars) {
  println("It exists!")
} else {
  println("It does not exist.")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges_in2)

---

## Break or Continue a Range

You can also use the `break` and `continue` keywords in a range/`for` loop:

### Example

Stop the loop when `nums` is equal to `10`:

```kotlin
for (nums in 5..15) {
  if (nums == 10) {
    break
  }
  println(nums)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges_break)

### Example

Skip the value of 10 in the loop, and continue with the next iteration:

```kotlin
for (nums in 5..15) {
  if (nums == 10) {
    continue
  }
  println(nums)
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_ranges_continue)

---

# Kotlin Functions

A **function** is a block of code which only runs when it is called.

You can pass data, known as parameters, into a function.

Functions are used to perform certain actions, and they are also known as **methods**.

---

## Predefined Functions

So it turns out you already know what a function is. You have been using it the whole time through this tutorial!

For example, `println()` is a function. It is used to output/print text to the screen:

### Example

```kotlin
fun main() {
  println("Hello World")
}
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_helloworld)

---

## Create Your Own Functions

To create your own function, use the `fun` keyword, and write the name of the function, followed by parantheses **()**:

### Example

Create a function named "myFunction" that should output some text:

```kotlin
fun myFunction() {
  println("I just got executed!")
}
```

---

## Call a Function

Now that you have created a function, you can execute it by **calling** it.

To call a function in Kotlin, write the name of the function followed by two parantheses **()**.

In the following example, `myFunction()` will print some text (the action), when it is called:

### Example

```kotlin
fun main() {
  myFunction() // Call myFunction
}

// Outputs "I just got executed!"
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function)

A function can be called multiple times, if you want:

### Example

```kotlin
fun main() {
  myFunction()
  myFunction()
  myFunction()
}

// I just got executed!
// I just got executed!
// I just got executed!
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_multiple)

---

## Function Parameters

Information can be passed to functions as parameter.

Parameters are specified after the function name, inside the parentheses. You can add as many parameters as you want, just separate them with a comma. Just note that you must specify the type of each parameter (Int, String, etc).

The following example has a function that takes a `String` called **fname** as parameter. When the function is called, we pass along a first name, which is used inside the function to print the full name:

### Example

```kotlin
fun myFunction(fname: String) {
  println(fname + " Doe")
}

fun main() {
  myFunction("John")
  myFunction("Jane")
  myFunction("George")
}

// John Doe
// Jane Doe
// George Doe
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_param)

---

When a **parameter** is passed to the function, it is called an **argument**. So, from the example above: `fname` is a **parameter**, while `John`, `Jane` and `George` are **arguments**.

---

## Multiple Parameters

You can have as many parameters as you like:

### Example

```kotlin
fun myFunction(fname: String, age: Int) {
  println(fname + " is " + age)
}

fun main() {
  myFunction("John", 35)
  myFunction("Jane", 32)
  myFunction("George", 15)
}

// John is 35
// Jane is 32
// George is 15
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_param2)

**Note:** When working with multiple parameters, the function call must have the same number of arguments as there are parameters, and the arguments must be passed in the same order.

---

## Return Values

In the examples above, we used functions to output a value. In the following example, we will use a function to **return** a value and assign it to a variable.

To return a value, use the `return` keyword, and specify the **return type** after the function's parantheses (`Int` in this example):

### Example

A function with one `Int` parameter, and `Int` return type:

```kotlin
fun myFunction(x: Int): Int {
  return (x + 5)
}

fun main() {
  var result = myFunction(3)
  println(result)
}

// 8 (3 + 5)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_return)

Using two parameters:

### Example

A function with two `Int` parameters, and `Int` return type:

```kotlin
fun myFunction(x: Int, y: Int): Int {
  return (x + y)
}

fun main() {
  var result = myFunction(3, 5)
  println(result)
}

// 8 (3 + 5)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_return2)

---

## Shorter Syntax for Return Values

There is also a shorter syntax for returning values. You can use the `=` operator instead of `return` without specifying the return type. Kotlin is smart enough to automatically find out what it is:

### Example

```kotlin
fun myFunction(x: Int, y: Int) = x + y

fun main() {
  var result = myFunction(3, 5)
  println(result)
}

// 8 (3 + 5)
```

[Try it Yourself »](https://www.w3schools.com/kotlin/trykotlin.php?filename=demo_function_assignment)

---
