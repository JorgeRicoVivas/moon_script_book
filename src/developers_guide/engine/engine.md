# Engine
The Engine allows you to compile ASTs out of scripts source code, and 
these ASTs can be run directly:

````rust
let engine = Engine::new();
let context = ContextBuilder::new();

// Create an AST out of a script that prints to the standard output
let ast = engine.parse(r###"println("Hello world")"###, context).unwrap();

/// Execute the AST
ast.execute();
````

The Engine type allows you to configure how these scripts are compiled,
allowing you to indicate constants, functions or types that are common
to all your scripts, this will be specified in the following pages.