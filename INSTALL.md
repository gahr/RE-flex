[![logo][logo-url]][reflex-url]

Installation
------------

### Windows users

Use `reflex/bin/reflex.exe` from the command line or add a **Custom Build
Step** in MSVC++ as follows:

1. select the project name in **Solution Explorer** then **Property Pages**
   from the **View** menu (see also
   [custom-build steps in Visual Studio](http://msdn.microsoft.com/en-us/library/hefydhhy.aspx));

2. add an extra path to the `reflex/include` folder in the **Include
   Directories** under **VC++ Directories**, which should look like
   `$(VC_IncludePath);$(WindowsSDK_IncludePath);C:\Users\YourUserName\Documents\reflex\include`
   (this assumes the `reflex` source package is in your **Documents** folder).

3. enter `"C:\Users\YourUserName\Documents\reflex\bin\reflex.exe" −−header-file
   "C:\Users\YourUserName\Documents\mylexer.l"` in the **Command Line** property
   under **Custom Build Step** (this assumes `mylexer.l` is in your
   **Documents** folder);

4. enter `lex.yy.h lex.yy.cpp` in the **Outputs** property;

5. specify **Execute Before** as `PreBuildEvent`.

If you are using specific reflex options such as `--flex` then add these in
step 3.

Before compiling your program with MSVC++, drag the folders `reflex/lib` and
`reflex/unicode` to the **Source Files** in the **Solution Explorer** panel of
your project.  Next, run `reflex.exe` simply by compiling your project (which
may fail, but that is OK for now as long as we executed the custom build step
to run `reflex.exe`).  Drag the generated `lex.yy.h` and `lex.yy.cpp` files to
the **Source Files**.  Now you are all set!

In addition, the `reflex/vs` directory contains batch scripts to build projects
with MS Visual Studio C++.

### Unix/Linux and macOS

On Linux and macOS systems you can use [homebrew](https://brew.sh) to install
RE/flex with `brew install re-flex`.

Otherwise, you have two options: 1) quick install or 2) configure and make.

### Quick install

For a quick clean build assuming your environment is pretty much standard:

    $ sh clean.sh
    $ sh build.sh

This compiles the **reflex** tool and installs it locally in `reflex/bin`.  You
can add this location to your `$PATH` variable to enable the new `reflex`
command:

    export PATH=$PATH:/reflex_install_path/bin

The `libreflex.a` and `libreflex.so` libraries are saved locally in
`lib`.  Link against one of these libraries when you use the RE/flex regex
engine in your code.  The RE/flex header files are locally located in
`include/reflex`.

To install the man page, the header files in `/usr/local/include/reflex`, the
library in `/usr/local/lib` and the `reflex` command in `/usr/local/bin`:

    $ sudo sh allinstall.sh

### Configure and make

The configure script accepts configuration and installation options.  To view
these options, run:

    $ ./configure --help

Run configure and make:

    $ ./configure && make

To build the examples also:

    $ ./configure --enable-examples && make

After this successfully completes, you can optionally run `make install` to
install the `reflex` command and `libreflex` library:

    $ sudo make install

Unfortunately, cloning from Git does not preserve timestamps which means that
you may run into "WARNING: 'aclocal-1.15' is missing on your system."  To
work around this problem, run:

    $ autoreconf -fi
    $ ./configure && make

### Optional libraries to install

- To use Boost.Regex as a regex engine with the RE/flex library and scanner
  generator, install [Boost][boost-url] and link your code against
  `-lboost_regex`.

- To use PCRE2 as a regex engine with the RE/flex library and scanner
  generator, install [PCRE2][pcre2-url] and link your code against
  `-lpcre2-8`.

- To visualize the FSM graphs generated with **reflex** option `--graphs-file`,
  install [Graphviz dot][dot-url].

### Improved Vim syntax highlighting for Flex and RE/flex

Copy the `lex.vim` file to `~/.vim/syntax/lex.vim` to enjoy improved syntax
highlighting for Flex and RE/flex.


Examples
--------

The examples require the [Bison][bison-url] tool to demonstrate scanning with
parsing using the **reflex** `--bison` and `--bison-bridge` options.

To build the examples, first build the reflex tool and then execute:

    $ cd examples
    $ make

If `libboost_regex` could not be found, then edit lib/Make to change the
installation location of Boost so that `boost/regex.hpp` and `libboost_regex`
can be properly located, for example:

    INCBOOST = /opt/local/include
    LIBBOOST = /opt/local/lib/libboost_regex-mt.dylib


Documentation
-------------

To view the documentation visit [manual][manual-url] or open
`doc/html/index.html` in a browser.

The documentation is generated by Doxygen from `doc/index.md` and several
source code files.  To generate the documentation:

    $ make doc/html


Files
-----

The RE/flex distribution directory tree should at a minimum contain the
following files:

    reflex/
    |
    |__ bin/
    |   |__ reflex.exe            Windows executable reflex
    |
    |__ doc/
    |   |__ Doxyfile
    |   |__ README.md             URLs to online documentation
    |   |__ footer.html
    |   |__ header.html
    |   |__ reflex-logo.png
    |   |__ html/
    |   |   |__ index.html        HTML documentation (also online)
    |   |__ man/
    |       |__ reflex.1          man page
    |
    |__ examples/
    |   |__ Make                  quick-build Make(file)
    |   |__ Makefile.am           automake file
    |   |__ Makefile.in           automake file
    |   |__ build.sh              quick build examples
    |   |__ ...
    |
    |__ fuzzy/                    fuzzy (approximate) pattern matcher
    |   |__ README.md
    |   |__ Makefile
    |   |__ fuzzymatcher.h        FuzzyMatcher class for fast fuzzy matching
    |   |__ ftest.cpp             FuzzyMatcher testing
    |
    |__ include/                  reflex header files to install
    |   |__ reflex/
    |       |__ abslexer.h
    |       |__ absmatcher.h
    |       |__ bits.h
    |       |__ boostmatcher.h
    |       |__ convert.h
    |       |__ debug.h
    |       |__ error.h
    |       |__ flexlexer.h
    |       |__ input.h
    |       |__ matcher.h
    |       |__ pattern.h
    |       |__ pcre2matcher.h
    |       |__ posix.h
    |       |__ ranges.h
    |       |__ setop.h
    |       |__ stdmatcher.h
    |       |__ timer.h
    |       |__ traits.h
    |       |__ unicode.h
    |       |__ utf8.h
    |
    |__ lib/                      reflex library files to install
    |   |__ Make                  quick-build Make(file)
    |   |__ Makefile.am           automake file
    |   |__ Makefile.in           automake file
    |   |__ convert.cpp
    |   |__ debug.cpp
    |   |__ error.cpp
    |   |__ input.cpp
    |   |__ matcher.cpp
    |   |__ pattern.cpp
    |   |__ posix.cpp
    |   |__ unicode.cpp
    |   |__ utf8.cpp
    |
    |__ src/                      reflex command-line tool
    |   |__ Make                  quick-build Make(file)
    |   |__ Makefile.am           automake file
    |   |__ Makefile.in           automake file
    |   |__ reflex.cpp
    |   |__ reflex.h
    |
    |__ tests/
    |   |__ Make                  quick-build Make(file)
    |   |__ Makefile.am           automake file
    |   |__ Makefile.in           automake file
    |   |__ rtest.cpp
    |   |__ ...
    |
    |__ unicode/
    |   |__ Make                  quick-build Make(file)
    |   |__ Blocks.txt            Unicode source file
    |   |__ Scripts.txt           Unicode source file
    |   |__ UnicodeData.txt       Unicode source file
    |   |__ block_scripts.l       convert Blocks.txt to block_scripts.cpp
    |   |__ block_scripts.cpp
    |   |__ language_scripts.l    convert Scripts.txt to language_scripts.cpp
    |   |__ language_scripts.cpp
    |   |__ letter_scripts.l      convert UnicodeData.txt to letter_scripts.cpp
    |   |__ letter_scripts.cpp
    |
    |__ vs/                       Visual Studio batch scripts
    |   |__ ...
    |
    |__ INSTALL.md                this file
    |__ LICENSE.txt               BSD-3 license
    |__ README.md                 README to get started, changelog
    |__ CONTRIBUTING.md           about contributing
    |__ CODE_OF_CONDUCT.md        when contributing
    |__ Makefile.am               automake file
    |__ Makefile.in               automake file
    |__ allinstall.sh             quick build and install
    |__ build.sh                  quick build (local build)
    |__ clean.sh                  quick cleanup
    |__ configure                 ./configure && make
    |__ lex.vim                   improved Flex and RE/flex Vim syntax file

License and copyright
---------------------

RE/flex by Robert van Engelen, Genivia Inc.
Copyright (c) 2016-2020, All rights reserved.   

RE/flex is distributed under the BSD-3 license LICENSE.txt.
Use, modification, and distribution are subject to the BSD-3 license.

[logo-url]: https://www.genivia.com/images/reflex-logo.png
[reflex-url]: https://www.genivia.com/get-reflex.html
[manual-url]: https://www.genivia.com/doc/reflex/html
[flex-url]: http://dinosaur.compilertools.net/#flex
[lex-url]: http://dinosaur.compilertools.net/#lex
[bison-url]: http://dinosaur.compilertools.net/#bison
[dot-url]: http://www.graphviz.org
[FSM-url]: https://www.genivia.com/images/reflex-FSM.png
[boost-url]: http://www.boost.org
[pcre2-url]: http://pcre.org
