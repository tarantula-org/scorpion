
## Primitive Types

### Floating point numbers
Existing float types are:
- f16
- f32
- f64
- f80
- f128

Similar to ints, smaller floats coerce into larger floats implicitly but not the other way around

```rs
f1: f32 = 1.23;
f2: f128 = 99999.4;
```

### Fixed point numbers
Todo

### Booleans
Bools can have different bit width like ints, but the one you probably want the most is `Bool` which tries to take as least space as possible

Like ints, bools can coerce to other bools implicitly, but unlike ints bigger bools coerce into smaller ones implicitly since it's impossible for a bool to have a value other than true/false

```swift
b1: Bool = true;
b2: b32 = false;
```

### Strings
Strings exist in 4 Types, these are the same as their C++ equivalents

`Str` which is a dynamic array of characters
`VStr` a readonly view of strings
`CStr` a C string
`FStr` a fixed size string

```swift
cs: CStr = c"Hello world!";
s: Str = "Hi mom!";
vs: VStr = v"Look at me!";
fs: FStr = f"I am stale";
```
	
### Pointer
Pointers `^T` are variables that point to other variables of type `T`, they are prefixed with `^` so an `^i32` is a pointer to an int variable of bit width 32

Pointers can also point to other data on the heap, but this will be explained later

Pointers in scorpionest can't be null

```rs
mut i: i32 = 2;
mut p1: ^i32 = &i;
```

### Nullables
A nullable `?T` is a type that can have either `T` or `null`

```swift
maybe: ?Bool = null;
```

#### Pointers and Nullables
With what we have, we can have nullable pointer like `?^T` but beware not to mistake it for `^?T` which is a pointer to a nullable

```rs
i = 1;
mut p: ?^T = &i;
p = null;
```

### Results
A Result is a type that has either a `T` or an `E` where `E` is a enum that inherits the `Error` trait, errors and handling them will be explained later

```swift
r1: Result(i32, MathError) = .DivisionByZero;
r2: Result(b42, BoolError) = true;
```

### VoidPtr
A pointer that points to any data, this is similar to C's `void*`

```swift
v1 = 2;
v2 = false;
mut ap: VoidPtr = &v1;
ap = &v2;
```

### Types of Types
While confusing, having types as variables is useful, you pass type into a variable with type `Type`, this can be useful in generics which will be explained later

`Type` is technically a union of `StructType`, `EnumType`, etc

```py
t: Type : i32;
```

### Arrays
Arrays are a fixed size collections of data with type `T` with size `s`, accessing and modifiying arrays will be explained later

```swift
arr: [3]i32 = {1, 2, 3};
```

### Slices
Slices are pointers to an array with a length, they can be appended to 

```swift
arr: [3]i32 = {1, 2, 3};
sli1: []i32 = arr;
sli2: []i32 = arr[1:2];
```

#### MultiPointers
Multipointers are just pointers that can be indexed, they aren't recommended to use and only exist for C compat, they can be declared with `[^]T`

### Vectors
Vectors are dynamic size collections of data with type `T` and can be expanded in size later

```swift
vec: Vec(Bool) = {true, false, true};
```

### Maps
A `Map(K,V)` is a type similar to a list, but instead of accessing via an int index, it is accessed via a key of type `K`

```swift
map: Map(Str, i32) = {"Hello": 4, "World": 54};
```

### Sets
Sets are collections that only allow 1 instance of the same value

```swift
s: Set(Bool) = {true, false};
```

### Ranges
A range represents a start and end, with it's `T` being an int

```swift
rang: [:]u8 = iota[3:4];
```

## Operators
Operators having a precedence is todo right now

### Math operators
The basic Math operators `+ - / *` from other languages exist in scoprionest, with the only exception of the modulus, which has been split into functions

```swift
my_var = (3 * 4) + (5 / (3 - 4));
m = modf(4, 2);// modulus floored
```

### Logic operators
Scorpionest supports `and`, `or`, `xor` and `not` for bools and other truthy types

```swift
is_human = breathes and drinks_water;
is_in_pain = hurt_physically or sick;
hungry = not(full);
dev = backend xor frontend;
```

### Bitwise operators
Logic operators also work as bitwise operators, others are functions

```swift
x = shr(1, 1) and 4;
y = not shl(3, 4);
```

### Array programming
Arrays of size `2..4` can be used as Math Vectors! Possible values to access are
- `x y z w`
- `r g b a`
- `h s v a`
- `s v`

```swift
Vec2 :: [2]f32;
v1:= Vec2{1, 2};
v2:= Vec2{3, 1};
v3:= v1 * v2;
v4 := v3.yx; //Swizzling
```

### Null coalescing operators

