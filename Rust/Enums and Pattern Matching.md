# Enums and Pattern Matching

```rust
// Each variabe has different embedded types
enum Message{
    Quit,
	Move { x: i32, y: i32 },
	Write(String),
	ChangeColor(i32, i32, i32),
}

impl Message {
    fn call(&self) {
	}
}

let m = Message::Write(String::from("hello"));
m.call();

fn main() {
    let msg = Message::ChangeColor(0, 160, 255);

    match msg {
        Message::Quit => {
            println!("The Quit variant has no data to destructure.")
        }
        Message::Move { x, y } => {
            println!(
                "Move in the x direction {} and in the y direction {}",
                x, y
            );
        }
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => println!(
            "Change the color to red {}, green {}, and blue {}",
            r, g, b
        ),
    }
}

```

Enums work with generic types, the following is in the prelude so does not need to be bought into scope.

```rust
enum Option<T> {
    Some(T),
	None,
}

fn plus_one(Option<i32>) -> Option<i32> {
    match x {
	    None => None,
        Some(i) => Some(i + 1),
    }
}


match x {
    Some(i) => {
	   println!("We got something, it was {}", i);
	},
	_ => (), // Says that the match is not exhaustive
}

// The following is useful if only one variant is to be matche
// Can add else, but the compiler does not ensure all variants are handled
if let Some(i) = x {
    println!("We got something, it was {}", i);
}
```
