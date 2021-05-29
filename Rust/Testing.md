# Testing

## Unit tests

When `cargo new` is run with option `--lib` the initial file `src/lib.rs` has a single sample test. Tests are run with `cargo test`.

```rust
#[cfg(test)]
mod tests {
    // Add `use super::*` to access containing module
	
    #[test]
    fn a_test_function() {
	    a_helper_function();
        assert_eq!(2 + 2, 4);
    }
	
	fn a_helper_function() {
	}
}
```

### Assertions and results
```rust
assert!(condition_expected_to_be_true);

// Values in following must implement PartialEq and Debug traits
// For structs `#[derive(PartialEq, Debug)]` will provide default implementations
assert_eq!(test_value, expected_value);
assert_ne!(test_value, unexpected_value);

// A format string and parameters can be added to replace error message
assert!(
	result.contains("Carol"),
	"Greeting did not contain name, value was `{}`",
	result
);

// Assertions just panic
panic!("Error message");

// Returning a Result lets `?` operator be used during the test
#[test]
fn should_return_ok() -> Result<(), String> {
	some_action_returning_result()?;
	Ok(())
}
```

### Annotations

annotation | use
---|---
`[test]` | Identify function as a test case.
`[should_panic]` | Expect test case to panic.
`[should_panic(expected = "msg substr")]` | Expect test case to panic with a message containing `msg substr`.
`[ignore]` | Do not run this test, include in ignored test count.

### Running tests
Help from test compiler: `cargo test --help`.
Help from test program: `cargo test -- --help`.
Ordinarily all tests are run in parallel, to run them in series: `cargo test -- --test-threads=1`.
Ordinarily only output from tests that fail is rendered, to render all output: `cargo test -- --show-output`.
To only run tests whose names contain `somestring`: `cargo test somestring`.
To run ignored tests: `cargo test -- --ignored`.
To only run unit tests: `cargo test --lib`. To include unit tests if other filters apply add `--lib`.

### Organising tests
1. Add a `test` module to each source file to test its contents. The `cfg(test)` attribute ensures the module is only compiled when the `test` configuration option 	is provided.

   ```rust
   #[cfg(test)]
   mod tests {
       ...
   }
   ```
   
2. Private functions from the file module can be tested because modules can see their ancestors' private parts.

## Integration tests

`tests/integration_test.rs`:
```rust
use adder;

#[test]
fn it_adds_two() {
    assert_eq!(4, adder::add_two(2));
}
```

Each file in the `tests` directory is a separate crate, code under test needs to be bought into scope. Each file is compiled to a separate test program which `cargo test` will run individually.

Only files in the `tests` directory get compiled as test crates. This allows sub-directories to be used for code common across test crates. Use `mod subdir;` in each test crate.

The code in [[Packages, Crates and Modules#Crates|binary crates]] cannot be tested. This code should be kept to a minimum and functionality put in the library crate.
### Running tests
To only run one integration test program: `cargo test --test integration_tests`. To run multiple programs repeat `--test`.