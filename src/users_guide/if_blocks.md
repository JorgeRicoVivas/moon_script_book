# If blocks

The following syntax allows you to create a series of conditions followed by 
some code to execute, the first condition met will be the one to trigger it's
matching block:

````rust
if condition_1{
    statements_1
} else if condition_2 {
    statements_2
} else if condition_3 {
    statements_3
...
} else {
    statements_n
}
````

for example:

````rust
if number >= 10{
    println("This number has at least two digits")
} else {
    println("This number is either negative or has less than two digits")
}
````

<br>

The ``else`` statements are not mandatory, making the following also valid:

````rust
if number >= 10{
    println("This number has at least two digits")
}
````

## Optimizations

When the conditions for an if block are constant, they might be inlined to
lower the performance and memory costs of both executing and storing lines
that will never be visited, the following examples will be transformed:

<table>
<thead>
<td> Input </td> <td> Moon Script Optimization </td>
</thead>
<tr>
<td>

```rust
if true{
    println("Always executed")
} else{
    println("Never executed")
}
```

</td>
<td>

```rust
println("Always executed")
```

</td>
</tr>

<tr>
<td>

```rust
if true {
    println("Always executed")
} else if n>5{
    println("Never executed")
} else{
    println("Never executed")
}
```

</td>
<td>

```rust
println("Always executed")
```

</td>
</tr>

<tr>
<td>

```rust
if n>5 {
    println("Only executed when n>5")
} else if true{
    println("Only executed when not n>5")
} else{
    println("Never executed")
}
```

</td>
<td>
<i>- if n isn't known when compiling this script</i>:

```rust
if n>5 {
    println("Only executed when n>5")
} else if true{
    println("Only executed when not n>5")
}
```

<i>- if n is known when compiling this script and n>5</i>:

```rust
println("Only executed when n>5")
```

<i>- if n is known when compiling this script and not n>5</i>:

```rust
println("Only executed when not n>5")
```

</td>
</tr>
</table>