# Assignments
Values might be assigned to a variable, where variables' names must start
with a letter and can be followed by more letters, numbers, ``:``, and ``_``.

In Rust it is common to set a variable preceded by in ``let``, however,
that's not required in here, meaning both of these statements are correct:

```rust
let my_number = 5
```

```rust
my_number = 5
```
<br>

A value might be reassigned in the same fashion as in an assignment:

```rust
my_number = 5
my_number = 10
```

<br>

Using ``let`` allows you to just clarify ambiguity when working with nested
blocks:

```rust
modify_me = "I am unmodified"
do_not_modify_me = "I am unmodified"
if true {
    modify_me = "Modified!" //This reassigns 'modify_me'
    let do_not_modify_me = "Assigned!" //This create a new variable 
                                       // only available on this scope
    print(modify_me) //Prints Modified!
    print(do_not_modify_me) //Prints Assigned!
}
print(modify_me) //Prints Modified!
print(do_not_modify_me) //Prints I am unmodified
```


## Input variables
If you are not the developer of the program that is using MoonScript, you 
might find the developers used Input Variables, these are variables the 
developers include into your code without you requiring to use it.

For example, if the context of the program was a car rental, you might be
writing a script for actions to take after an user has asked to rent a
specific car, to allow you using them, the developer might give you 
variables like ``car``, ``appointment``, or ``client`` so you can access 
a bigger context, when they do, you don't need to do anything special, you
can use them directly, like this:

````rust
println("Sending email to the client with rent details");
client.send_email("You have rented a car for the date "+appointment.date.to_string);
````

For more information about the Input Variables used by your program, contact
its developers and their documentation.


## Optimizations

Values and variables known at compile-time* might get inlined when possible:

<table>
<thead>
<td> Input </td> <td> Moon Script Optimization </td>
</thead>
<tr>
<td>

````rust
a = 5
b = a
print(b)
````

</td>
<td>

````rust
print(5)
````

</td>
</tr>
</table>

*This includes some of the Input Variables