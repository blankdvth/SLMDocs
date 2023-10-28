# Counting Documentation

This documentation is based on `v2.1.0`. (View the [Changelog](/changelogs/plugins/counting))

## Rules
- **Goal**: Count as high as possible, cooperatively (one person sends a number, someone else sends the next number, repeat).
- You may **not** send two numbers in a row by yourself.
- The count will reset to `0` whenever someone sends the wrong number, or sends twice in a row.
- Messages will be marked with a ✅ once successfully processed, if you see a message without a ✅, wait for it to be processed before sending another number. Messages from the bot itself is exempt from this rule.

## Supported Syntax
A wide variety of syntax is supported in counting. You can simply send the next number (e.g. `#!python 42`), *or* you can send an expression that evaluates to that number.

We use a minified version of the ***Python*** evaluator to evaluate the expressions -- a large amount of basic Python syntax is supported. Note that $\LaTeX$ syntax is **not** supported.

***Expressions must evaluate to an integer, if it evaluates to a float (decimal number), the expression will not be considered.***

### Operations
#### Arithmetic
<div class="annotate" markdown>

- `+`: Addition
- `-`: Subtraction
- `*`: Multiplication
- `/`: Division (1)
- `//`: Floor Division (2)
- `%`: Modulus (3)
- `**`: Power (4)

</div>

1. Will return result as a float (decimal) number. You may need to handle decimals.
2. Divide, then floor the result (no decimals).
3. Divide, and return the remainder. (e.g. `#!python 5 % 2` → `#!python 1`)
4. The `^` syntax to represent power is **not** supported. Use `**` instead. There is also a [limit](#quirks--limitations) to the size of power operations.

#### Logical
- `==`: Equality
- `<`: Less Than
- `>`: Greater Than
- `<=`: Less Than or Equal To
- `>=`: Greater Than or Equal To

#### Binary
- `<<`: Bitwise Left Shift
- `>>`: Bitwise Right Shift
- `&`: Bitwise AND
- `~`: Bitwise Invert

!!! info

    Syntax for bitwise XOR (`^`) and bitwise OR (`|`) was removed to prevent confusion with the power and absolute value operators respectively.
    
    This functionality has been replaced by the `bitxor()` and `bitor()` [functions](#functions) respectively.

#### Miscellaneous
- `not`: Negation - *negates a boolean value (`#!python not True` → `False`, `#!python not False` → `True`)*
- `in`: *whether something is contained within an iterable (`#!python "i" in "team"` → `False`)*

### Functions
*When you see a parameter in a function with `foo="bar"`, or similar, that parameter is optional -- the default value is the one after the equals sign*

<div class="annotate" markdown>

- [`int(value, base=10)`](https://docs.python.org/3/library/functions.html#int): *Converts from a variety of data types, to an `int`. If converting from `float`, this will truncate (ignore all numbers after decimal).*
- [`float(value)`](https://docs.python.org/3/library/functions.html#float): *Converts from a variety of data types to a `float`.*
- [`str(value)`](https://docs.python.org/3/library/functions.html#func-str): *Converts from most data types to a `str`.*
- [`floor(value)`](https://docs.python.org/3/library/math.html#math.floor) (1): *Rounds down.*
- [`ceil(value)`](https://docs.python.org/3/library/math.html#math.ceil) (2): *Rounds up.*
- [`round(value, digits=0)`](https://docs.python.org/3/library/functions.html#round): *Rounds (<= 0.5 down, > 0.5 up). You can optionally specify the number of digits to keep after the decimal point.*
- [`sqrt(value)`](https://docs.python.org/3/library/math.html#math.sqrt) (3): *Get the square root of `value`.*
- `sin(value)`: *Get the sine of `value` **degrees**.*
- `cos(value)`: *Get the cosine of `value` **degrees**.*
- `tan(value)`: *Get the tangent of `value` **degrees**.*
- [`degrees(value_radians)`](https://docs.python.org/3/library/math.html#math.degrees): *Convert `value` in radians to degrees.*
- [`radians(value_degrees)`](https://docs.python.org/3/library/math.html#math.radians) *Convert `value` in degrees to radians.*
- [`abs(value)`](https://docs.python.org/3/library/functions.html#abs): *Get the absolute value of `value`.*
- [`bitxor(a, b)`](https://docs.python.org/3/library/operator.html#operator.xor): *Get the exclusive bitwise OR (XOR) of `a` and `b`. Equivalent to `a ^ b`.*
- [`bitor(a, b)`](https://docs.python.org/3/library/operator.html#operator.or_): *Get the bitwise OR of `a` and `b`. Equivalent to `a | b`.*
- `log(value, base=10)`: *Get the log of `value`, to base `10` by default.*
- [`ln(value, base=e)`](https://docs.python.org/3/library/math.html#math.log): *Get the log of `value`, to base `e` (Euler's number) by default.*

</div>

1. The aliases `rounddown` and `round_down` can be used instead of `floor`.
2. The aliases `roundup` and `round_up` can be used instead of `ceil`.
3. The aliases `sqroot` and `squareroot` can be used instead of `sqrt`.

### Variables (Constants)
A few variables (constants) are provided, they cannot be changed (assignment forbidden).

- [`pi`](https://docs.python.org/3/library/math.html#math.pi)
- [`e`](https://docs.python.org/3/library/math.html#math.e)

### Base Literals

You can specify a number in another base by using the following prefixes before the number:

- **Binary**: `0b` (e.g. `#!python 0b1111`)
- **Octal**: `0o` (e.g. `#!python 0o17`)
- **Decimal**: *no prefix* (e.g. `#!python 15`)
- **Hexadecimal**: `0x` (e.g. `#!python 0xf`)

If you are not performing base-specific operations, and want everything to be an int, you can convert it. You can do this with the `#!python int()` [function](#functions).

If you have a base literal (e.g. `#!python 0b1111`), you can pass it directly into `#!python int()` (e.g. `#!python int(0b1111)`).  
If you have a string containing a number in another base, you have to specify the base (e.g. `#!python int("1111", 2)`).

### If-Else Expressions
Python's in-line if expressions are supported, using the standard format:
```py
option1 if boolean else option2
```
!!! example

    ```py
    100 if 2 > 1 else 0
    ```

    Would evaluate to `100` because `2>1`. If it were `1>2` instead, it would've evaluated to `0`.

## Quirks & Limitations
- **Power** operations are limited to ***`10000`*** (either base or exponent), to prevent timely calculations.
- **Iterables** are limited to ***`10000`*** in length, to prevent memory overloading.
- **Floats** (decimal numbers) are automatically rounded to ***`16`*** decimal places, this is a limitation of the evaluator.
- Certain functionality (imports, attributes starting with `_`, etc) are blocked for security reasons -- do NOT attempt to bypass these.
