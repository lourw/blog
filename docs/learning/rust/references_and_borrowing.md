---
title: Rust References and Borrowing
description: An brief summary of what this project was about
---

With C you are able to set pointers and reference them at will. The caveat being that C will not guarantee if your pointer will be referencing a valid memory address. Given the user has so much control over memory addresses, it also allows for bugs to creep into code. Points don't keep track of the validity of underlying values and it's totally possible to accidentally set a pointer pointing to null. 

Rust circumvents the pointer issue by using its own system of memory management. A reference in rust is similar to C in that it acts like a pointer to data behind a memory address. However, it is guaranteed that the reference will point to valid data. 

## Reference Syntax
Similar to C, rust also uses the same symbols for reference/pointer manipulation. 

=== "Rust"

    ```
    fn main() {
        let mut x: u32 = 10; # initialize a variable with value 10

        let y: 
    }
    ```

=== "C"

    ```
    int main(void) {
        int x = 10; # initialize a variable with value 10

        int* y = &x # initialize a pointer that references the address of a value

        *x = 20; # dereference pointer and set new value at pointer location
    }
    ```

## References
* [Rust docs](https://doc.rust-lang.org/std/primitive.pointer.html)


