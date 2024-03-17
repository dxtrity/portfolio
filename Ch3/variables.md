# Variables
As mentioned in previous chapters, variables are immutable by default in Gear. This is one of the ways the compiler drives you to create safer more robust code. However, of course, you are still able to make your variables mutable via the *mutability operator* (`!`).

When a variable is immutable. It's value is bound to whatever is assigned to it. You can't change it. This is like a `const` in other languages. Let's create a new project called *Variables* to better understand them.

```gear
    ...
func main() -> void {
    let x:int = 0;
    puts(x);
    x = 5;
    puts(x);
}
```

Let's pretend all of the necessary functions have already been imported here. If you try compile this code, you will run into an error.

```bash | psh
$ cog build
Compiling 'variables': (src/main.gear)
[ERROR](E000): cannot assign an immutable variable 2 values.
     --> src/main.gear: line:6
     |
   4 | let x:int = 0;
     |
fix? | consider making this mutable: let !x:int = 0;
     |
   6 | x = 5;
err  | ^^^^^ cannot assign an immutable variable 2 values.

Need help? run `cog help E000` to get more info on the error.
error: couldn't compile due to previous errors.
```

This example shows how the compiler will not only stop you from compiling the code. But how it also helps you fix it. The error shows you why your code didn't compile, where the error occured and how you can potentially fix it. You can use `cog help <error>` at any point to get more info on error codes.