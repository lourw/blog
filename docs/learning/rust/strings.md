There are two ways to store strings in rust:

## String literal

```rust
let x: &str = "Hello World";
```

- stored in binary
- stored in the stack
- fixed in size

## String Type

```rust
let x:String = String::from("Hello World");
```

- dynamic in type
- stored on the heap

### Clone
To make a deep copy of a `String`, you need to use the `.clone()` method:

```rust
let x: String = String::from("Hello World");
let y: String = x.clone();
```
