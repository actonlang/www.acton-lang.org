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
Install the build time dependencies.

## Debian / Ubuntu
```
apt install git haskell-stack make pkg-config procps
```

On older distributions (like Ubuntu 20.04), stack is too old to properly function and needs to be upgraded first.

```sh
stack upgrade
export PATH=~/.local/bin:$PATH
```

## Mac OS X

```sh
brew install haskell-stack pkg-config
```

# Building the Acton system
Run `make` in the project root:
```
make -j
```

`-j` is for parallel make using as many CPU cores as available on your machine.

# Running tests
```
make test
```
