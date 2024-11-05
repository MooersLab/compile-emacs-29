![Version](https://img.shields.io/static/v1?label=compile-emacs-29&message=0.1&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Protocols for compiling GNU Emacs version 29 from source code with Tree-Sitter support

I switched to using version 29 about six months ago.
This is the current version of Emacs.


## Prerequisites
You must install the tree-sitter C library before compiling Emacs.
The [emacs-tree-sitter.el](https://github.com/emacs-tree-sitter/elisp-tree-sitter) package is built into Emacs29 and 30 but not the C library.
I installed the tree-sitter C library with macports and with homebrew.
I am not sure which was used when compiling Emacs.
I had also installed imagemagick with macports or homebrew a long time ago.

## Download Emacs source code
This is what I used on October 13, 2024.
Note that in lieu of using `git clone`, you can download a tar file of the source code for the desired version from the [GitHub Repository](https://ftp.gnu.org/gnu/emacs/) for GNU Emacs.
I had to do the latter because the former gave me version 31 of Emacs, the bleeding edge development version.

## Compile protocol

untar the downloaded Emacs source code and move to the unpacked folder.

```bash
export LDFLAGS=-L/usr/local/Cellar/giflib/5.2.2/lib
./autogen.sh
mkdir build
cd build
../configure --with-gnutls=ifavailable --with-cairo --with-xwidgets\
--with-tree-sitter --with-imagemagick --program-suffix=29 \
--prefix=/Users/blaine/bin
# Use 8 processors to speed up make.
make -j8
make install -j8
# Note that the installation did not go quite as planned. 
# I resolved the problem by the next steps.
cd nextstep
cp -R Emacs.app /Applications/Emacs29.4.app
# Make a profile directory. 'O' is for a profile centered on org-mode. 
# 'S' is for using straight as the package manager.
# Note that I do not hide this directory with a preceding dot.
mkdir ~/e29os
# I store this alias in a .bashAppAliases file that I source fromm my .zshrc file.
alias e29os='/Applications/Emacs29.4.app/Contents/MacOS/Emacs\
 --init-directory ~/e29os --debug-init'
e29os # Emacs should fire up.
```

## Now check on the configuration features inside Emacs.

```emacs
C-h v  % enter at the : prompt
system-configuration-features RET

% The following should be returned:
ACL DBUS GIF GLIB GMP GNUTLS IMAGEMAGICK JPEG JSON LCMS2 LIBXML2 MODULES NOTIFY\
KQUEUE NS PDUMPER PNG RSVG SQLITE3 THREADS TIFF TOOLKIT_SCROLL_BARS TREE_SITTER\
 WEBP XIM ZLIB XWIDGETS
```

After a major upgrade to the operating system, like from Samona to Sequoia, Emacs will have to be recompiled.
Remove the old build directory if it is still there.
Make a new build directory and carry on as before.

|Version       |Changes                                                                                               |Date                  |
|:-------------|:-----------------------------------------------------------------------------------------------------|:--------------------:|
| Version 0.1  | Initiated project. Added badges, funding, and update table.                                           | 2024 November 5     |



## Sources of funding

- NIH: R01 CA242845
- NIH: R01 AI088011
- NIH: P30 CA225520 (PI: R. Mannel)
- NIH: P20 GM103640 and P30 GM145423 (PI: A. West)

