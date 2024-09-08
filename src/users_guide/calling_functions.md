# Calling functions
Functions might be called directly by their name, like:

```rust
print("Hi")
```

<br>

When calling a function with multiple parameters, it isn't needed for you to
separate these arguments with commas, making the following calls valid:

```rust
my_function(1, null, "With commas")
my_function(1 null "No commas!")
```

<br>

If you are using an implementation with multiple modules, two modules can
have the same function for a name, like 'sum' function in both a 'math' and
an 'my_nums' module, to disambiguate from them, you can precede the module's 
name to the function, like:

```rust
math/sum("1 2 3")    //Calls 'sum' from the 'math'    module
my_nums/sum("1 2 3") //Calls 'sum' from the 'my_nums' module
```

<br>
<br>


*Note that Moon Script comes with very few functions, currently just
having 'print' and 'println' functions to print ***one*** value. 

## Optimizations

Some functions are constant, meaning that when its arguments are known at
compile time, their result will be calculated at compile time to prevent
needlessly calculating the same value over each execution:

<table>
<thead>
<td> Input </td> <td> Moon Script Optimization </td>
</thead>
<tr>
<td>


````rust
a = 5
b = 10
sum = add(a b)
return sum
````

</td>
<td>

Given the custom function named <i>add</i> is constant:

````rust
return 15
````

</td>
</tr>
</table>