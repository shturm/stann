# Building ANSI C program

## GCC vs Microsoft compilers

MSVC++, C# and other compilers usage is mostly operated via GUI using Visual Studio IDE. GCC isn't much different than the Microsoft compilers in terms of usage but doesn't come with a single strongly coupled IDE (like Visual Studio) and thus requires initial knowledge to get your project to build.

This isn't because GCC authors and consumers are incapable of producing a shiny tool for developing and building C/C++ code (Eclipse, Netbeans, ... to name a few), but because they aim to resolve the issue of fast-building and thoroughly-configured applications. ASCII text file configurations (like the Makefile) and command-line environments are natural in UNIX systems. Toughen up and get comfortable with it.

To soften the hardship of your first encounter I will provide you with different approaches to get you going, each with increasing

## Build automatically all files

```
cc *.c # compile all .c files
./a.out # run the program
```

That won't handle any missing libraries, warnings and other mishaps but will get you started building and running your program.

## Build with a Makefile using multiple options

Declare some variables

```
CC ?= cc # or gcc
CCFLAGS ?= -Wall
```

First compile core libraries. 
```
all:
	$(CC) $(CCFLAGS) -c app.core.c -o app.core.o
	$(CC) $(CCFLAGS) app.exec.c app.core.o
```
`all:` defines the default job that will execute when you run `make` with no parameters in the `Makefile` directory.

`$(CC) $(CCFLAGS) -c app.core.c -o app.core.o` compiles just `app.core.c` and `app.core.h`. `app.core.h` is referenced from within `aap.core.c` so it gets compiled  too.

The `-c` option cancels linking so the compiler doesn't look for `main()` function and other libraries that we compile/provide manually later on. This allows us to recompile only small portions of the program and benefits large projects. Every programmer will at least once work on a big project where this will be very useful.

`$(CC) $(CCFLAGS) app.exec.c app.core.o` builds your executable from `app.exec.c` where the `main()` function is, along with the core of our app - `app.core.o`.


To rebuild or simply clean up your working space you want to have a `clean` task. Expect to find a `clean` task in most projects. Here is ours:

```
clean:
	rm -rf *.a
	rm -rf *.o
	rm -rf a.out
```

It removes
- library files - `.a` (`a` stands for archive)
- precompiled files - `.o` like `app.core.o` which is produced on the above task
- default output executable file `a.out`
