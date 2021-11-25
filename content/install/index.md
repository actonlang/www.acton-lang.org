+++
title = "Install"
weight = 1
template = "page.html"
+++

# Install

Acton is available for Linux and Mac OS X.

# Debian / Ubuntu

Install Acton via apt repository:

```sh
wget -q -O - https://apt.acton-lang.io/acton.gpg | sudo apt-key add -
echo "deb http://apt.acton-lang.io/ bullseye main" | sudo tee /etc/apt/sources.list.d/acton.list
sudo apt-get update
sudo apt-get install -qy acton
```


# Mac OS X using Homebrew

Acton is available as a Homebrew tap for x86_64 CPUs. Apple M1 silicon is not currently supported.

```sh
brew install actonlang/acton/acton
```


# Binary release tar ball

There are pre-built binary release tar balls available for download for Linux
and Mac OS X in case the above package formats are not suitable. See the guide,
[installing Acton from a release tar ball](from-tar-ball).


# Building from source

See [building from source](from-source).
