# Generic Types
```rust
// Example generic function (no constraints on T)
fn my_function<T>(list: &[T]) -> &T {}

// Example generic structure
struct Point<T> {
	x: T,
	y: T,
}

// A generic implementation for above
impl<T> Point<T> {
    fn x(&self) -> &T {
	    &self.x
	}
}

// A method just for Point<f32>
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
	    (self.x.powi(2) + self.y.powi(2)).sqrt()
	}
}

// Multile type parameters
struct Point<T, U> {
	x: T,
	y: U,
}
```

# Traits

```rust
// Trait definition
pub trait Summary {
    fn sumarize(&self) -> String;
	
	// Method is default implementation
	fn byline(&self) -> String {
	    String::from("anon.")
	}
}

// Trait implementation for some type
pub struct NewsArticle {
    ...
}

impl Summary for NewsArticle {
    fn sumarize(&self) -> String {
	    ...
	}
}
```

A trait can only be implemented for a type in the crate that defines the trait or the crate that defines the type.

A default implementation may call other methods that have no default implementation. Default implementations are overridden with the same syntax for implementing methods with no default implementation. It is not possible to call the default implementation from an overriding implementation of that method.

```rust
// Parameters of trait types (first takes ownership, second borrows)
fn my_function(arg: impl MyTrait) -> i32 {}
fn my_function(arg: &impl MyTrait) -> i32 {}

// This is syntactic sugar for
fn my_function<T: MyTrait>(arg: &T) -> i32 {}

// Long form has its uses, e.g. here both args must be same concrete type
fn my_function<T: MyTrait>(arg1: &T, arg2: &T) -> i32 {}

// To require two separate traits
fn my_function(arg: &(impl MyTrait + YourTrait)) -> i32 {}
fn my_function<T: MyTrait + YourTrait>(arg: &T) -> i32 {}

// Alternative syntax, useful with lots of bounds
fn my_function<T>(t: &T) -> i32
    where T: MyTrait + YourTrait
{}

// Return type can be a trait, but function implementation can only return
// a single contrete type
fn my_function() -> impl MyTrait {
    MyStruct {}
} 
```

```rust
// Methods for generic types can be implemented based on traits of type parameters
impl<T: MyTrait> MyCollection<T> {
    fn only_when_t_has_my_trait() {}
}

// Similary for traits
// MyTrait can be implemented for any type that implements YourTrait
impl<T: MyTrait> YourTrait for T {
    ...
}
```

# Ownership
- Each value in Rust has a variable that’s called its _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

The values of types which do not implement the copy trait are moved on assignment (including argument and return value passing), this means that in `a = b;` the variable `b` is no longer valid. A copy can be created with the clone method `a = b.clone()`.

The `Copy` trait cannot be implemented when special functionality is required when the value goes out of scope (marked by the `Drop` trait). Generally `Copy` is implemented by types that fit entirely on the stack: integers, booleans, floating points, characters, tuples containing only `Copy` types.

## References

A type prefixed with `&` is a reference type, a reference to a value also uses `&`. Use of a reference needs no annotation.
```rust
let v = String::from("Hello world!");
let r: &String = &v;
let l = r.len();
```

References do not take ownership. The value is not dropped when the reference goes out of scope.

By default references are immutable, that is they do not allow the value being referenced to be changed. References follow read many, write one rules: several immutable references OR one mutable reference may be in scope. The scope of a reference starts when it is introduced and extends to when it is last used (which may be earlier than the closing brace).
```rust
let v = String::from("Hello world");
let r1: &String = &v;
r1.push("!"); // Error
let r2: &mut String = &mut v;
r2.push("!"); // OK
```

A reference may not exist after the referenced value goes out of scope.

## Slices

A slice is a reference with a length. The borrowing rules of references apply.
```rust
let s = String::from("Hello world!");
let h: &str = &s[..5];
let w = &s[6..11];
let a = [1, 2, 3, 4, 5];
let b: &[i32] = &a[1..3];
```

# Lifetimes

The lifetime of parameter `s` must be as long as the lifetime of the returned reference.
```rust
fn my_function<'a>(s: &'a String) -> &'a str {}
```

The lifetime of the returned reference is unrelated to the lifetime of the parameter.
```rust
fn my_function<'a, 'b>(s: &'a String) -> &'b str {}
```

Static lifetime continues to the end of the program
```rust
fn bool_to_string(b: bool) -> &'static str {
    if b {"true"} else {"false"}
}
```

The lifetimes of any values assigned to references within a struct must be at least as long as the lifetime of the struct. I think the stuct definition needs the annotations so that they can be referred to in method and function prototypes.
```rust
struct MyStruct<'a, 'b> { 
    a_ref: &'a str,
    b_ref: &'b str,
}

impl<'x, 'y, 'z> MyStruct<'x, 'y> { 
    fn new(s: &'z str) -> MyStruct<'x, 'y> { 
        println!("{}", s);
        MyStruct{
            a_ref: "hello",
            b_ref: "other",
        } 
    } 

    fn log(&self, t: &str) { 
        println!("{}: {} {}", t, self.a_ref, self.b_ref);
    } 

    fn update(&mut self, t: &'x str) { 
        self.a_ref = t;
    } 
}

fn main() { 
    let mut m: MyStruct;
    let t = String::from("Goodbye");

    { 
        let s = String::from("Hello world!");
        m = MyStruct::new(&s);
        m.log("first");
        m.update("first");
        m.update(&t);
    } 

    println!("{}", m.a_ref);
}

```

Lifetime parameters go before generic type parameters
```rust
fn my_func<'a, T>(s: &'a T) -> &'a T { 
    s 
}

fn main() { 
    println!("{}", my_func(&"hello"));
}
```

## Lifetime elision

Compiler can fill in lifetime annotations according to these rules:
1. Each parameter that is a reference gets its own lifetime parameter.
2. If there is exactly one input lifetime parameter, this is assigned to the output lifetime parameters.
3. If there are multiple input lifetime parameters, but one is `&self` or `&mut self` (i.e. this is a method) the self lifetime is assigned to all output lifetime parameters.

If these rules do not apply, or do not match the actual lifetimes, the programmer must annotate the function.
