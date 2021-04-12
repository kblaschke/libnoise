# libnoise

A portable, [open-source](LICENSE.md), coherent noise-generating library for C++

---

libnoise is a portable C++ library that is used to generate [coherent noise](doc/Glossary.md#coherent-noise), a type of
smoothly-changing noise. libnoise can generate [Perlin noise](doc/Glossary.md#perlin-noise), ridged multifractal noise,
and other types of coherent-noise.

Coherent noise is often used by graphics programmers to generate natural-looking textures, planetary terrain, and other
things.

In libnoise, [coherent-noise](doc/Glossary.md#coherent-noise) generators are encapsulated in classes
called [noise modules](doc/Glossary.md#noise-module). There are many different types of noise modules. Some noise
modules can combine or modify the outputs of other noise modules in various ways; you can join these modules together to
generate very complex coherent noise.

libnoise is known to compile on all platforms and architectures that support C++.

See [INSTALL.md](INSTALL.md) for instructions on how to build and use the library.

Note: libnoise does not generate images, it only generates coherent-noise values. It is up to the programmer to use
these values to generate images.