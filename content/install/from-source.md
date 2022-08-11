+++
title = "Building Acton from source"
+++
# Building Acton from source

If you want to mess around with Acton itself, like hack on the compiler or add to the standard library of modules you will need to build the Acton system from source.

# Get the code
```
git clone git@github.com:actonlang/acton.git
```

# Build dependencies
Install the build time dependencies. This also includes the dependencies for
using `actonc` to compile Acton programs.

## Debian / Ubuntu
```
apt install gcc haskell-stack libbsd-dev libmd-dev libprotobuf-c-dev libutf8proc-dev libuv1-dev make pkg-config procps uuid-dev zlib1g-dev
```

On older distributions (like Ubuntu 20.04), stack is too old to properly function and needs to first be upgraded.

```sh
stack upgrade
export PATH=~/.local/bin:$PATH
```

## Mac OS X on x86_64

```sh
brew install argp-standalone haskell-stack libuv pkg-config protobuf-c utf8proc
```

## Mac OS X on M1
```sh
brew install argp-standalone haskell-stack libuv pkg-config protobuf-c utf8proc
```
Ensure you add LLVM to your path:
```sh
export PATH="/opt/homebrew/opt/llvm/bin:$PATH"
```

# Building the Acton system
Run `make` in the project root:
```
make -j
```

`-j` is for parallel make using as many CPU cores as available on your machine.

# Running tests
You can run the test suite through:
```
make test
```

