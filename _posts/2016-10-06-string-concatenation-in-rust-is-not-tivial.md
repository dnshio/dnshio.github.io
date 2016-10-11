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
it. Rust is a "systems programming language" developed by Mozilla foundation. It's becoming popular with plenty of rave reviews
about seg-fault safety and the modern syntax. I highly recommend taking a look if you haven't already.

Rust, being a systems language, gives you low level access to the system, giving you the control over how things are
laid out in memory. But all this control sometimes makes it very difficult to do some of the most trivial things in programming.
Take string concatenation for example.

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

### But WTF? why do you have to do that?
The simple answer is because how concatenation is done. When you are using the `+` operator for `Strings` in Rust, you are
essentially calling the [std::ops::Add Trait](https://doc.rust-lang.org/std/ops/trait.Add.html) on the `String` on the
left hand side. And this Trait is designed so that the left hand side argument should be `String` and the right hand side
argument should be an `&str`.

So now we understand that this is because of a particular design decision of the standard library. But why? Why not just
allow for two arguments like `add(a: String, b: String) -> String`?

This is basically down to how and where things are stored in the memory and the operational cost of mutating them.

* `impl<'a> Add<String> for &'a str` requires prepending, which is not as efficient as appending
* `impl Add<String> for String` needlessly consume two arguments when one is sufficient
* `impl<'a, 'b> Add<&'a str>` for &'b str hides an unconditional memory allocation


