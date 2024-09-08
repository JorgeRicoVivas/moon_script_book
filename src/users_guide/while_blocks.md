# While blocks

The following syntax allows you to create a loop happening every time
it's condition is met:

````rust
while condition {
    statements
}
````

Note that ``break`` is <span style="color:red">**NOT**</span> currently
supported.

## Optimizations

When the condition of a while block is known at compile-time and it is
false, said block is removed as it will never execute, for example:

<table>
<thead>
<td> Input </td> <td> Moon Script Optimization </td>
</thead>
<tr>
<td>

````rust
print("Before loop")
while false{
    print("Never happening")
}
print("After loop")
````

</td>
<td>

````rust
print("Before loop")
print("After loop")
````

</td>
</tr>
</table>