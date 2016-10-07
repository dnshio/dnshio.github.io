---
layout: post
title: "String concatenation in Rust"
description: Most trivial things in high-level languages can be quite complicated in low-level languages
modified: 2016-10-07
category: Music
tags: [programming, rust]
mathjax:
---
So I've been messing with [Rust language](https://www.rust-lang.org) for a couple of weeks now and I am thoroughly enjoying
it. Rust is a "systems programming language" (i.e: languages that compile down to machine code and does not require a runtime)
developed by Mozilla foundation (same dudes as Firefox browser). Systems languages are best suited for writing software that
directly control hardware such a drivers and embedded software. C and C++ generally carried the flag in this area since I
was born. And since the late 90's the focus shifted from mostly systems programming to applications programming. This shift
in focus meant that more and more higher level programming languages were invented since then - Java, Python and Ruby being
examples of those.

Rust is very new. Version 1.0 (stable) was released in the summer of 2015. What this means is that there is now a systems
language out there whose syntax is totally different to C. It borrows interesting concepts from functional languages like
Haskell, without the cost of abstraction (because it's compiled down to machine code). It's multi-paradigm so you can mix
functional and imperative styles of code. i.e you can still loop using an accumulator AND fold and map over collections.

But one of the biggest selling points of Rust is guarantees safety against [Segment faults](http://stackoverflow.com/questions/2346806/what-is-a-segmentation-fault).
It does so by eliminating the root causes of seg faults: aliasing and mutation. In other words, seg faults happen when
there are multiple references to a single segment in the memory and more than one of those references has the ability to
change the data. Rust solves this problem by introducing the concept of "Ownership and Borrowing" whereby ensuring that
there is only ever a single mutable reference to a location in memory.

Anyway.. all this is great but writing Rust forces you to do something that you don't normally do when working with languages
like Java, Python, Ruby and ahem... PHP, which is thinking about your data as pieces of information that exists *physically*.
This is because Rust doesn't have a garbage collector and you know exactly when memory is freed (as soon as a value goes
out of scope). This is great because even the worst code that you write in Rust would end up running faster than the best
code that you write in a high-level language. In fact, this is one of the reasons why Rust was invented in the first place.

But all this control sometimes makes it very difficult to do some of the most trivial things in programming. Take string
concatenation for example.

This is how you do it in Javascript:

```javascript
var x = "Hello ";
var y = "World";
var xy = x + y;
```

And in PHP:

```php
<?php
$x = "Hello ";
$y = "World";
$xy = $x . $y;
```

So you would think that the following would work in Rust:

```rust
let x = "Hello ";
let y = "World";
let xy = x + y;
```

The code above won't compile. This is because Rust has 2 types of strings. `&str` and `String`. The code above declares
two `string literals` whose type is `&str`. These are also called `string slices` (or static strings). This means that
string slices have a fixed size and cannot be mutated.

>   A string literal is a string slice that is statically allocated, meaning that it’s saved inside the compiled program,
    and exists for the entire duration it runs. The reference binding is a reference to this statically allocated string.
    Any function expecting a string slice will also accept a string literal.

So what about `String`? Strings are allocated on the heap and can be mutated. It's essentially a vector of unicode chars.
`Strings` are generally created from a `&str` by using to_string() function. So lets try again:

```rust
let mut x = "Hello ".to_string();
let mut y = "World".to_string();
let xy = x.push_str(y);
```

That doesn't work either. It'll give you `expected &str, found struct ``std::string::String`. Ok. So how about this then:

```rust
let mut x = "Hello ".to_string();
let mut y = "World".to_string();
let xy = x + y;
```

Again, the compiler will throw the same error as before. So in order to get around it, you have to dereference one of the
`Strings` to obtain the `&str` version of it. This is done easily by prefixing an ampersand to the second argument, which
essentially dereferences the `String` and passes a pointer of the `string slice` to the concat function. So the working
version of the previous example would look like this:

```rust
let mut x = "Hello ".to_string();
let mut y = "World".to_string();
let xy = x + &y;
```

If you are making a call to a function that's returning a `String` and you want to concatenate it to an existing string
then you just add the ampersand to the beginning of the function call.

```rust
fn get_world() -> String {
    "World".to_string()
}

fn main() {
    let mut x = "Hello ".to_string();
    let mut xy = x + &get_world();
}
```

Sometimes life can be hard like that.