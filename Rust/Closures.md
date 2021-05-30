# Closures

```rust
let closure = |param1: u32, param2: f64| -> String {
    // Implementation
}
```

Type annotations are optional. Without them the compiler will infer concrete types using the calling code. If the closure is called multiple times the compilation will fail if no consistent types can be found.

```rust
let closure = |param1, param2| {
    // Implementation
}
```

Closures with a single expression can omit the braces.

```rust
let add = |a, b| a + b;
```

## Storing closures
This example shows the `Fn` trait being used to specify the type of a closure. This is required since each closure has a different type, even if the parameters and return types are the same.

```rust
struct Cacher<T>
where
    T: Fn(u32) -> u32,
{
    calculation: T,
    value: Option<u32>,
}

impl<T> Cacher<T>
where
    T: Fn(u32) -> u32,
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        match self.value {
            Some(v) => v,
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            }
        }
    }
}

fn main() {}
```

## Traits for closures and functions

- `FnOnce` - Indicates a callable that may not be called more than once. Typically because ownership of captured variables is passed on.