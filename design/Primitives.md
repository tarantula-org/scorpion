# Primitives

## Integers

### States
Ints exist in 2 states commonly: signed, unsigned

- Signed: has both negative and positive numbers
- Unsigned: no negative numbers but more positive numbers than signed

Syntax would be `[I|U]N`, where `N` is the size

Ant alternative to `I` is a `S`, this is less common but might be more clear

### Size

Mandatory int sizes are 8, 16, 32, 64 and 128

A variable bit width is possible, from 1 to 65535, this can eleminate the need for bitfields

### Special ints

Ints also have a `[I|U]size` that differs from your pc's architecture

A `UIntPointer` type could be useful for low level development

Compile time ints might be useful, but considering compile time metaprogamming isn't planned, we are not sure

### Conversion

Int coercon is an important is debated, but it's found to simplify development and make working alot less stressful

Ints would coerce into bigger types, for example `U8` -> `U16`

Unsigned ints could coerce into bigger signed types, for example `U8` -> `I16`

Ints coercing into floats is debatable, WIP


### Possible Aliases

`Byte` for `U8`

### Methods & Constants

Basic Math operator are used, check out `Operators.md` for more info

There are also

```cpp
//Math
neg(val: I) -> I;
rem(val: I) -> I;
mod(val: I) -> I;
// Bitwise
not(val: I) -> I;
shl(val: I) -> I;
shr(val: I) -> I;
and(a, b: I) -> I;
or(a, b: I) -> I;
xor(a, b: I) -> I;
//Logic
truthy(val: I) -> B;
//UIntPointer only
offsetOf[T](member: M) -> UIntPointer;
//Constants
MAX :: meta.max(I);
MIN :: meta.min(I);
```

## Floating point numbers

### States
Floats normally are signed, however Possibly unsigned floats may be useful, for example with color values for extra precision when bytes are unavaliable

There are multiple float specifications, we might choose specific ones that are useful for graphics programming

### Size

Existing float types are:
- F8 (intended to be in range of 0-1)
- F16
- F32
- F64
- F80 (possibly?)
- F128

A `FSize` type may be useful, should be easy to add, however this needs more research for if it's even needed

### Conversion

Float coercion is possible, however it depends 

Implicit large to small (`F64` -> `F32`) casting would be a no-go, as it would result in loss of data, however small to large may be added if there is no loss precision

### Method & Constants

```cpp
//Math
neg(val: F) -> F;
rem(val: F) -> F;//More variants perhaps
mod(val: F) -> F;//More variants perhaps
//Constants
INF :: meta.inf(F);
NAN :: meta.nan(F);
MAX :: meta.max(F);
MIN :: meta.min(F);
```

## Booleans

### Size

Bools typically exist as 1 byte, however there is possibility of larger bools and variable bools

Static bool types are:
- B8
- B16
- B32
- B64
- B128

Bools could be variable in width, so there might be a `B13`, `B70` and `B1`, however the use of these is to be debated

A `BSize` type is not viewable as useful, but if needs are seen, it might be added

`Bool` could be 1 or 8 bits in implementation, as a shortcut for the smallest BoolSize

### Conversion

Unlike ints and floats, any bool type can implicity convert to another, as only they all share the same value range

### Method & Constants

```cpp
//Logic
not(val: B) -> B;
and(a, b: B) -> B;
or(a, b: B) -> B;
flip(val: ^B) -> B;//Flips bool value and returns it
//Special conversion
toInt(val: B) -> I;//Int type is inferred from assign/return
toFlt(val: B) -> F;//Float type is inferred from assign/return
```

## Pointer
Pointers would use the `^T` syntax

Pointers in scorpionest can't be null, and must be assigned, another proposal is to have `free` procedures remove the pointer from the current scope

To get the address of a value, use `&v`, functions of course that return pointers can return it, if better syntax, exists, it would be preferred, planned ideas right now

- `v&`

The syntax to dereference a pointer would either be `p^` or `p.^`, which is better is up to debate

See [Memory](Memory.md) for more useful

### Void Pointer
A pointer that points to an address without any knowledge of what it is, is unsafe, can be casted from and to `^T`

Possibly names are

- RawPtr
- VoidPtr
- Addr

### Methods & Constants

```cpp
//Safe
ownerOf[T](m: $M) -> ^T;
copy(p: ^T, deep: Bool) -> ^T;
//Unsafe
fromUIntPointer[T](val: UIntPointer) -> ^T;
isValid(p: ^T) -> Bool;
forceDerefGet(p: ^T) -> T;
forceDerefSet(p: ^T, val: T);
INVALID_POINTER :: 0;
```

## Nullables
A nullable, also known as an option or a maybe, it a type that either contains a value or no value

They use the `?T` syntax

A possible none value keyword is `null` and `nil`, `null` is more common but `nil` is shorter

Unlike other langauges, you can't force get the value, you must check it always, this is to ensure there are less panics and bugs than wanted

Possible nullables deOption syntax would be

```kt
when n as v => do(v); 
```

```
when v = n => do(v);
```

### Null pointers

A nullable pointer would use the syntax `?^T`, this allows the developer to have null pointers and also mandate it exists before derefencing

`^?T` should not be confused for a nullable pointer, as it is a pointer to a nullable

### Methods & Constants

See [Operators](Operators.md) for operators for result (since they can also be used for result)

```cpp
//Constants
None //If constant value is preferred over keyword
```