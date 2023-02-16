---
title: "Part 1: REPL"
description: Read-eval-print-loop that controls the database CLI
---

## Overview
The first step in implementing a database is to set up the user interface (read-eval-print-loop or REPL) that allows a user to interact with the underlying database once it's built. Using a simple while loop and some IO commands, this part sets up a simple while loop CLI that waits for user input and completes an action based on a given input. 

## Challenges
For this first part, the core logic being implemented was mostly a while loop and some IO features. The REPL just needed to complete the following actions: 

1. await user input when run
2. read and process user input
3. output some result or text back to user

There wasn't much complexity in this step other than figuring out rust's IO libraries. In C you have the `string.h` library that handles a lot of the string manipulation and `stdio.h` for managing IO. 

## Finished Code

=== "Rust"
    ``` rust
    use std::{io::{self, Write}, process};

    fn main() {
        println!("Hello, world!");
        loop {
            print_prompt();
            io::stdout().flush().unwrap();

            let mut input = String::new();

            io::stdin()
                .read_line(&mut input)
                .expect("Failed to read line.");

            if input.trim() == ".exit" {
                println!("Exiting from process...");
                process::exit(0x0100);
            } else {
                println!("{}", input);
            }
        }
    }

    fn print_prompt() {
        print!("db > ");
    }
    ```
=== "C"
    ``` C title="Taken from 'Build Your Own Database in C'"
    #include <stdbool.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>

    typedef struct {
        char* buffer;
        size_t buffer_length;
        ssize_t input_length;
    } InputBuffer;

    InputBuffer* new_input_buffer() {
        InputBuffer* input_buffer = malloc(sizeof(InputBuffer));
        input_buffer->buffer = NULL;
        input_buffer->buffer_length = 0;
        input_buffer->input_length = 0;

        return input_buffer;
    }

    void print_prompt() { printf("db > "); }

    void read_input(InputBuffer* input_buffer) {
        ssize_t bytes_read =
            getline(&(input_buffer->buffer), &(input_buffer->buffer_length), stdin);

        if (bytes_read <= 0) {
            printf("Error reading input\n");
            exit(EXIT_FAILURE);
        }

        // Ignore trailing newline
        input_buffer->input_length = bytes_read - 1;
        input_buffer->buffer[bytes_read - 1] = 0;
        }

    void close_input_buffer(InputBuffer* input_buffer) {
        free(input_buffer->buffer);
        free(input_buffer);
    }

    int main(int argc, char* argv[]) {
        InputBuffer* input_buffer = new_input_buffer();
        while (true) {
            print_prompt();
            read_input(input_buffer);

            if (strcmp(input_buffer->buffer, ".exit") == 0) {
                close_input_buffer(input_buffer);
                exit(EXIT_SUCCESS);
            } else {
                printf("Unrecognized command '%s'.\n", input_buffer->buffer);
            }
        }
    }
    ```

## Notable Differences

I'm pretty sure I ended up using some more high level libraries since a lot of the input and I/O management was handled by rust's `std::io` library. 