For nullables there exist 2 operators, `?=` which only assigns if the left hand value is null and `??` which assigns a default value if the previous expression is null

```rs
mut n: Bool = null;
n ?= null ?? true;
```

### Try catch
These are derived from Zig
```cpp
t := try getVal();
c1 := getVal() catch other;
c2 := getVal() catch(err) {...};
```

### Default values
Scorpionest doesn't allow default values, every variable must be assigned either
- Value
- Zero/Default value
- Garbage

```go
v:i32= 12;
z:i32= 0;
g:i32= ---;
```

## Declartions and imports

### Variables
There are 3 types of variables:
- Mutable
- Immutable
- Constant

each decalared
```rs
mut m := 1;
im := 1;
c :: 1;
```


### Importing
Import packages using `#import`

```js
m :: #import "core:Math";
core :: #import "core";
io :: core.io;
```

## Control flow

### Do
Do refers to a single statement, this mostly used with other control flows and serve as a single statement alternative to blocks, declare a do statement by `=>`

```kt
mut i = 1;
=> i += 1;
```

### Blocks
Blocks allow you to encapsulate statements into smaller pieces of code which may return a value, variable shadowing is also supported

```rs
let i = 1;
{
  let i = 2;
  io.puts(f"{i}"); //2
}
io.puts(f"{i}"); //1
```

### When
When is similar to a hybrid between `if` and [`switch`|`match`] from other languages, you can use `when` with a `bool` as an if or use with a matchable as a switch

```kt
//if
when b => #nope;
elif not(b2) => #nope;
else => #nope;
//Switch
when i {
  0 => #nope;
  1, 2 => #nope;
  #fall 3 => #nope; //fallthrough
  4 => #nope;
  else => #nope;
}
```

### Loops

Loops only exist 1 version, but can be extended to support iterators and conditions, they can be compared to C++ as folows

```rs
//C++
while (true) {}
//Scorpionest
loop {}
```

```rs
//C++
while (cond) {}
//Scorpionest
loop if cond: {}
```

```rs
//C++
for (int i = 0; i < len; i++) {}
//in scorpionest
loop e in iota[:len] {}
```

```rs
//C++
for (auto item : collection) {}
//in scorpionest
loop e in list {}
```

```rs
//Iterate by reference with index
loop &e, i in list {}
```

Loops in scorpionest can be exited early or skip a loop

```rs
loop i in iota[:600]{
  when modf(i) == val{
    case 2 => skip;
    case 10 => stop;
    else => #nope;
  }
}
```

### Defer

Defer executes it's piece of code after the block ends

```go
lib.init();
defer lib.deInit();
```

#### ErrDefer

ErrDefer is similar to defer, but only executes when the function fails

```cpp
let obj_ptr = alloc.new(i32);
errdefer del(obj_ptr);
try useInt(obj_ptr.^);
```

### Scope

Scope tells a function to automatically call another one when the function scope ends, this can be used as a single line alternative to defer

```beef
scope lib.init();
```

## Procedures
Procedures can be declared via the `proc` keyword, and require a return value type, the most used ones are `Void` which means no return value and `Unit` which means errors maybe returned, parameters follow the name first type second rule

```py
main :: proc() : Unit{
  callMe(1, 2);
};

callMe :: proc(i1, i2: i32) : i32 => return i1 + i2; 
```

There exist some types which are exclusive to function returns/parameters

### Params

Params is a way to have vardiac arguments in a nice way, they are passed as a collection and can be iterated at compile time

```py
call_varargs :: proc() Unit do _ = try get_one({1, 2, 3});

get_one :: proc(nums: Params(usize)) Result(usize, ValueError) {
  loop i in nums => when i == 1 => return i;
  return .ValueNotFound;
}
```

This also means parameters can be passed after varargs, unlike other languages

### Lambda/Closure

Functions can also be local/anonymous

```rs
get_one := proc() : i32 => return 1;

def out_func := proc() : Void{
  one := 1;
  closure := proc() : Void{
    io.puts("{}", one);
  }
}
```

### Generics

Generics allow passing different types to the same parameter, there exist 2 types of generics, implicit and explict

```rs
//explicit
exp :: fn($T: Type, v: T) T => return v;
//implicit
imp :: fn(v: $T) T => return v;
```

### Pipe operator
The `|>` operator can be used to pipe output from a proc to another

```haskell
get_even(items)|>power_2();
```

### Overloading
you can declare overloadable procs

```rs
area :: proc{circle_area, square_area};

overload rect_area -> area; 
```

## Structs
Structs are data containers, they are first class types and of type `StructType`

```rs
Dog :: struct{
  iq: i32,
  size: f64,
  sub_dog : struct{
    parent_dog: ^Dog,
  },
};
```
Tuple and Tag structs are also supported

