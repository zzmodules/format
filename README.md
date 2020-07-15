format
======

> String format macros for ZZ

## Installation

Add this to your `zz.toml` file:

```toml
[dependencies]
format = "*"

[repos]
format = "git://github.com/zzmodules/format"
```

## Usage

```c++
using format
```

## API

### `@format::with(fmt, ...)`

Creates a formatted string from a variety of special tokens with traditional
`printf` style specifiers supported.

### Tokens

* `{}` - Treats argument like a _struct_, calling `.display()` for a `char *`
* `{*}` - Treats argument like a _pointer to a struct_, calling `->display()`  for a `char *`
* `{0..9}` - References argument by position. Treatment is the same as `{}`
* `{*0..9}` - References pointer argument by position. Treatment is same as `{*}`
* `{$0..9}` - References argument _identifier name_ by position and uses it inline
* `{0..9%X}` - References argument by position with format. Treatment is like a limited `printf`

### `Display` Interface

The `@format::with()` macro will treat struct and struct pointers specified
with `{}`, `{*}`, and its variant as special input that should implement a
**displayable** interface. Structs should simply implement a
`display() -> char *` method like in the example below:

```c++
using format
using log
using string

struct Vector {
  float x;
  float y;
}

fn display(Vector *self) -> char * {
  new+64000 out = string::make();
  out.format("Vector { x: %0.2f, y: %0.2f }", self->x, self->y);
  return out.cstr();
}

fn main() -> {
  let vec = Vector { x: 1.23, y: 2.46 };
  log::info("%s", @format::with("{}", vec)); // Vector { x: 1.23, y: 2.46 }
  return 0;
}
```

## License

MIT
