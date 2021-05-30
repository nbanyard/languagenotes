# Error Handling

## Recoverable errors
Rather than exceptions the type `Result<T, E>` is used where a recoverable error may have occurred.

```rust
enum Result<T, E> {
    Ok(T),
	Err(E),
}

use std::result::Result;
use std::fs::File;
use std::io::Error;
// Type is normally inferred
let f: Result<File, Error> = File::Open("hello.txt");
// Note shadowing here
let f = match f {
    Ok(file) => file,
	Err(error) => panic!("Problem: {:?}", error),
};

// unwrap() panics
let f: File = File::Open("hello.txt").unwrap();
// So does expect(), but a string is added to the error message
let f: File = File::Open("hello.txt").expect("My error message");
```

### The ? operator

```rust
// Long hand way of propogating errors
fn my_function() -> Result<MyReturnType, some::Error> {
    let r = first_function();
	
	let r = match r {
		Ok(result) => result,
		Err(e) = return Err(e),
	}
	// Other stuff
	OK(final_r)
}

// Short hand
fn my_function() -> Result<MyReturnType, some::Error> {
    let r = first_function()?;
	
	// Other stuff
	OK(final_r)
}
```

The `?` is placed after a `Result` value. If the value matches `Ok`, the value is returned. If the value matches `Err` the error is converted to the error type of the enclosing function using the `from` function of the `From` trait.

## Unrecoverable errors
Generally programming errors. Program can be terminated with `panic!` macro.

By default panic unwinds the stack and cleans up data from each function. Programs can be built to skip this with the following entry in `Cargo.toml`. This produced some reduction in binary size.

```toml
[profile.release]
panic = 'abort'
```

The error message says where the panic occurred and gives instructions for using `RUST_BACKTRACE` to get a stack trace.