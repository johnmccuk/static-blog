04/10/2016

From the brief time I've spent looking at the Apple Swift language, it looks like its a relatively straightforward and elegant language. Strongly typed and built from the ground up to follow safe coding patterns, it offers the power of C in a much friendlier syntax. Swift is now also open sourced, so its available outside the safe confines of the Apple world.

Heres a quick run through of the very basics:

---
## Basic Types

### Constants
These don't need to be set when defined, but can only be set once.

```
let typeInteger: Int = 10
let typeString: String = "Hello World"
```
if the type is inferred, you don't need to define the type
```
let typeInteger = 10
let typeString = "Hello World"
```
### Variables
```
var typeInteger: Int = 10
var typeString: String = "Hello World"``
```
again, if the type is inferred, you don't need to define the type

```
var typeDouble = 10.55
var typeString = "Hello World"
```

```
typeInteger += 10 
//now equals 20

TypeString += "!" 
//now equals "Hello World!"
```

### Optionals
Never seen this before in any other language I'm familiar with, but it's a way that a variable can either hold a value or be nil.

```
var typeOptional : Double? = 12.4
// or set it to nil
var typeOptional: Double = nil
```
We can then check if it contains a value and if it has, unwrap it:

```
var typeInt : Int? = 10
if let unwrappedValue = typeInt
{
    print("value is \(unwrappedValue)")
}
```

You can also automatically unwrap the value when you set it:

```
var typeInt : Int! = 10
print("value is \(typeInt)")
```

This sounds a bit weird to me. I've always assumed a variable can contain null anyway (or nil in Swift), and should be checked before use anyway. Maybe this will become clearer as I start coding more with Swift.

---
## Control Flow
These are as you would expect, pretty similar across all languages.

```
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}

let names = ["John", "Paul", "Ringo", "George"]
for name in names {
    print("Hello, \(name)!")
}
```

```
var aValue = 10
var count:Int = 0
while count < aValue {
   count++
}
```

```
var teamAscore:Int = 2
var teamBscore:Int = 1

if teamAscore:Int > teamBscore {
    print("we won!")
} else if teamAscore < teamBscore {
    print("we lost!")
} else {
   print("a draw!")
}
```

---

## Functions

```
func helloWorld() -> String {
    return "hello world"
}
```

```
func sayHi(PersonsName: String) -> String {
    let greeting = "Yo, " + PersonsName + "!"
    return greeting
}
```

A variadic parameter accepts a range of values and is shown by 3 period characters (...)

```
func calcAverage(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
calcAverage(1, 2, 3, 4, 5)
```

---
## Collections

### Arrays
```
var typeArray = ['value A', 'value B']
```

### Tuples
fixed size collection
```
var typeTuple = (colour: 'black', suit: 'diamonds', 'value': 4)
```
### Dictionaries
key value store
```
var typeDict = ['typeA': 'value A', 'typeB': 'value B']
```

---

## Object Types


### Class
```
class FloatingPointExample { 
    var x: Float
    var y: Float

    init(x: Float, y: Float) {
        self.x = x
        self.y = y
    }

    //getter setter exampl
    var madeUpValue: Float {
       get {
           return self.x + self.y
       }
       set(newValue) {
           self.x = newValue /2
           self.y = newValue *2
       }
    }
}

var exampleClass = FloatingPointExample(x: 5.5, y: 6.0)
```

### Struct
```
struct Person {
    var name: String
    var age: Int
}

let dude = Person(name: "Joe Bloggs", age: 45)
```

Struct instances are always passed by value, and Class instances are always passed by reference.

### Enumerations
A kind of switch statement object
```
enum TrafficLight {
    case Red
    case Amber
    case Green
}

var lightColour = TrafficLight.Red
// = Red
```
---
One final point, Swift accepts Unicode characters, so the following is perfectly legal `let 🍏⌚️ = "appleWatch"`. Can't see it catching on as a naming standard, but cool nonetheless.

Here ends this quick guide to the Swift language. Hopefully its enough to see the syntax similarities/differences to other languages that your comfortable with. 

As is always the case after learning an initial language, additional languages are pretty quick to pick up a basic understanding of. Learning the standard libraries is the more time consuming part.