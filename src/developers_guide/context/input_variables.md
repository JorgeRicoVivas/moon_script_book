# Input variables

Input Variables allows you to include variables on your script without
the need of your users to create or declare them:


````rust
let engine = Engine::new();

// Create a context with an inlined named user_name whose value is 'Jorge'
let context = ContextBuilder::new()
    .with_variable(InputVariable::new("user_name").value("Jorge"));

// Creates and executes a script that returns the value of said variable
let user_name : String = engine.parse("return user_name;", context)
    .unwrap().execute().unwrap().try_into().unwrap();

assert_eq!("Jorge", user_name);
````

<br>
There are three kinds of Input Variables:<br><br>

- Inlined variables: These are given through the ``InputVariable::value`` 
method and gives an exact value, these variables will be inlined on the
AST.<br><br>
- Lazy / Produced variables: These are given through the
``InputVariable::lazy_value`` method and gives a 
``fn(...) -> Into<MoonValue>``, these variables won't be directly inlined,
but their value will be calculated just once every time the script is run.<br><br>
- Late variables: These require you to give them a type through either 
``InputVariable::associated_type_of<T>`` or ``InputVariable::associated_type``
to allow the Engine to discover it's proper type and therefore functions, 
but in exchange, you don't need to give the value on the ContextBuilder,
you can give them just before executing the script, meaning the value can
change every time you run the script.<br><br>
This might seem similar to Lazy variables, but this one allows you to 
fully customize the value, while a closure might be harder to handle. <br><br>
This example uses a Late variable instead of an Inlined one:

````rust
let engine = Engine::new();

// Create a context with a late variable named user_name whose type is that of a String
let context = ContextBuilder::new()
    .with_variable(InputVariable::new("user_name").associated_type_of::<String>());

// Compiles a script that returns the value of said variable and creates an executor to
// execute said script, to give the value of this variable to the executor, the method
// push_variable is called with the name of the late variable and it's value
let ast = engine.parse("return user_name;", context)
    .unwrap();
let ast_executor = ast.executor()
    .push_variable("user_name", "Jorge");

// Executes the AST to return the value of said variable
let user_name: String = ast_executor
    .execute().unwrap().try_into().unwrap();

assert_eq!("Jorge", user_name);
````

## Optimizations
If you give an Input Variable, but the AST doesn't use it, said variable
will not be included in the AST to prevent it from occupying memory, and 
in the case, a performance cooldown, as these won't be necessary to 
calculate.

## Performance
If performance is a top priority, you must consider how you give the values
in your scripts, as a matter of choosing, choose them with this priority
order:<br><br>

1. Engine's constants or ContextBuilder's Inlined variables: These variables
are always inlined in place, meaning their access is much faster, although
these should be small values.<br><br>
2. Lazy variables: These will get calculated once every script run, unlike 
constants or inlined variables, which are already calculated.<br><br>
3. Let the user create the variable: These work internally in almost the 
same way as Lazy variables, meaning they have the a very similar impact.
<br><br>
4. Late variables: They won't get inlined as constants or inlined variables,
and they require a search of a HashMap on every start of script when you
call ``ASTExecutor::push_variable`` or ``OptimizedASTExecutor::push_variable``,
unlike Lazy variables or used created variables.