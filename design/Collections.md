# Collections

## General information

Most syntax in this document follows 3 variants
- Builtin syntax
- Generic Type Syntax

and perhaps others

### Methods and Constants

```cpp
//All
len(c: C) -> I;
//Growing
cap(c: C) -> C;
//Int-indexed growing collections
pop(c: ^C) -> ?T;
push(c: ^C, v: T) -> ?^T, I;
consume(c: ^C) -> ?^T;// intended for loops where the key is consumed
//Slice
reverse(s: ^S) -> ^S;
sort(s: ^S) -> ^S;
copy(dst, src: ^S) -> ^S;//Returns dst
clone(og: S) -> S;
//Map
Todo();
```

## Arrays
Arrays are a fixed size collections of data with type `T` with size `s`, they are static and kept on the stack

Possible names for arrays are
- Array
- Bag
- Lot
- Row

Possible syntax is

```rs
[N]T;
Name(T, N);
``` 

### Bounded Arrays

A bounded array can be considered as a [Vector](#vectors) with an array backing instead of a slice backing, this means a bounded has a static max limit rather than depending on memory

Possible syntax is

```rs
[..N]T;
BoundedName(T, N);
```

### Enum Arrays

An enumerated array is one that uses enums instead of raw ints to index into it, this allows inference and labelling each field, can be used as a replacement for maps with string names

Enumerated arrays must be continuges, meaning the count starts from 0 and increments by 1

Bitset Arrays could be an option, todo: add BitSet Array

Possible syntax is

```rs
[EnumName]T;
Name(T, EnumName);
```

### Partial Enum Array
A partial enumerated array is one that doesn't use a continuges array and where every element doesn't need to be assigned

Possible syntax is

```rs
[..EnumName]T;
#partial Name(T, EnumName);
BoundedName(T, EnumName);
```

## Slices
A slice is an array whose length is runtime dependant rather than compiletime dependant, usually slices don't contain their own data but point to other data somewhere else, however you can create slices to create runtime array, see [Memory](Memory.md)

Possible syntax is

```rs
[]T
Slice(T)
```


## Vectors
Vectors are variable sized arrays, they have a backing slice and a len field, the slice's len field is known as a cap and always `cap >= len`, whenever the length increases, so does the cap

Possible syntax

```rs
[..]T;
[dyn]T;//Really depends if dyn is ever a keyword
[var]T;//Depend on if we have a var keyword
Vec(T);
```

## Maps
Maps are important types that are useful and will be added, they might be called either `Map` or `HashMap` depending on factors

Possible Syntax

```rs
[K]V;//Really problematic, esp with enumerated arrays
[.. as K]V;//Depends on Vector implementation
#map[K]V;
Map[K, V];//Generic Type approach
Map(K, V);
```

### Pair

## Sets
Sets are underrated collections that only allow 1 instance of the same value, while other languages depend on a map with a void value, this isn't really nice to work with

Possible syntax

```rs
[..]T with Solo;
[.. as Solo]T;
Set(T);
```

## Lists
Linked lists, could be a core library type rather than a builtin one

Possible Syntax

```rs
[^..]T;
List(T);
```
## MultiPointer
A pointer that can be indexed into, intended for unsafe development, casts implictiy from `^T`

Possible Syntax

```rs
[^]T;
#many ^T;//Heavily dislikable
MultiPointer(T);
```

## SOA
Struct of arrays is a useful optimisation strategy, it also could either be struct defined or array defined, struct defined info will be in [Struct Simd](Type%20Construction.md)

These would still be iterated over the same as normal arrays, only differing in compiled code

Possible syntax for array defined is

```rs
[]T as Soa;
[]T with Soa;
#soa[]T;
```

## Simd
Single Instruction multiple data is often useful for optimisations, implementing them in syntax is hard howeve

They would be defined for arrays, vectors and slices

Possible syntax is

```rs
[]T as Simd;
[]T with Simd;
#simd[]T;
```

## Array programming
Array programming allows using arrays as vectors, colors and other types, they also support numeric operators

### Types

- Math Vector, [x, y, z, w], len is 2-4
- Area, [width, height, length], len is 2-3
- TextureCoord, [u, v], len 2 (maybe more but i haven't gotten to more than 2 texturecoords yet?)
- Color, [r, g, b, a], len is 3-4

### Syntax
While these could be pre-defined as special structs (Vector2f32, Areau16, Coloru8), this would result in a mess (and possibly even require their own namespace), instead possible syntax is

```rs
[N]T as ArrayType;
[N, .ArrayType]T;
[N, ArrayType]T;
#arrayType("type")[N]T;
```

The most easy to looking syntax would be perhaps 1, as with simd it would be `[N]T as ArrayType with Simd`