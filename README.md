This crate exports a macro `enum_from_primitive!` that wraps an
`enum` declaration and automatically adds an implementation of
`num::FromPrimitive` (reexported here), to allow conversion from
primitive integers to the enum.  It therefore provides an
alternative to the built-in `#[derive(FromPrimitive)]`, which
requires the unstable `std::num::FromPrimitive` and is disabled in
Rust 1.0.

The current implementation requires all variants of the enum to
have an explicit discriminator value.  This restriction may be
relaxed in future versions.

## Usage

Add the following to your `Cargo.toml` file:

```
[dependencies]
enum_primitive = "*"
```

Import the crate using `#[macro_use] extern crate enum_primitive`, and
wrap your `enum` declaration inside the `enum_from_primitive!` macro.

## Example

```rust
#[macro_use] extern crate enum_primitive;
extern crate num;
use num::FromPrimitive;

enum_from_primitive! {
#[derive(Debug, PartialEq)]
enum FooBar {
    Foo = 17,
    Bar = 42,
}
}

fn main() {
    assert_eq!(FooBar::from_i32(17), Some(FooBar::Foo));
    assert_eq!(FooBar::from_i32(42), Some(FooBar::Bar));
    assert_eq!(FooBar::from_i32(91), None);
}
```
