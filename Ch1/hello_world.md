# Hello, world!
Now that you’ve installed Gear, it’s time to write your first Gear program. It’s traditional when learning a new language to write a little program that prints the text `Hello, world!` to the screen. So naturally we do that here too.

## Create a Project Directory
You’ll start by making a directory to store your Gear code. It doesn’t matter to Gear where your code lives, but for the case of simplicity, we suggest making a projects directory in your home directory and keeping all your projects there.

Open a terminal and enter the following commands to make a projects directory and a directory for the “Hello, world!” project within the *projects directory*.

For Linux, MacOS, and PowerShell on Windows, enter this:

```bash | psh
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

## Writing your first Gear Program
To ease the setting up and formatting of your project. Use `cog new` to lift all the burden of file management off you. We'll go over each and every file `cog` creates through this process, don't worry.

```bash
$ cog new hello_world
```

This command will create a: `src` folder, `build` folder, `config.cog` file and a `main.gear` file within `src`.

Open up the `src` directory and head into `main.gear`.

```gear
ref core::io::echo;

func main() -> void {
    echo("Hello world");
}
```

You will see that `cog` has generated a *Hello World* program for us. Let's go over how it all works.

Gear out the box doesn't feature builtin functions. To print text to a screen we need to import the `io` module from the **standard library** (`core`) and use the `echo` function. To import modules we use the `ref` keyword.

Let's break down the code line by line.

```gear
func main -> void {
    ...
}
```

The `func` keyword declares a function. While `main` is the name of it. The `main` function is an entry point to all your Gear programs. It's where all code is ran from on start up. the `->` operator is used to define a *return* value for the function. Gear is statically typed meaning we must declare what a function returns. As our function doesn't return anything, we use `void` to declare it doesn't return anything.

```gear
    echo("Hello World");
```

Moving on, the `echo` function is part of the standard library. It is used to print *char* and *String*s. We'll get into this in more detail in a future chapter. We then feed *"Hello World"* which is a *String*. And end the *expression* with a semicolon (`;`). All expressions in Gear end with the semicolon to signify an expression's end.

## Compiling and Running
Before running a Gear program, you must compile it using the  compiler by entering the `cog build` command and passing it the name of your source file, like this:

```bash | psh
$ cog build main.gear
```

On Linux, macOS, and PowerShell on Windows, you can see the executable by entering the `cd` command in your shell, then use `ls` to display the current folder contents:

```bash | psh
$ cd build
$ ls
hello_world
```

This shows the executable file (`hello_world.exe` on Windows, but `hello_world` on all other platforms). From here, you run the `hello_world` or `hello_world.exe` file, like this:

```bash | psh
$ ./hello_world
```

If your *main.gear* is your “Hello, world!” program, this line prints `Hello, world!` to your terminal.

## Gears, cogs and bolts!
**Cog** will not only create a new source file for your gear code. It will also create a `config.cog` file. This is the file that allows you to talk to the compiler. We'll go over this in more detail in another chapter. But for now, take this as one of the ways cog keeps track of your dependencies, project layout and any options you give it.

```cog
name = "hello_world"
version = "1.0.0"
cog = "0.1.0"
```

This is the default file layout of a brand new **cogfile**. You may notice that cog takes in the name you give it in the terminal and sets it in the `name` value. It will also set `version` to "1.0.0" by default. The `cog` value is set to your current cog version. You can view this via `cog --version`.

When compiling your program, cog will use the `name` value as the name for your executable. You can change this value at any point in development. No need for long-term commitment!