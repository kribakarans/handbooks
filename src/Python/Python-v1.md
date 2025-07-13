# Python Handbook

## Variables and datatype

## Get User Input and Perform Casting

## if-else Statements and Boolean Values

## For Loop

## Nested Loops

## While Loop

## Collections

List, Tuple, Set, Dictionary.

## Functions and Returns

## Classes and Objects

Empty class: (pass)

```
class b()
    pass
```

## Constructor:

```
class a():
    def __init__(self)       # Contructor method
        print("Hello!")
```

## Class Variables:

    Class variable: variable inside class.

    Instance variable: The variables inside the constructor.

## Class methods:

    1. Instance method
    2. Class method (@classmethod)
    3. Static method (@staticmethod)

**Class variable: variable inside class**

```
class phone():
    chargerType = "C-Type"     # Class variable
    def __init__(self, brand, price)
        self.brandname = brand
        slef.price = price
```

![Screenshot_20250713_134540_YouTube](Python/res/Screenshot_20250713_134540_YouTube.png)

![Screenshot_20250713_134613_YouTube](Python/res/Screenshot_20250713_134613_YouTube.png)

**Instance variable: The variables inside the constructor.**

```
class phone():
    def __init__(self, brand, price)
        self.brandname = brand # Instance variable
        slef.price = price     # Instance variable
```

![Screenshot_20250713_134047_YouTube](Python/res/Screenshot_20250713_134047_YouTube.png)

**Class methods:**

![Screenshot_20250713_140116_YouTube](Python/res/Screenshot_20250713_140116_YouTube.png)

## Inheritence: (Derived class)

    1. Single Inheritence
    2. Multiple inhertence
    3. Multi-level Inheritence
    4. Heirarchi-level Inheritence
    5. Hybrid Inheritence

**Single Inheritence:**

![Screenshot_20250713_183820_YouTube](Python/res/Screenshot_20250713_183820_YouTube.png)

**Multiple inhertence:**

![Screenshot_20250713_183916_YouTube](Python/res/Screenshot_20250713_183916_YouTube.png)

**Multi-level Inheritence:**

![Screenshot_20250713_184357_YouTube](Python/res/Screenshot_20250713_184357_YouTube.png)

**Heirarchi-level Inheritence:**

![Screenshot_20250713_184557_YouTube](Python/res/Screenshot_20250713_184557_YouTube.png)

**Hybrid Inheritence:**

![Screenshot_20250713_184737_YouTube](Python/res/Screenshot_20250713_184737_YouTube.png)

## Super Keyword:

    Access parent constructor from the child constructor.

    `super().__init__()`

![Screenshot_20250713_185659_YouTube](Python/res/Screenshot_20250713_185659_YouTube.png)

## Polymorphism:
    1. Variable arguments
    2. Method overriding

**Variable arguments:**

![Screenshot_20250713_190033_YouTube](Python/res/Screenshot_20250713_190033_YouTube.png)
![Screenshot_20250713_190209_YouTube](Python/res/Screenshot_20250713_190209_YouTube.png)

**Method overriding:**

![Screenshot_20250713_190529_YouTube](Python/res/Screenshot_20250713_190529_YouTube.png)

**Polymorphism and SuperMethod:**

![Screenshot_20250713_191525_YouTube](Python/res/Screenshot_20250713_191525_YouTube.png)

## Encapsulation: (Access modifiers)
- Public
- Protected (_varname)
- Private   (__varname)

```
    self.companyName   = 'Google' # Public variable
                                  # Access globally

    self._companyName  = 'Google' # Protected
                                  # Child classes can access
```

![Screenshot_20250713_192558_YouTube](Python/res/Screenshot_20250713_192558_YouTube.png)

![Screenshot_20250713_192628_YouTube](Python/res/Screenshot_20250713_192628_YouTube.png)

    self.__companyName = 'Google' # __ makes the variable as private
                                  # This var is visible within the class and its methods

![Screenshot_20250713_192208_YouTube](Python/res/Screenshot_20250713_192208_YouTube.png)

![Screenshot_20250713_192638_YouTube](Python/res/Screenshot_20250713_192638_YouTube.png)

## Exception handling:
    1. Compile time error
    2. Logical error
    3. Run time error

**Runtime error handling:**
```
try:
    a=input()
except Exception:
    print("Error")
```

![Screenshot_20250713_193111_YouTube](Python/res/Screenshot_20250713_193111_YouTube.png)

```
try:
    a=input()
except Exception as err:
    print("Error: ", err)
```

```
try:
    a=input()
except ValueError as er:
    print("Error: ", err)
except TypeError as er:
    print("Error: ", err)
```

![Screenshot_20250713_193237_YouTube](Python/res/Screenshot_20250713_193237_YouTube.png)

```
try:
    a=input()
except ValueError as er:
    print("Error: ", err)
except TypeError as er:
    print("Error: ", err)
finally:
    print("Done")
```

![Screenshot_20250713_193822_YouTube](Python/res/Screenshot_20250713_193822_YouTube.png)

## File Handling:

### Open/Read/Close file
```
    f = open("filenme.txt") # Extenstion is madatory ?
                            # Default Read mode
    print(f)
    data = f.read()
    f.close()
    print(data)
```

### Write file
```
    f = open("filenme.txt", "w")
    f.write("Banana")
    f.close()
```
### Readline
```
    f = open("filenme.txt")
    print(f.readline()) # Print first line
```
---
