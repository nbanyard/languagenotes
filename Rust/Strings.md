# Strings
## Types
Strings (`String` and `str`) are UTF-8 encoded sequences of bytes, compared to `char` which is a Unicode character.

`str` is in the core language, generally used to access slices.

`String` in standard library is growable. It implements many of the methods of `Vec<T>`. The implementation contains a `Vec<u8>`, as such the capacity, length and pointer are on the stack, the string data are on the heap.

## Creating and Manipulating
Creating strings

```rust
// Create an empty but growable string
let mut s = String::new();

// Types implementing `Display` trait have `to_string` method
let s = "initial contents".to_string();
let s = String::from("initial contents");
```

Updating strings

```rust
let mut s = String::from("foo");
s.push_str("bar");
s.push('!');

// Note s1 is not usable after string concatenation
let s1 = String::from("Hello ");
let s2 = String::from("World!");
let s3 = s1 + &s2;

let s = format!("{}, {}", s1, s2);
```

Accessing characters and sub-strings. Strings are implemented as `Vec<u8>`, since indexing would return individual bytes not characters and this is not normally what is required, indexing is not supported. 

```rust
// Slicing strings is supported, but done in bytes
// The program will panic if the slice is not done on a char boundary
let hello = "Здравствуйте";
let s = &hello[0..4]; // s == "Зд"

// c will be a char - string is consumed in whole unicode codepoints,
// but some codepoints are diacritics which may not make sense on their own
for c in s.chars() {}

// b will be u8
for b in s.bytes() {}
```

The argument to `push_str()` is a string slice and is usable after the call.

String handling crates are available.

## String parameters

When defining a function that takes a string parameter that it does not modify, generally use `&str`.  This means: the value is borrowed so the ownership is not changed, the value is passed by reference so no copy is made; `String` values are automatically turned into `&str` using the `Deref` trait 