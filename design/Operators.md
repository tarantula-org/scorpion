# Operators

Operators having a precedence is todo right now

## Math operators
The basic Math operators `+ - / *` from other languages exist, with the only exception of the modulus, which has been split into functions

Unlike other langauges, they don't panic when overflowing/underflowing, instead you can choose behavior to prevent this
- Wrap
- Clamp
- Math operators return a Result instead (really bad idea)

## Collection & Range operators

There exist 2 operators

- Index value
- Span slice

Possible syntax is

```cpp
col(n) = val;
val = col(n);
slice = col(s:e:i);
```

## Null operators

For nullables and result types, there is 4 operators

- value or else
- assign if not null
- try
- catch

Possible syntax is

```cs
val = null ?? 1;
val ?= 1;//Other side can't be null
val = getValOrErr()?;
val = getValOrErr() as? err => fmt.print("{0}", .{err});
```

```cpp
val = null catch 1;
val else= 1;
val = try getValOrErr();
val = getValOrErr() catch(err) => fmt.print("{0}", .{err});
```