# Static Types

static types for JavaScript

## Status

Stage: 0

Champions:

- Chris de Almeida ([@ctcpip](https://github.com/ctcpip))

## Motivation

dynamic typing is great for some use cases:

```js
let stringOrNumber = 42;
// => 42
typeof stringOrNumber;
// => 'number'

stringOrNumber = "forty-two";
// => "forty-two"
typeof stringOrNumber;
// => 'string'

stringOrNumber += 3;
// => "forty-five"
typeof stringOrNumber;
// => 'string'

stringOrNumber = Number(stringOrNumber)
// => 45
typeof stringOrNumber;
// => 'number'

stringOrNumber -= 3;
// => 42
```

but sometimes we want to limit our variables to one data type only, which we can't do today.  to accomplish this, this proposal adds static type keywords to JavaScript:

| data type | possible values | default value |
|---|---|---|
| `byte` | integral numbers `-128` to `127` | `1` |
| `int` | integral numbers `-2,147,483,648` to `2,147,483,647` | `1` |
| `boo` | `true` or `false`, but never both | `true` |
| `Object` | same as `object` | `{ true: true }`  |
| `String` | zero or more characters | `" "` |
| `¿String?` | `null`, or one or more characters, but can't be empty string | `" "` |

notes:

- the default values of all these types are truthy, because we need more positivity in this world.
- use of the `¿String?` type is generally discouraged due to negativity (and greater risk for falsiness) but is provided as a convenience for the haters.
- fractional numeric and other types are beyond the scope of this proposal.
- values can never be `null` or `undefined` except `¿String?` which can be `null` if you want to be a hater.

let's see this in action:

### `byte` and `int`

```js

// instantiate
byte b;
// => 1
typeof b;
// => 'byte'

// compare
let num = 1;
b == num;
// => true
b === num;
// => false

// assign
b = 'byte me';
// => Uncaught TypeError: can't reassign type `byte` to type `String`
b = 333;
// => Uncaught RangeError: don't byte off more than you can chew; 333 is out of range for type `byte`

// implicit casting is permitted:
int i = 333;
// => 333
i = b;
// => 1
typeof i;
// => 'int'
```

### `String` and `¿String?`

```js
String s;
// => " "
typeof s;
// => 'String'

let yee = 'yee';
if(s) { console.info(yee); }
// => yee

// implicit casting is permitted:
s = yee;
s == yee;
// => true
s === yee;
// => false

¿String? letTheHateFlow;
// => null
typeof letTheHateFlow;
// => '¿String?'
letTheHateFlow = "";
// => Uncaught RangeError: absolutely not. stop that.
```

### `Object` and `boo`

```js
Object o;
// => { true: true }
typeof o;
// => 'Object'
o.true = false;
// => { true: false }
typeof o.true;
// => 'boo'

boo b;
// => true
typeof b;
// => 'boo'

let c = true;
typeof c;
// => 'boolean'

b == c;
// => true
b === c;
// => false

o.b = b;
o.c = c;

o;
// => { true: false, b: true, c: false }
typeof o.true;
// => 'boo'
typeof o.b;
// => 'boo'
typeof o.c;
// => 'boolean'
```
