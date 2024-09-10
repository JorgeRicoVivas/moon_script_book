# Adding functions
Custom-made functions can be added to the Engine as long as its parameters 
and the return type implement ``Into<MoonValue>`` and 
``TryFrom<T> for MoonValue``, this is already implemented for Rust 
basic types like ``String`` or ``i32``, a ``MoonValue`` itself, or a
custom-made type you made as explained in the Custom Types section.

The following example creates and uses a function named ``sum_two`` for it
to be called and executed inside a MoonScript:


````rust
let mut engine = Engine::new();

// Creates a function that adds two numbers, this function is a function that can be
// called at compile time, so we also call 'inline' to enable this optimization.
let function_sum_two = FunctionDefinition::new("sum_two", |n: u8, m: u8| n + m)
    .inline();

// The function is added to the engine
engine.add_function(function_sum_two);

// Creates and executes a script that sums 1 and 2 and returns its result
let ast_result : i32 = engine.parse(r###"sum_two (1,2);"###, ContextBuilder::new())
    .unwrap().execute().unwrap().try_into().unwrap();
assert_eq!(3, ast_result);
````

## Using Result<Value, Error>

The return type of the function can also be ``Result<DesiredValue, ErrorDescription>``,
where:
- ``DesiredValue``: It's the value the function will return to the Script, this
still requires for it to be ``Into<MoonValue>`` and 
- ``TryFrom<MoonValue> for DesiredValue``.
- ``ErrorDescription`` is either a &str or a String describing why the error 
happened, if the execution finds this value, it will return it as a ``SimpleError``
description, this way your script developer can find what's wrong.

In this example, the function ``sum_two`` now checks for the two values to not
overflow u8's range, returning a line telling the two numbers for cases where 
they are larger:

````rust
let mut engine = Engine::new();

// Creates a function that adds two numbers, this function is a function that can be
// called at compile time, so we also call 'inline' to enable this optimization.
let function_sum_two = FunctionDefinition::new("sum_two", |n: u8, m: u8| {
        n.checked_add(m).ok_or(format! ("Error, numbers too large ({n}, {m})"))
    })
    .inline();

// The function is added to the engine
engine.add_function(function_sum_two);

// Creates and executes a script that sums 1 and 2 and returns its result, not failing
let ast_result: i32 = engine.parse(r###"sum_two(1,2);"###, ContextBuilder::new())
.unwrap().execute().unwrap().try_into().unwrap();
assert_eq!(3, ast_result);

// Creates and executes a script that sums 100 and 200, forcing the compilation to fail
let compilation_error = format!("{}",
    engine.parse(r###"sum_two(100,200);"###, ContextBuilder::new())
    .err().unwrap());
println!("{}", compilation_error);
````

The previous code will show a compilation error, printing the following to the
standard output:

Error: Could not compile.
Cause:
- Position: On line 1 and column 1
- At: <span style="color:red">sum_two</span><span style="filter: brightness(50%)">(200,200)</span>
- Error: The constant function sum_two was tried to be inlined, but it returned this error:<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Could not execute a
  function due to: Error, numbers too large (100, 200). 