+++
title = "Install binary release tar ball"
template = "page.html"
+++
# Install Acton from binary release tar ball

There are pre-built binary release tar balls available for download for Linux
and Mac OS X in case the above package formats are not suitable. Download a
tar ball from [the Release page](https://github.com/actonlang/acton/releases).
Pick the latest stable versioned release.

In case you are looking to live on the bleeding edge or have been asked by a
developer (in case you ran into a bug) you can pick `tip`, which is built
directly from the `main` branch.

Extract the Acton tar ball:
```
$ tar jxvf acton-*
```

You will want to include the `acton/bin` directory in your `PATH` so you can use
`actonc`.
