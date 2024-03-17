# Data Types
As previously mentioned, Gear is a *statically typed* language. This means each variable has to have a specific type annotation so the compiler knows how to use it. Let's go over the basics.

## Primitives
A primitive type represents a single value. Gear has four primary primitives: integers, floating-point numbers, Booleans, and characters. You may recognize these from other programming languages.

### Integer Types
An integer is a number without a fractional component. We've used plenty in previous examples, the `int` type. This type declaration indicates that the value it’s associated with should be an signed integer. By default Gear uses 32-bit integers. You can specify the size via `int<32>`. The table below shows the built-in integer types in Gear. We can use any of these variants to declare the type of an integer value.

<div style="width:1000px;text-align:center;margin-left:25vh;padding:2px">
    <table>
    <tr>
        <th>Length</th>
        <th>Signed</th>
        <th>Unsigned</th>
    </tr>
    <tr>
        <td>8-bit</td>
        <td>8</td>
        <td>u8</td>
    </tr>
    <tr>
        <td>16-bit</td>
        <td>16</td>
        <td>u16</td>
    </tr>
    <tr>
        <td>32-bit</td>
        <td>32</td>
        <td>u32</td>
    </tr>
    <tr>
        <td>64-bit</td>
        <td>64</td>
        <td>u64</td>
    </tr>
    </table>
</div>

