# Data Type

## Data type

- JavaScript has **dynamic type**: data types are automatically assigned to varibales.

***Primitive Type***

- Directly stored in **stack**, taking small and fixed memory size.

***Reference Type***

- Stored in either **stack** or **heap**, taking larger and unfixed memory size. Storing the reference of memory address in **stack**, which points to actual value in **heap**.

### Primitive JavaScript Data Types

In JavaScript, a primitive (primitive value, primitive data type) is data that is **not an object** and has **no methods**.

- number
    - Floating point numbers, for decimals and integers
    - `var age = 18;`
- string
    - Sequence of characters, used for text
    - `var name = 'Wu';`
- boolean
    - Logical data type that can only be **true** or **false**
    - In boolean expression, `1` is `true`, `0` is `false`
    - `var isTure = true;`
- bigint (ES10)
- symbol (ES6)
    - For declaring an unique value
- undefined
    - data type of a variable that does not have a value yet
    - `var undefinedVar;`
- null
    - also means 'non-existent'
    - `var nullVar = null;`
    - which is **seemingly** primitive, but indeed is **a special case** for every Object: and any structured type is derived from null by the Prototype Chain.

### Primitive wrapper objects in JavaScript

Except for `null` and `undefined`, all primitive values have object equivalents that wrap around the primitive values:

- `[String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)` for the string primitive.
- `[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)` for the number primitive.
- `[BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)` for the bigint primitive.
- `[Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)` for the boolean primitive.
- `[Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)` for the symbol primitive.

The wrapper's `[valueOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)` method returns the primitive value.