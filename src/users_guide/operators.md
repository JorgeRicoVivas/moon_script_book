# Operators

## Unary Operators

| Operator | Operation       | Example               |
|----------|-----------------|-----------------------|
| ``-``    | Negates a value | ``---5`` = ``-5``     |
| ``!``    | Inverts a value | ``!false`` = ``true`` |

## Arithmetic Operators

| Operator | Operation               | Example         |
|----------|-------------------------|-----------------|
| ``+``    | Adds two values         | ``1+2`` = ``3`` |
| ``-``    | Substracts two values   | ``3-2`` = ``1`` |
| ``*``    | Multipies two values    | ``2*3`` = ``6`` |
| ``/``    | Divides two values      | ``4/2`` = ``2`` |
| ``%``    | Remainder of a division | ``5%3`` = ``2`` |

## Logic / Bitwise Operators

| Operator | Operation                                                   | Example                        |
|----------|-------------------------------------------------------------|--------------------------------|
| ``&&``   | AND Logic gate                                              | ``true && false`` = ``false``  |
| ``\|\|`` | OR Logic gate                                               | ``true \|\| false`` = ``true`` |
| ``^``    | XOR Logic gate                                              | ``true ^ true`` = ``false``    |
| ``<<``   | Shift n times to the left                                   | ``1<<1`` = ``5``               |
| ``>>``   | Shift n times to the right                                  | ``5>>1`` = ``1``               |
| ``==``   | Compares if two values are equal                            | ``5==5.0`` = ``true``          |
| ``!=``   | Compares if two values are different                        | ``5!=5`` = ``false``           |
| ``>=``   | Compares if first value is greater or equal than the second | ``5>=6`` = ``false``           |
| ``<=``   | Compares if first value is lower or equal than the second   | ``5<=6`` = ``true``            |
| ``>``    | Compares if first value is greater than the second          | ``5>5`` = ``false``            |
| ``<``    | Compares if first value is lower or equal than the second   | ``5<5`` = ``false``            |


## Optimizations

All operators are essentially functions, and they are all also constant,
that when the values they entail are constant (known at compile-time),
their results will be inlined as it happens with functions.

<table>
<thead>
<td> Input </td> <td> Moon Script Optimization </td>
</thead>
<tr>
<td>


````rust
return 10 + 5
````

</td>
<td>

````rust
return 15
````

</td>

<tr></tr>
<td>


````rust
let num = 15
let comparasion = num > 10
return comparasion
````

</td>
<td>

````rust
return true
````

</td>

</tr>
</table>