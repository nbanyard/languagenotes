# Expressions and Statements

## Control flow expressions and statements

### If expressions

```rust
if a > 4 {
    println!("Big");
} else if a > 2 {
    println!("Medium");
} else {
    println!("Small");
}

let number = if condition { 5 } else { 6 };
```

### Loops

#### Infinite
```rust
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};
```

#### While
```rust
let mut counter = 0;
let result = while counter < 10 {
    counter += 1;
};
assert_eq!(result, ());
```

#### For
```rust
let a = [10, 20, 30, 40, 50];
for element in a.iter() {
	println!("the value is: {}", element);
}

for number in (1..4).rev() {
	println!("{}!", number);
}
println!("LIFTOFF!!!");
```

## Summary of expressions and statements

| Construct | Type |
|---|---|
| Function definition | Statement |
| Function call | Expression |
| `let` | Statement |
| `{}` block | Expression (value of last expression) |
| Expression followed by `;` | Statement |