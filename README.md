# Introduction

## Libraries

### Freeglut

Freeglut is quick and effective. It's a very small window library that provided a few very basic functionality for giving you a window and getting into an OpenGL context. If you are on the Raspberry Pi, or just want somewhere to get started with OpenGL then this is a good library to go with.

### glfw

Glfw is the window library currently used in the OpenGL red book. It's a simple library, similar to Freeglut with a few more controls on window hints. There are a lot of resources to work with between their site and RedBook examples on OpenGL. While the library is written in C a lot of the examples are written in C++ and use GLM. One note is that depending on your environment, this is not the easiest library to compile and run.

### SDL

SDL is definitely one of the most seductive and compelling libraries out there. It's a full fledged library which includes api's for audio, video, controls and the library is supported to cross compile to a lot of different platforms. It's also easy to install the tools required to compile and run programs in it. The downside is that for the wide adoption of emulators and games that use SDL, I can't for the life of me find any resources in C or Linux to figure out how to get a handle on this library.

### GTK

GTK is the standard this site uses. The most difficult part is figuring out the syntax for creating and looping a `GTKGLArea`, but once you get the basics down, it's a pretty versatile toolkit to work with with a lot of options for expansion. The XML and approach for layouts is kind of confusing if you want to branch out, but in terms of creating applications in C, on Linux, you'd be hard pressed to find better tools than this.

### EGL

"Embedded Graphics Library"

### Raylib

I haven't used it directly
