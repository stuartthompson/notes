# Div Euclid & Rem Euclid

These functions are used to calculate the quotient and remainder of two
numbers.

This is useful for understanding how many whole times a number can be divided
by another number and what the remainder will be after the division.

## Example

A table column is 20 characters wide. Some content to be rendered into the
column is 45 characters wide. If the text is wrapped, it is useful to know
how many whole lines of text will be needed and how many characters will
remain to be rendered on the final line.

The following code uses div\_euclid and rem\_euclid to calculate the number of
whole lines and the number of remaining characters respectively.

```rust
let width = 20;
let content = "This long line of text contains 45 characters";

let whole_lines = content.len().div_euclid(width);
let remaining_chars = content.len().rem_euclid(width);

println!("Whole lines: {}", whole_lines);
println!("Remaining chars: {}", remaining_chars);
```

```text
Whole lines: 2
Remaining chars: 5
```

### Rust Playground

Permalink:
[Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=c658d74013a221bffa92fad32294bf75)
