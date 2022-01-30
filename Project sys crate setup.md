Change directory to the `gplugin-sys` dir.

Create a `Gir.toml` file.
This file doesn't have to be called `Gir.toml`, if it's named something else,
`gir` has to be called with `-c <your file>`.


```toml
[options]
girs_directories = [
    "path/to/gplugin/build/gplugin",
    "../gir-files"
]
work_mode = "sys"
library = "GPlugin"
version = "1.0"
min_cfg_version = "1"
external_libraries = [
   "GLib",
   "GObject",
]
```

To generate the sys crate run `gir -o <projectname-sys>` (remember to add `-c
<filename>` if you didn't name the Gir file `Gir.toml`).
This will generate a Rust project for you.

The generated project will have the dependencies a bit wrong and the version
of GPlugin is set to one but at the time of writing this is 0.37.

Open the `Cargo.toml` file and do the following amendments:

Change 

```toml
[dependencies.glib-sys]
git = "https://github.com/gtk-rs/gtk-rs-core"

[dependencies.gobject-sys]
git = "https://github.com/gtk-rs/gtk-rs-core"
```
to
```toml
[dependencies.glib]
git = "https://github.com/gtk-rs/gtk-rs-core"
package = "glib-sys"

[dependencies.gobject]
git = "https://github.com/gtk-rs/gtk-rs-core"
package = "gobject-sys"
```

Change

```toml
name = "gplugin"
version = "1"
```
to
```toml
name = "gplugin"
version = "0.3"
```

Now you can move all the files in your newly crated Rust project to its parent
directory.

This should leave you with the following structure:

```
├── gir
├── gir-files
├── gplugin
└── gplugin-sys/
    ├── build.rs
    ├── Cargo.lock
    ├── Cargo.toml
    ├── Gir.toml
    ├── src
    ├── target
    └── tests
```

At this point, given that GPlugin is made available via either `meson devenv` or
`ninja install`, the project should build with `cargo build`.
