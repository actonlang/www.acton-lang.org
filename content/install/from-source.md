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

## Debian
```
apt install gcc haskell-stack libbsd-dev libprotobuf-c-dev libutf8proc-dev make uuid-dev zlib1g-dev
```

## Mac OS on x86_64

```sh
brew install argp-standalone haskell-stack protobuf-c utf8proc util-linux
```

## Mac OS on M1
```sh
brew install --HEAD haskell-stack
brew install argp-standalone llvm protobuf-c utf8proc util-linux
```
Ensure you add LLVM to your path:
```sh
export PATH="/opt/homebrew/opt/llvm/bin:$PATH"
```

# Building the Acton system
Simply run `make` in the project root:
```
make
```

# Running tests
You can run the test suite through:
```
make -C test
```

