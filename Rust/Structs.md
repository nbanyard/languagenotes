# Structs

## Definitions

```rust
struct User {
    username: String,
	email: String,
	sign_in_count: u64,
	active: bool,
}
```

```rust
struct Color(i32, i32, i32);
```

```rust
struct MyUnit(); // Spoiler alert: used for traits
```

Provide an automatically generated debug format.
```rust
#[derive(Debug)]
struct Rectangle {
    height: u32,
	width: u32,
}

println!("My rectangle: {:?}", rectangle);
```

## Instance

```rust
let username = String::from("someone");
let mut user1 = User {
    email: String::from("someone@example.com"),
	username, // Field and variable have same name
	active: true,
};
user1.sign_in_count = 3; // user1 has to be mutable
let user2 = User {
    email: String::from("another@example.com"),
	username: String::from("another"),
	..user1 // only assigns fields not set above
}
```

```rust
let black = Color(0, 0, 0);
```

## Methods

```rust
struct Rectangle {
    width:u32,
	height: u32,
}

// Can have more than one impl block if required
impl User {
    fn area(&self) -> u32 {
	    self.width * self.height
	}
	
	// Method with extra arguments
	fn can_hold(&self, other: &Rectangle) -> bool {
	    self.width > other.width && self.height > other.height
	}
	
	// Associated function, has no self parameter
	fn square(size: u32) -> Rectangle {
	    Rectangle {
		    width: size,
			height: size,
		}
	}
}

// Method and associated function usage
let rect = Rectangle {
    width: 30,
	height: 50,
};
let square = Rectangle::square(25);
println!("The square fits in the rectangle: {}", rect.can_hold(square));
```