All type variants need to be wrapped in *arrow-heads*(`<>`) to comply with syntax. Each type can be either *signed* or *unsigned*. This referring to whether the integer can be negative or not. The signed numbers are encoded using [two's complemement](https://en.wikipedia.org/wiki/Two%27s_complement).

Each signed variant can store numbers from **-(2n^-1)** to **2n^-1 - 1** inclusive, where n is the number of bits that variant uses. So an `<8>` can store numbers from **-128 to 127**. Unsigned variants can store numbers from **0** to **2n - 1**, so a `<u8>` can store numbers from **0 to 255**.

So how do you know which one to use? Gear is forgiving in the sense of *loose integers*. You can just default to using the `int` type without any *size annotation* and the compiler will just choose for you at compile time. For example, a variable: `let x:int = 5;`. Would compile to the same thing as `let x:int<u8> = 5;`. If your variable is *immutable* like in the previous example, Gear automatically compiles it to the right size. However, if your variable is *mutable*: `let !x:int = 5;` and it's value changes during runtime. Gear will default to `<32>`. If you need to use a larger number, add in *size annotations* to your desired size.

> **Integer Overflow**<br>
If you have a variable that has been strictly set to a specific size. For example, a `let x:<u8>`. This means you can hold values of 0 to 255. If at any point you try to exceed this range whether into negatives or postives. An *integer overflow* will occur.<br><br>
If you are in **debug** mode, which is by default when using `cog build`. Your compiler will throw an `overflow error` with a `fatal`. This means your program will display an error then exit.<br><br>
However, if you are in **release** mode, `cog build --release`. The compiler, at runtime, will instead just use *two's complement wrapping*. To summarize, the value that is exceeding it's minimum or maximum length will wrap around to the start or end of it's size. For example, if we have a `<u8>` with a size of 255, if we then add 1 to it. The value changes from 255 to 0. Instead of 256. If we do the opposite, we have a `<u8>` with a size of 0. And we take away 1 from it. The value changes from 0 to 255.<br><br>
Relying on integer wrapping is **always** considered **unsafe**. However, the compiler may not flag you up on it. We recommend handling overflow using a variety of different methods like *temporary values* or throwing errors via `fatal`.

### Floating-Point Types
Gear has just two floating point primitives. You initialise them using the `float` keyword. Similar to integers, floats are automatically assigned by the compiler, so you never really need to worry about *size annotation* unless you are working with specific scenarios such as embedded computing. In this case, you can choose a size the same way as an `int`. Using: `float<32>` or `float<64>`. By default, Gear uses `float<32>` during **debug** mode. But during **release**, we use `float<64>`. This is because `float<32>` and `float<64>` are so similar to each other at low level but `float<64>` is more accurate than `float<32>` without a direct drawback.

```gear
func main() -> void {
    let x:float = 1.04;
    let y:float<32> = 1.04;
}
```

Here is an example of how floats are declared in Gear. They follow the same naming and typing conventions as integers however they are capable of storing fractional units. Floating-point numbers are represented according to the **IEEE-754** standard. The `float<32>` type is a single-precision float, and `float<64>` has double precision.

### Numeric Operations
Gear is capable of all the usual mathematical operations you find in alternative programming languages: addition, subtraction, multiplication, division, and modulus remainder division. Let's check out how to use them:

```gear
func main() -> void {
    let addition:int = 1 + 2;       // 3

    let subtraction:int = 2 - 1;    // 1

    let multiply:int = 3 * 5;       // 15

    let divide:int = 6 / 2;         // 3

    let mod:int = 45 % 7;           // 3
}
```

Each expression in these statements uses a mathematical operator and evaluates to a single value, which we then assign to variable.

### Boolean Type
Similar to near to all other programming languages, Boolean evaluate to just two states: `true` and `false`. A Boolean value is stored as just one byte in size. The Boolean type is annotated via the `bool` keyword on a variable: `let x:bool = false;`.

### Character Type
Gear has one primitive type for alphabetic characters: `char`. Below is how we use this type in action:

```gear
func main() -> void {
    let x:char = 'a';
    let e:char = '😻';
}
```

Gear allows the use of both single quotes (`'`) and double quotes (`"`) to declare the `char` type. At the low level, the `char` type is four bytes in size. It is encoded in UTF-8 by default. It can store virtually any character that is defined in the Unicode Standard. Accented characters, emoji and zero-width space characters.

You can choose to encode the `char` type in a different standard via `char<'ascii'>`. However, there is no real use case in doing this, but it's there if you need it.

## Extended Types
When we talk about *extended types* this refers to types that can store multiple values. Gear has two primitive extended types.

### Tuples
A *tuple* is a way to group a number of values together. The values do not need to have one specific type. However, tuples do have a fixed length. This means they can't grow or shrink during runtime.

We create tuples by writing comma seperated values sorrounded by parentheses. Each position in a tuple has a type but the values in a tuple do not need to all share the same type.

```gear
func main() -> {
    let x:tuple = (13,'h',"hello");
    let x:tuple (int, char, string) = (13, 'h', "hello");   // type annotation
}
```

This example shows the way we create a tuple with strict and loose type annotations. Gear handles types in tuples by default, but if you wish to allocate strict types you can.

To access values inside of a tuple, use the tuple name followed by a period (`.`) and then the tuple element index.

```gear
func main() -> void {
    let tup:tuple = (13, 15, 17);
    let x:int = tup.1;                  // 13
    puts(tup.3)                         // prints 17
}
```

You may also wish to destructure tuples directly. You can do this like so:

```gear
func main() -> void {
    let tup:tuple = (13, 15, 17);
    let (x, y, z) = tup;
    let (x:int, y:int, z:int) = tup;        // type annotation
}
```

### Arrays
Similarly to tuples. Arrays in Gear have a fixed length. However, unlike tuples, they have a strict type. This means every element inside the array has to have one specific type:

```gear
func main() -> void {
    let arr:[int;3] = [11, 13, 15];
}
```

The array is declared via a `let` keyword and given an identifier (`arr` in this case) and then using *type annotation* we put the array type and length in brackets (`[]`) seperated by a semicolon (`;`).

#### Accessing Array Elements
To access an array element we use the identifier that it is bound to and then access individual values using the index surrounded by brackets (`[]`). For example:

```gear
func main() -> {
    let arr:[int;3] = [3, 5 ,7];
    let x:int = arr[1];             // 3
}
```

#### Compiler errors
As mentioned above, arrays are fixed length. If you were to try access an index that is *out of bounds* or larger than the array's size at runtime. The compiler will throw a `fatal` error. Let' see an example:

```gear
ref core::string;
ref core::io::echo;
ref core::io::puts;
ref core::io::listen;

func main() -> {
    let months:[string;11] = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

    // Ask user to input number
    echo("Please enter the month number:");
    let !input:string = listen();
    let choice:int = input.toInt() - 1; // arrays start at 0

    // Use user input as index to access array value and then print out the result
    echo(months[choice]);
}
```

This code will fail on build without using ```@unsafe line:11``` because we don't handle what happens if the user inputs something other than a string. However, we will just leave this code unsafe for the example.

This code will then compile and you will be able to run it. However, if the user inputs a number that is negative or larger than 12. The program will throw a `fatal` error as the array only stores 12 values.