# glutin -  OpenGL, UTilities and INput
[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/tomaka/glutin?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![](http://meritbadge.herokuapp.com/glutin)](https://crates.io/crates/glutin)

[![Docs.rs](https://docs.rs/glutin/badge.svg)](https://docs.rs/glutin)

Alternative to GLFW in pure Rust.

[![Build Status](https://travis-ci.org/tomaka/glutin.png?branch=master)](https://travis-ci.org/tomaka/glutin)
[![Build status](https://ci.appveyor.com/api/projects/status/cv5xewg3uchb3854/branch/master?svg=true)](https://ci.appveyor.com/project/tomaka/glutin/branch/master)

```toml
[dependencies]
glutin = "*"
```

## [Documentation](https://docs.rs/glutin)

## Try it!

```bash
git clone https://github.com/tomaka/glutin
cd glutin
cargo run --example window
```

## Usage

Glutin is an OpenGL context creation library and doesn't directly provide OpenGL bindings for you.

```toml
[dependencies]
gl = "*"
libc = "*"
```

```rust
extern crate gl;
extern crate glutin;
extern crate libc;

fn main() {
    let window = glutin::Window::new().unwrap();

    unsafe { window.make_current() };

    let gl_ = match gl::GlType::default() {
        gl::GlType::Gl => gl::GlFns::load_with(|symbol| window.get_proc_address(symbol) as *const _),
        gl::GlType::Gles => gl::GlesFns::load_with(|symbol| window.get_proc_address(symbol) as *const _),
    };


    gl_.clear_color(0.0, 1.0, 0.0, 1.0);

    for event in window.wait_events() {
        gl_.clear(gl::COLOR_BUFFER_BIT);
        window.swap_buffers();

        match event {
            glutin::Event::Closed => break,
            _ => ()
        }
    }
}
```

Note that glutin aims at being a low-level brick in your rendering infrastructure. You are encouraged to write another layer of abstraction between glutin and your application.

## Platform-specific notes

### Android

 - To compile the examples for android, initialize the submodules, go to `deps/apk-builder/apk-builder` and run `cargo build`, then go back to `glutin` and call `ANDROID_HOME=/path/to/sdk NDK_HOME=/path/to/ndk NDK_STANDALONE=/path/to/standalone cargo test --no-run --target=arm-linux-androideabi`
