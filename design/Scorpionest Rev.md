## Primitive Types





### Ranges
A range represents a start and end, with it's `T` being an int

```swift
let rang: Range(u8) = Range(5, 10)
```

## Declartions and imports

### Variables

There are 2 ways to declare variables:
- `let`: A runtime constant
- `var`: a runtime variable
- `def`: A compile time constant

```swift
let rim = 1;
var rm = 5;
def cim = 54;
```

### Using

The `use` keyword is to alias expressions, it is similar to a more stable `#define`
```rs
use One = 1;
use Package = myPackage;
use Integar = i32;
```

## Control flow

### Do

Do refers to a single statement, this mostly used with other control flows and serve as a single statement alternative to blocks

```kt
var i = 1;
do i += 1;
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

### If
If works like any other langauge, but also supports using it as a switch statement

```kt

if cond {
  #nope;
}
elif obj is SomeType{
  #nope;
}
else do #nope;

//Switch/Match

if i == {
  0 => do #nope;
  1 => do #nope;
  2 => do #nope;
  else do #nope;
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
loop if not cond: stop {}
```

```rs
//C++
for (int i = 0; i < len; i++) {}
//in scorpionest
loop i in range(100) {}
```

```rs
//C++
for (auto item in collection) {}
//in scorpionest
loop i in list {}
```

Loops in scorpionest can also be a hybrid between a while and a for loop

```rs
loop x in list if x.cond: skip {}
```

Loops in scorpionest can be exited early or skip a loop

```rs
loop i in range(600){
  if modf(i) == {
    case 2 => do skip;
    case 10 => do stop;
    else do #nope;
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

## Functions

Functions can be declared via the `fn` keyword, and require a return value type, the most used ones are `Void` which means no return value and `Unit` which means errors maybe returned, parameters follow the name first type second rule

```py
def main = fn() Unit{
  callMe(1, 2);
}

def callMe = fn(i1, i2: i32) i32 do return i1 + i2; 

```

There exist some types which are exclusive to function returns/parameters

### Params

Params is a way to have vardiac arguments in a nice way, they are passed as a collection and can be iterated at compile time

```py
def call_varargs = fn() Unit do _ = try get_one({1, 2, 3});

def get_one = fn(nums: Params(usize)) Result(usize, ValueError) {
  loop i in nums if i != 1: skip do return one;
  return .ValueNotFound;
}
```

This also means parameters can be passed after varargs, unlike other languages

### Param Type Union
A parameter type union allows you one of multiple types, but if there is common usage between object (addable objects), then an interface may be more suitable 

```kt
def tasty = fn(plant: ParamUnion(Vegetable, Fruit)) Str{
  if plant is {
    case Fruit => do return "Tasty",
    case Vegetable => do return "Yucky"
  }
}
```

### Lambda/Closure

Functions can also be local/anonymous

```rs
let get_one = fn() i32 do return 1;

def out_func = fn() Unit{
  let one = 1;
  let closure = fn() Unit{
    io.puts("{}", one);
  }
}
```

### Generics

Generics allow passing different types to the same parameter, there exist 2 types of generics, implicit and explict

```rs
//explicit
def exp = fn(const T: Type, v: T) T do return v;
//implicit
def imp = fn(v: typedef T) T do return v;
```

## Structs and traits

```rs
def Animal = trait{
  def make_noise = fn();
}

def Dog = struct{
  mut pup: Bool
}

impl Dog{

  def cute(this): Bool{
    return this.pup;
  }

  impl Animal for Self{
    over make_noise(){
			puts("Woof!");
		}
  }
}

Cat :: struct(var scratches := false);

impl Cat{
  def scratch = fn(scratchable : Dyn^ = 1){
	  if scratchable.^ is Str and scratchable.contains("hiss"){
		  return
	  }
	//String formatting
  puts(f"Scratched ${scratchable}!!!");
  }
  
  impl Animal for Self{
    def make_noise = fn(){
	    puts("Meow?");
    }
  }
}

```


