Here are my notes for creating Rust bindings for GPlugin.

Start by creating a project directory: `mkdir gplugin`

Then clone the following repos into the newly made directory:

* `https://github.com/gtk-rs/gir/` TODO: confirm that we actually need this
* `https://github.com/gtk-rs/gir-files`

Run `cargo new --lib gplugin` for the binary crate,
and finally `mkdir gplugin-sys` for the sys crate.

The project directory should now contain the following:

```
.
├── gir         # gtk-rs/gir
├── gir-files   # gtk-rs/gir-files
├── gplugin     # binary crate
└── gplugin-sys # sys crate
```


* [Building GPlugin](building-gplugin.md)
* [Project sys crate setup](sys-crate.md)
* [Binary crate](binary-crate.md)
