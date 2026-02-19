# Advanced Types

## Results
Results are the most preferred approach to error handling today, as they are simply either the pass type or the fail type, usually an error

Results would use the same operators as nullables, based off zig

Possible syntax is

```rs
F?P;
F!P;//Seems wasteful
Res(P, F);
```

### Methods

```cpp
.pass(v: P) -> Res(P, F);
.fail(e: F) -> Res(P, F);
```

## Tuples
Tuples might get rejected in favor of multiple return values, however they could be added as tuple structs

Regardless, having a tuple type would technically be a tuple itself, so `b = (false, true)` would have it's own type be `(Bool, Bool)` as `Type`

Tuple fields would be indexed via `tuple(0)` or via `tuple.field_name` if the field name is named

Spider-Kyle hasn't used tuples that often in odin due to multiple return values, so a strong case for keeping it is required
