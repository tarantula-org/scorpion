# Comments

## Basic Comments

These must be included, as they help with in-code documentation, debugging, and reminders

Syntax is

```cs
//Hi!, I am a comment
```

Another possible syntax, although less preferred as `~` could be used for something else

```
~Hi!, I am a comment
```

## Multiline Comments

These could be added and have potential uses, but most code editors allow multiline commenting via Ctrl+K

Possible syntax

```cs
/*
Line 1
Line 2
Line 3
*/
```

Another syntax, with mandatory new lines

```cs
//*
Line 1
Line 2
Line 3
//*
```

## Item documentation

Items documentation is used to describe items such as data and procedures

They could have a normal comment before them

An explicit syntax could be

```
//! a variable with a value of 1
let i = 1;
```

##  Module documentation 

Module documentation is used to describe what a module does,this might also be file local, so more of Module/File documentation

It might depend if there is a `package` or similar keyword

```java
//Contains pure functions and constants for math
package math
```

If not, then `//#` would be possible, and must be declared the first thing in the file

```cs
//# This module gives access to Foo and Bar,
```