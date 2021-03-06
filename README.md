# wasm-demo-rs
A basic example of Rust compiled to WebAssembly, using Rust's own native tools.

## About
This is a simple demo for compiling Rust to WebAssembly.
This demo uses the nightly toolchain, the `cargo web` command, and the `stdweb` crate.
While this is a new feature, there is some setup that you will need to do so you can build and run this. I recommend doing it on Linux, since we'll be using a single Makefile I wrote to make my life easier, but you could also replicate its commands manually on your favorite OS, assuming it runs Rust, `cargo web` and `stdweb` fine.

I felt the need to create this repository for future reference, so it is only a simple demo with small considerations. I might add or remove things, if necessary.

## Dependencies
As stated above, this demo depends on the nightly toolchain (for now), and the `cargo-web` tool.
Assuming you have `rustup` installed, you can install the nightly toolchain with the following command:

```bash
$ rustup toolchain install nightly
```

After that, you can add Rust's native WASM target with this command:

```bash
$ rustup target add wasm32-unknown-unknown --toolchain nightly
```

And that ought to take care of things on the toolchain side. Now, you'll need to install `cargo-web`.

```bash
$ cargo +nightly install cargo-web
```

That should provide you with the latest version of `cargo-web`, using the nightly toolchain.

## Compiling
The following commands should work nicely with the `+nightly` flag if you're using the nightly toolchain. Once the WASM target gets to the `stable` toolchain, you might be able to omit that flag.

### Using provided Makefile
I've written a well-documented Makefile that should make stuff easier for compiling.
When you run `make`, a "site" folder will appear, containing an `index.html` file, along with the `wasm` binary and the `js` glue code. This will export the application, ready to be uploaded anywhere you'd like for a live demo.

If you just want to test it, you can run `make webstart`, and your browser will be fired up at `localhost:8000` with your demo app.

### Compiling by hand
You can compile directly using `cargo` as well, only instead of building, you'll have to use `cargo-web` to generate your files.
Fire up your terminal and use the following command on the root folder:

```bash
$ cargo +nightly web build --target-webasm --release
```
This should give you your .js and .wasm files on `target/wasm32-unknown-unknown/release`.
If you only want to test your demo app, you can use `cargo web start` to run it on `localhost:8000`. This will also fire up your browser:

```bash
$ cargo +nightly web start --target-webasm --release
```

Notice that changing your Rust files will also recompile your project, but you'll have to refresh the browser page manually so it takes effect.

#### Considerations about this method
Notice that the static `index.html` refers to `js/app.js` instead of `wasm-demo-rs.js`. This is done on purpose, since `cargo web start` asks you to refer to this file on your `html`. To fix this, you can use a small substitution, such as using `sed`, when copying the index.html file to your output folder (this is done on the Makefile, so refer to it for an example).

## Useful links
- [Setting up the native WASM target for Rust](https://www.hellorust.com/setup/wasm-target/)
- [Koute's awesome NES emulator, Pinky, running on the web](https://github.com/koute/pinky/tree/master/pinky-web), which inspired this small demo. Also has bindings for Emscripten and asmjs targets.
- [raphamorim's awesome WASM + Rust tutorial](https://github.com/raphamorim/wasm-and-rust), which covers the Emscripten side of things while also explaining some aspects of WASM.
- [My reimplementation of an old game, Super BrickBreak, in Rust/WASM](https://github.com/luksamuk/super-brickbreak-rs), which uses this very code as a template.
