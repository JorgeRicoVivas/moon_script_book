# Properties
Some functions have the first parameter as a property, meaning you can call them
directly from the object in a more concise manner:

````rust
let x = point.x //Assigns the value of x from Point to a variable
let x = get_x(point) //This executes the same as the previous statement,
                     //but it's more verbose
````

Some properties can also receive arguments:

````rust
let x = point.set_x_and_take(5) //Saves the value of x from Point
let x = set_x_and_take(point 5) //This executes the same as the previous statement,
                                //but feels harder to read
````


## Getters and Setters as Properties
Calling a property with no parameters, it's the same as calling either a 
function named ``get_*name*`` or ``*name*``, like:

````rust
let x = point.x //Assigns the value of x from Point to a variable
let x = get_x(point) //This executes the same as the previous statement,
                     //but it's more verbose
````

However, if the property is an assignment, it won't search for 
``get_*name*``, but ``set_*name*`` instead

````rust
point.x = 5 //Assigns the value of x from Point to a variable
set_x(point 5) //This executes the same as the previous statement,
               //but it's more verbose
````

## Optimizations

Properties are essentially just functions, meaning if their function
and their arguments are both constant, they can get inlined in the same
fashion as shown in the functions section.