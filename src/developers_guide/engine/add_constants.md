# Adding constants
Constants are values that are known when compiling your scripts, this allows
to inline them to execute the multiple optimizations that you can find in the
user's guide.

To create a constant you just need to call the Engine::add_constant function
with a name for your constant and a value, said value must implement 
``Into <MoonValue>`` (Or implementing ``From<T> for MoonValue``), this is already
implemented for rust most basic types: ``bool``, ``String``, ``()``, ``i8``,
``i16``, ``i32``, ``i64``, ``i128``, ``isize``, ``u8``, ``u16``, ``u32``,
``u64``, ``u128``, ``usize``, ``f32`` and ``f64``.


````rust
let mut engine = Engine::new();

// Create a constant named ONE
engine.add_constant("ONE", 1);

// Creates and executes a script that returns the constant
let ast_result = engine.parse(r###"return ONE;"###, ContextBuilder::new()).unwrap()
    .execute().unwrap();

assert_eq!(MoonValue::Integer(1), ast_result);

// The value returned by an AST execution is a MoonValue, luckily, MoonValue implements
// TryFrom for basic rust primitives and String, so we can get it with try_into()
// as an i32
let ast_result_as_i32 : i32 = ast_result.try_into().unwrap();
assert_eq!(1, ast_result_as_i32);
````