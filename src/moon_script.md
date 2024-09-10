# Moon Script

This is a very basic scripting language for simple scripting with some
syntax based on Rust's.

It doesn't mean to compete with other scripting languages implemented
in Rust like Rhai or dyon, and therefore lacks many features these
have, for example, Moon Script doesn't allow creating custom types yet,
like Rhai does, but allow to create 'associations', which are values that 
can be represented through a series of what MoonScript consider primitives,
like integers, strings, or booleans, for example, an human ID can be
expressed as the primitive 'array' with values [name: String, age: integer].

The goal of MoonScript is for those using it to find themselves scripts
in the simplest manner possible, while still boosting performance, examples
of these simplified manners could be not needing to write semi-colons (``;``)
at the end of lines, or not writting commas (``,``) to separate elements, for
example, this code are actually two valid sentences in MoonScript:

````rust
variable=function(1 2 3)
other_variable="No commas or semicolons used"
````
