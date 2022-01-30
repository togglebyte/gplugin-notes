# Building GPlugin

Install the following dependencies (Arch linux, btw)

```
$ pacman -S mercurial meson ninja base-devel gobject-introspection help2man
```

Clone the GPlugin project. This does not have to live inside your Rust project:

```
hg clone https://keep.imfreedom.org/gplugin/gplugin
```

Build:

```
$ meson -Dlua=false -Dpython3=false -Dperl5=false -Dvapi=false -Dgtk3=false -Dgtk4=disabled -Ddoc=false build
$ cd build
$ ninja
```

Verify the build: 

```
$ meson devenv gplugin-query
```

From here on there are two options:
* Run `meson devenv` and "GPlugin" will be available
* Install GPlugin by running `ninja install` TODO: did not work for me, can't find the gplugin.pc file?
