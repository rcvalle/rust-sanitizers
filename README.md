sanitizers
==========

![Build Status](https://github.com/rcvalle/rust-crate-sanitizers/workflows/build/badge.svg)

Interfaces and FFI bindings for the
[sanitizers](https://github.com/google/sanitizers) interfaces.


Installation
------------

To install the `sanitizers` crate:

1. On a command prompt or terminal with your package root's directory as the
   current working directory, run the following command:

       cargo add sanitizers

Or:

1. Add the `sanitizers` crate to your package root's `Cargo.toml` file and
   replace `X.Y.Z` by the version you want to install:

       [dependencies]
       sanitizers = "X.Y.Z"

2. On a command prompt or terminal with your package root's directory as the
   current working directory, run the following command:

       cargo fetch


Usage
-----

To use the `sanitizers` crate:

1. Import the sanitizer module or funtions from the `sanitizers` crate. E.g.:

       use sanitizers::asan::*;

2. Use the provided interface for the sanitizer. E.g.:

       ...
       let mut data = vec![0u8; 100];
       let data_ptr = data.as_mut_ptr() as *const c_void;

       // Poison the memory region
       unsafe {
              __asan_poison_memory_region(data_ptr, data.len());
       }

       // Check if the memory region is poisoned
       let is_poisoned = unsafe { __asan_address_is_poisoned(data_ptr) };
       assert_eq!(is_poisoned, 1);
       ...

3. Build your package with the sanitizer enabled. It is recommended to rebuild
   the standard library with the sanitizer enabled by using the Cargo build-std
   feature (i.e., `-Zbuild-std`) when enabling the sanitizer. E.g.:

       RUSTFLAGS="-Clinker=clang -Clink-arg=-fuse-ld=lld -Zsanitizer=address" \
         cargo build -Zbuild-std -Zbuild-std-features \
         --target x86_64-unknown-linux-gnu


Contributing
------------

See [CONTRIBUTING.md](CONTRIBUTING.md).


License
-------

Licensed under the Apache License, Version 2.0 or the MIT License. See
[LICENSE-APACHE](LICENSE-APACHE) or [LICENSE-MIT](LICENSE-MIT) for license text
and copyright information.
