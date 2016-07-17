# ES8 Proposal: "use stricter"

Current status of this proposal is -1. It's being work on alongside the optional static types proposal located here: https://github.com/sirisian/ecmascript-types

## Rationale

TODO

### "use stricter";

THIS SECTION IS A WIP

This is an extension of strict mode. Stricter mode would bring all the types into the language, without the use of import (as described below), and change typeof's behavior returning the actual type. In this mode the following would occur:

```js
let foo:(); // typeof foo == "()", a function with no parameters and return type any
let foo:():void; // typeof foo == "():void", a function with no parameters and no return type
let foo:(uint8):uint8; // typeof foo == "(uint8):uint8"
let foo:uint8[]; // typeof foo == "uint8[]"
let foo:MyClass; // typeof foo == "MyClass"
```

This relies on features like Array.isArray to be used to check if something is an array. Function.isFunction would be added to check if something is a function.

Similar to ES5's strict mode, stricter mode would change the semantics of the language. Binary operators which used to have simplified rules would now function intuitively.

```js
function Foo(a:float32, b:float32, c:float32)
{
    if (a < b < c) // a < b && b < c, no longer (a < b) < c
    {
    }
    else if (a < b == c)
    {
    }
}
```

```js
function Foo(x:uint8)
{
    if (0 < x <= 5) // Number < uint8 <= Number, Number casts to uint8 in both cases.
    {
    }
}
```

Types no longer lead to situations where the language can compare true or false to a non-boolean type. In stricter mode the grammar rules are effectively changed. (Other changes, like operator overloading, will probably change them also).

#### Automatic semicolon inseration (ASI) removal

This mode removes automatic semicolon insertion (ASI) rules. These rules are generally veiwed as a mistake which added unnecessary grammar to the language while allowing sloppy code. With this change is also a change to the grammar for continue, break, return, throw, arrow function, and yield. In stricter evaluation new lines are no longer an issue and braces are allowed to line up. As an example of a now valid and intuitive program:

```js
return
{
    a: 1
};
```

#### Trailing comma removal

ES5 added trailing commas. This mode removes the feature requiring more strict usage of undefined and trailing commas. Examples that are now syntax errors:

```js
let a = [,]; // syntax error
let b = [,,]; // syntax error
let c = {a:1,} // syntax error
```

# Required Changes to the Specification:

### 11.9 Automatic Semicolon Insertion

Make a comment that in stricter mode that ASI is not applied.

### 12.5.6 The typeof Operator

The table needs to be updated with all the new types and nullable type explanation.

### 12.10 Relational Operators
### 12.11 Equality Operators

In stricter mode the grammar rules and behavior of these operators change as defined before. This section needs to cover the new behavior with respect to all integer, floating point, decimal, and other types. (Determine at the very least which types must use the new rules).

### A.1 Lexical Grammar 

*FutureReservedWord* ::  
&nbsp;&nbsp;&nbsp;&nbsp;**await**

&nbsp;&nbsp;&nbsp;&nbsp;The following tokens are considered to be *ReservedTypes* when parsing stricter mode code. (See ??.??.??)

*ReservedTypes* :: **one of**  
&nbsp;&nbsp;&nbsp;&nbsp;**number** **bool** **string** **object** **symbol** **int8** **int16** **int32** **int64** **uint8** **uint16** **uint32** **uint64** **bigint** **float16** **float32** **float64** **float80** **float128** **decimal32** **decimal64** **decimal128** **bool8x16** **bool16x8** **bool32x4** **bool64x2** **bool8x32** **bool16x16** **bool32x8** **bool64x4** **int8x16** **int16x8** **int32x4** **int64x2** **int8x32** **int16x16** **int32x8** **int64x4** **uint8x16** **uint16x8** **uint32x4** **uint64x2** **uint8x32** **uint16x16** **uint32x8** **uint64x4** **float32x4** **float64x2** **float32x8** **float64x4** **rational** **complex** **any** **DataView** **Date** **Error** **EvalError** **InternalError** **Map** **Promise** **Proxy** **RangeError** **ReferenceError** **RegExp** **Set** **SyntaxError** **TypeError** **URIError** **WeakMap** **WeakSet**
