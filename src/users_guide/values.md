# Values
In Moon Script every value is a primitive between an integer (i128),
decimal (f64), boolean (bool), text (String), null, or arrays, where each
array may contain different types of said primitives or even other arrays,
like for example, a Moon Value can be:

```rust
[0, false, null, empty, "Text",
  ["This array is inside another array",
    [0, "This array is in an array inside in another array"]
  ]
]
```
<br>
Separating values with commas is highly recommended, but they don't need
to be, for example, making the following a valid array with numbers 1, 2
and 3:

```rust
[1 2 3]
```
<br>

This lists every single type of Moon value:
- null = A ``null`` value (``empty`` is also valid)
- boolean = A ``true`` or ``false`` value (``yes`` and ``no`` is also valid)
- decimal = Numbers with decimal places, like ``0.1``, ``-1.2``, or ``.1``
- integer = Numbers with no decimal places, like ``1``, ``5``, ``132``, ``-4``
- string: Any text between two ", like ``"My text"``
- array: A collection of Moon values enclosed by '['' ']', like:
``[1, true, "Hi", null]`` <br>
Arrays' values can be accessed by index following them by ``[*index*]``, like:<br>
````rust
let my_array = ["First", "Second", "Third"]
print(my_array[1]) //Prints "Second"
````

