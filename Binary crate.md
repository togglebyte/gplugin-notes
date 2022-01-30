Change directory to the binary crate created at the beginning of all of this.

Just like the sys crate, crate a `Gir.toml` file.

```toml
[options]
girs_directories = [
    "/path/to/gplugin/build/gplugin",
    "../gir-files"
]
library = "GPlugin"
version = "1.0"
min_cfg_version = "1"
target_path = "."
work_mode = "normal"
generate_safety_asserts = true
deprecate_by_min_version = true
# with this option enabled, versions for gir and gir-files saved only to one file to minimize noise
single_version_file = true


external_libraries = [
    "GLib",
    "GObject",
]

generate = [
    "GPlugin.CoreFlags",
    "GPlugin.Loader",
    "GPlugin.Manager",
    "GPlugin.Plugin",
    "GPlugin.PluginState",
    "GPlugin.get_flags",
    "GPlugin.get_option_group",
    "GPlugin.init",
]

[[object]]
name = "GPlugin.PluginInfo"
status = "generate"
generate_builder = true
```

Next up add some dependencies to `Cargo.toml`:

```toml
[dependencies]
libc = "0.2"
glib = {git = "https://github.com/gtk-rs/gtk-rs-core"}
bitflags = "1.3.2"
gtk = "0.15.2"

[dependencies.ffi]
package = "gplugin-sys"
version = "*"
path = "../gplugin-sys"
```

There are some macros referred to in the auto generated code that doesn't exist
that should be added to `src/lib.rs`:

```rust
/// Asserts that this is the main thread and `gtk::init` has been called.
macro_rules! assert_initialized_main_thread {
    () => {
        if !::gtk::is_initialized_main_thread() {
            if ::gtk::is_initialized() {
                panic!("GTK may only be used from the main thread.");
            } else {
                panic!("GTK has not been initialized. Call `gtk::init` first.");
            }
        }
    };
}

/// No-op.
macro_rules! skip_assert_initialized {
    () => {};
}
```

Then finally import the auto generated code in `src/lib.rs`

```rust
mod auto;
pub use auto::*;
```