```rs
Tuple :: struct(a, b: i32);

Tag :: struct;
```

Constants can be assigned to structs you own

```rs
FoodStand :: struct{
  food_amount: u8,
};

FoodStand -> MAX_FOOD :: 3;
```

Procedures can be assigned too

```rs
Printer :: struct;

Printer -> Print :: proc() : Void => io.puts("Hello!");
```

you can call it like `Printer.Print();`, if a proc has the parent struct as it's first parameter and has a `@Method` attribute, it can be called like a method

```rs
Putter :: struct(p: i32);
//Self refers to the currect type
Putter -> Push :: proc(this: ^Self) : Void => this.p += 1;
putter := Putter(1);
putter.Push();
```

Methods can also be declared externally via `@Method`, these aren't overridable however

```
@Method
MyMethod :: proc(i: ^i32) : Void => i.^ = 4;
```

### Embedding
You can embed structs inside other structs, this can be used for pseudo inheritance

```rs
Entity :: struct{
  pos: Vec3,
  vel: Vec3, 
};

Player :: struct{
  embed Entity,
  name: VStr,
}

```

## Distinct types
A type can declared distinct by `#distinct`, you can override procs related to that type with `override`, a distinct type can be cast to it's original type or other distinct types with the same parent, distinct types can be useful to not confuse values (ex: time, distance, entities)

```swift
Remover :: #distinct Putter;
//Requires same signture
override Remover.Push :: proc(this: ^Self) : Void => this.p -= 1;
remover := Remover(2);
remover.Push();
putter := Putter(remover);
putter.Push();
```

## Unions
Unions are types that can be mutliples times, but only 1 active at a time, you can match them in a `when` block

```rs
Primitive :: union{
  Boolean :: b32,
  Integer :: i32,
  Floating :: f32,
  //Error :: f32,
};

p: Primitve = true;//Implicit conversion from b32 to Primitive's Boolean 

when e in p{
  .Boolean => #nope;  
  .Integer => #nope;
  .Floating => #nope;
}
```

## Enums
Enums are a limited ints, enums can also be embedded into other enums and they can use the `.EnumValue` shorthand syntax

```swift
Fruit :: enum{
  Apple,
  Orange,
  Banana,
};

Plant :: enum{
  embed Fruit,
  Flower,
  Cactus,
};
```

Enum arrays are also supported

```
tasty: [Fruit]Bool = {
  .Apple = true,
  .Orange = true,
  .Banana = true,
};
```

### Bitset
An enum for C compat can be prepended `@Bitset` to turn it into a bitpack for C compat

```swift
@Bitset
WinFlags :: enum[c.int]{
  resizable,
  fullScreen,
  closable,
};

wf := WinFlags{
  .resizable = true,
  .fullscreen = true,
  .closable = false,
};
```

## Bitpacks
Bitpacks are a group of 1 bit booleans, they can be used for flags instead of multiple bools

```swift
Effects :: bit_pack{
  poison,
  healing,
  strength,
  fireproof,
};
```

## Generic types
Generic types are types that are returned from proc with a `@Type` decorator, to release the procedures related to the type outside, use `out`

```kt
@Type
Spanned :: proc($T: Type) : Type{
  ReturnType :: struct(
      value: T,
      span: [:]i32,
    );

  out ReturnType -> Init(value: T, span: [:]) => ...;
};

s := Spanned(i32).Init(1, iota[1:3]);
```

## Todo Case


## Decorator
Decorators are compile time, they can apply metadata, change structs or do anything! they are metaprogamming and designed for use everywhere

```kt
//First field is required
AddField :: deco(thing: StructType) => meta.addField(thing, "wow", i32);

@AddField
ShouldBeEmpty :: struct;

sbe := ShouldBeEmpty;
sbe.wow = 3;
```

### Common decorators
`@Soa` can be applied to a `[]T` where `T` is a struct to apply a optimision for SOA, this will only affect the backend compiling, not the writing, meaning no need to change anything

`@Simd` can be applied to a `[]T` to turn it into a Simd Vector

`@Packed` can be applied to a struct to pack it tightly

`@CUnion` can be applied to a struct to make it a C union

`@Function` is a mixture of `@NoDiscard`, `@NoSideEffects` and `@Constant`

`@SubRoutine` is an alias for `@NoReturn`

`@Local` can be applied to a proc field to make it static

`@Async` can be added to a proc to make it async

`@Comptime` can be made to ensure a proc runs at compile time always, if this is not always wanted, a `@ConstExpr` may be preferred

### Decorator fusing
Decorators can be fused, this may be wanted to make code shorter, but it's prefered to not be abused

```kt
//Arguments are concatented
MyDeco :: @OtherDeco + @SomeDeco;
```



