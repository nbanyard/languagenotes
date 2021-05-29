### Literals, Constants, Variables

## Literals

## Constants

* Can only be set to constant expressions.
* Must be annotated with its type.
* Can be in any scope, including global.

```rust
const MAX_POINTS: u32 = 100_000;
```

## Variables

Define and initialize an immutable variable of an inferred type.
```rust
let x = 5;
```

Define and initialize a mutable variable of an inferred type.
```rust
let mut x = 5;
```

Define and initialize an immutable variable of a given type.
```rust
let x: u128 = 5;
```

### Shadowing

Immutable variables cannot be changed. But! A new variable can shadow an old one but using the `let` keyword again.
```rust
fn main() {
  let x = 5;
  let x = x + 1;
  let x = x + 2;
  println!("The value of x is '{}'", x);
}
```

Shadowing allows the type to be changed.
```rust
fn main() {
  let spaces = "   ";
  let spaces = spaces.len();
  println!("There were {} spaces", spaces);
}
```