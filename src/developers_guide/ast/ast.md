# ASTs

As shown in the previous sections, calling ``Engine::parse`` will return an
``AST``, these ASTs allows you to run the script, and they are independent of 
the ``Engine`` and ``ContextBuilder`` they were created with as  any function,
constant, or input variables are inlined on the AST itself, this means you can
even remove and free an Engine and the AST will still work.

To run an AST you can directly call ``AST::execute``, or create an ASTExecuter
with ``AST::executor``, the executor allows you to further customize the AST's
execution, for example, ``ASTExecutor::push_variable`` allows you to push Late
Variables (Check the Input Variables section) and then to call 
``ASTExecutor::execute``.

Independently on how you call it, you'll be given a ``Result<MoonValue, 
RuntimeError>``, if the execution got an error, you will get ``Err(RuntimeError)``
that can display as shown in the 'Better error formatting' section.

If you get ``Ok(MoonValue)``, it means the execution was successful, and said
it's the return value of the script, which you can turn into any type that 
implements ``TryFrom<MoonValue> for T``, which is a requirement for custom 
types as specified in the 'Creating custom types', this is already implemented 
for ``bool``, ``String``, ``()``, ``i8``, ``i16``, ``i32``, ``i64``, ``i128``,
``isize``, ``u8``, ``u16``, ``u32``, ``u64``, ``u128``, ``usize``, ``f32`` and
``f64``.