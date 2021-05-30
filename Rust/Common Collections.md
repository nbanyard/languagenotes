# Vector

Construction

```rust
// Empty vector
let v: Vec<i32> = Vec::new();

// Initialised vector - type inferred
let v1 = vec![1, 2, 3];
```

Modification

```rust
v.push(5);
```

Access

```rust
// Program will panic if length is less than three
let third: &i31 = &v[2];

// Safer access used get() which returns an Option<&T>
match v.get(2) {
	Some(third) => println!("The third elements is {}", third),
	None => println!("There is no third element")
}

// Read via iteration
for i in &v {
	println!("{}", i);
}

// Modify via iteraion, v must be mutable
for i in &mut v {
	*i += 50;*
}
```

When a member is accessed in this way the entire array is borrowed immutably. So it is not possible to update the array while references exist.

A vector and its contents are freed when the vector goes out of scope.



# Hash Map

```rust
fn main() {
    use std::collections::HashMap;

	// Looks like magic, but scores will be HashMap<String, i32>
    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
}
```

`Vec` provides `collect` to gather data into different types of collections. Here we specify it will be a HashMap, but leave the compiler to infer that it is a `HashMap<String, i32>`.

```rust
fn main() {
    use std::collections::HashMap;

    let teams = vec![String::from("Blue"), String::from("Yellow")];
    let initial_scores = vec![10, 50];

    let mut scores: HashMap<_, _> =
        teams.into_iter().zip(initial_scores.into_iter()).collect();
}
```

`insert` copies values of types implementing the `Copy` trait (e.g. integers) but causes the HashMap to take ownership of other values.

```rust
// score is Option<&V>
let team_name = String::from("Blue");
let score = scores.get(&team_name);

// Iteration
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
for (k, v) in &scores {
	let key: &String = k;
	let value: &i32 = v;
	println!("{}: {}", key, value);
}
```

`insert()` replaces any existing value. `entry()` returns a value of enum `Entry` representing a value that may or may not exist. `Entry::or_insert()` inserts the value only if `Entry` does not exist. In either case a mutable refrence to the (possibly new) value is returned.

```rust
// Set in any case
scores.insert(team_name_1, 10);
// Set if does not exist
scores.entry(team_name_2).or_insert(50);
// Increment current value (treating missing value as 0)
// score is `&mut i32` and must go out of scope to release borrow
let score = scores.entry(team_name_3).or_insert(0);
*score += 1;
```

The hashing function is cryptographically strong, but can be switched for a different a function, crates exist with alternatives.