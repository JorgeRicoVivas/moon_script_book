# Creating custom types

MoonScript doesn't use the real concept of a custom type, but rather
an association, this means every custom type will be turned into a MoonValue
during the script's executing, and will return a MoonValue, for this matter,
your custom types must implement ``From<MyType> for MoonValue`` and 
``TryFrom<MoonValue> for MyType``, which is planned to be a derive macro
in the future.

The following example shows how you can create a custom type:

````rust
struct MyType {
    name: String,
    age: u16,
}

impl From<MyType> for MoonValue{
    fn from(value: MyType) -> Self {
        MoonValue::from(vec![
            MoonValue::from(value.name),
            MoonValue::from(value.age)
        ])
    }
}

impl TryFrom<MoonValue> for MyType {
    type Error = ();

    fn try_from(value: MoonValue) -> Result<Self, Self::Error> {
        match value {
            MoonValue::Array(mut moon_values) => Ok(
                Self{
                    name: String::try_from(moon_values.remove(0)).map_err(|_|())?,
                    age: u16::try_from(moon_values.remove(0)).map_err(|_|())?,
                }
            ),
            _=>Err(())
        }
    }
}
````

<br>

If these requirements are met, then it's type can be used when defining
constants, functions parameters and return types, or input variables, 
for example, this way you can create a constant value of ``MyType`` and
also create a getter function that gets a value of it:

````rust
let mut engine = Engine::new();

// Create a value for the type
let my_type_example = MyType { name: "Jorge".to_string(), age: 23 };

// Create a constant with said type
engine.add_constant("BASE_HUMAN", my_type_example.clone());

// Create a getter for the field age
engine.add_function(FunctionDefinition::new("age", |value:MyType|{
    value.age
})
    // The function is associated to the type 'MyType', this means the function age
    // will work as a property, so instead of calling 'age(BASE_HUMAN)',
    // we write 'BASE_HUMAN.age', for more information about properties, check the
    // properties secion of the user's guide
    .associated_type_of::<MyType>());

// Create and execute a script that uses the custom constant and function, where
// it gets the age of the human
let age : u16 = engine.parse("BASE_HUMAN.age", Default::default())
    .unwrap().execute().unwrap().try_into().unwrap();

assert_eq!(my_type_example.age, age);
````