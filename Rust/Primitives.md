# Primitives

## Numeric

| Length | Signed | Unsigned | Floating point |
|---|---|---|---|
| 8-bit | `i8` | `u8` | - |
| 16-bit | `i16` | `u16` | - |
| 32-bit | `i32`* | `u32` | `f32` |
| 64-bit | `i64` | `u64` | `f64`* |
| 128-bit | `i128` | `u128` | - |
| arch | `isize` | `usize` | - |

\* Default type

### Handling overflow
The primitive integer types have methods such as `wrapping_add()`, `checked_add()`, `overflowing_add()` which handle integer overflow in different ways. The default way panics in a debug build and wraps in a release build.

## Boolean

```rust
let f: bool = false;
```

## Character

The `char` type is four byte Unicode Scalar Value from `U+0000` to `U+D7FF` and `U+E000` to `U10FFFF`.

## Tuple

Compound type with fixed size and member types.

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;
let x1 = tup.0;
```

## Array

Compound type with fixed size and single fixed type.

```rust
let a = [1, 2, 3, 4]; // Implicit type
let b: [i32; 5] = [1, 2, 3, 4]; // Explicit type
let c = [3; 5]; // Short for [3, 3, 3, 3, 3]
let z = a[3];
```

Invalid array element access results in a run time panic.
