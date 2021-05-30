# Packages
Packages are used to build, test and share crates. Contains any number binary crates and up to one library crate (must contain at least one crate).

```sh
nick@NSB2 rusttest % cargo new --bin my-bin
     Created binary (application) `my-bin` package
nick@NSB2 rusttest % cargo new --lib my-lib 
     Created library `my-lib` package
nick@NSB2 rusttest % find . \( -name .git -prune \) -o -type f -print
./my-bin/Cargo.toml
./my-bin/.gitignored
./my-bin/src/main.rs
./my-lib/Cargo.toml
./my-lib/.gitignored
./my-lib/src/lib.rs
```

If `src/main.rs` exists the package contains a binary crate with the same name as the package. If `src/lib.rs` exists the package contains a library create with the same name as the package. Both can exist. Other binary crates can be created by placing one file for each in `src/bin`.

## Using external packages

Add package to dependency list in Cargo.toml

```toml
[dependencies]
rand = "0.5.5"
```

Import contents into source code.

```rust
use rand::Rng;
```

Standard library `std` does not need to be specified in dependencies, but its contents need bringing into scope (or accessed via full path).

Short cut for bringing different parts of a package into scope.

```rust
// Long hand
use std::cmp::Ordering;
use std::io;

// Short hand
use std::{cmp::Ordering, io};

// SECOND EXAMPLE
// Long hand
use std::io;
use std::io::Write;

// Short hand
use std::io::{self, Write};

// GLOBING
use std::collections::*;

// RENAMING
use std::io::Result as IoResult;

// REXPORTING
pub use std::io::Result;
```
# Crates
Tree of modules to produce a library or executable.

# Modules 
Controls organization, scope and privacy of paths.

## Module for library crate
src/lib.rs

```rust
mod front_of_house {
    mod hosting {
		// Public function (used below)
        pub fn add_to_waitlist() {
		  // Private, but can see parent's private parts
		  super::count_diners() {}
		}

		// Currently private
        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
	
	fn count_diners() {}
}

mod back_of_house {
    // Structure and toast member are public, seasonal_fruit is private
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
	    // Only way of creating Breakfast since season_fruit is private
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
	
	// All variants are public!
    pub enum Appetizer {
        Soup,
        Salad,
    }

}
 
use crate::front_of_house::serving;
use crate::back_of_house::Breakfast;

pub fn eat_at_restaurant() {
	// Absolute path
	crate::front_of_house::hosting::add_to_waitlist();

	// Relative path
	front_of_house::hosting::add_to_waitlist();
	
	// Use of use allows direct use of serving
	// Rust idiom says only modules, not function should be bought into scope
	serving::take_order()
	
	// On the other hand types ARE bought into scope! (unless there is a clash)
	let breakfast = Breakfast::summer("brown");
}

```

### Multiple files
Putting modules into separate files does not affect they way they are consumed.

src/lib.rs

```rust
mod front_of_house;
```

src/front_of_house.rs

```rust
mod hosting;
```

src/front_of_house/hosting.rs

```rust
pub fn add_to_waitlist() {}
```

# Paths
Ways of naming items such as structs, functions or modules.

Bought into scope with `use`, made public with `pub`.