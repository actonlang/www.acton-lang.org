+++
title = "Install"
weight = 1
template = "page.html"
+++

# Install

Acton is available for Linux and Mac OS X.

The platforms listed here are for developing with Acton. The final binary executable produced from compiling an `.act` can run on other platforms, for example Ubuntu 18.04 and can be made to run on even older systems through static linking, see [Acton by Example: compilation](https://acton.guide/compilation.html)

# Debian / Ubuntu

## Supported versions
- Debian 11 / x86_64
- Debian 12 / x86_64
- Ubuntu 20.04 / x86_64
- Ubuntu 22.04 / x86_64
- Ubuntu 23.04 / x86_64

## Install via apt repository:

The APT configuration is for `bullseye` but the Acton package is forward compatible and works just fine on bookworm as well as similarly modern Ubuntu releases.
```sh
wget -q -O - https://apt.acton-lang.io/acton.gpg | sudo apt-key add -
echo "deb [arch=amd64] http://apt.acton-lang.io/ bullseye main" | sudo tee /etc/apt/sources.list.d/acton.list
sudo apt-get update
sudo apt-get install -qy acton
```


# Mac OS X

## Supported versions
- MacOS 10.5 / x86_64
- MacOS 11 / x86_64
- MacOS 12 / x86_64
- MacOS 12 / aarch64 (M1)
  - no pre-built binary package ("bottle" in Homebrew lingo) available so it will build from source
- MacOS 13 / x86_64
- MacOS 13 / aarch64 (M1)
  - no pre-built binary package ("bottle" in Homebrew lingo) available so it will build from source


## Install via Homebrew
```sh
brew install actonlang/acton/acton
```


# Binary release tar ball

There are pre-built binary release tar balls available for download for Linux
and Mac OS X in case the above package formats are not suitable. See the guide,
[installing Acton from a release tar ball](from-tar-ball).


# Building from source

See [building from source](from-source).
