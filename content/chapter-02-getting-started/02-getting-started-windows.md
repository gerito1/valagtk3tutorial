
# Getting Started in Windows

## Initial Warning

Due the lack of documentation/support working with vala in windows
could be easily a painful experience however if you want to (for any
circumstances) start programming vala in windows this guide should
help you.

## Before Start

### Your editor

In order to program it's essential to use an editor. The editor is
the tool which edits the source code and therefore an essential part
of your toolkit. In principle you could use *any* program that can
edit a text file (source files are merely text files) but there is
a detail to be aware. Windows and Unix systems (as Linux and Mac OS)
use a different format to handle text files therefore not any program
would be equally suitable for program in vala.

Because vala is a programming language used mostly in Unix systems
and a lot of resources are available for Unix, it would be a nice
idea use an editor which support Unix's NewLine format.

If you want more information you could read this wikipedia's article
[NewLine](http://en.wikipedia.org/wiki/Newline).


Microsoft's Notepad, only supports Windows NewLine format but if you
try to open files in Unix format the text might be messy on it. So because
vala is used mostly in Unix it would be most likely to find online
files in Unix format rather than Windows format and thus this advice.

Some editors capable of read Unix and Windows format:

[Notepad2](http://www.flos-freeware.ch/notepad2.html)

[Notepad++](http://www.notepad-plus-plus.org/)

[Metapad](http://liquidninja.com/metapad/)

[SciTE](http://www.scintilla.org/SciTE.html)

[Gedit](http://ftp.gnome.org/pub/GNOME/binaries/win32/gedit/)

It's up to you choose the proper editor. Some have even syntax highligting
and other interesting features but that goes beyond this tutorial.

## Internet

This guide assumes you have access to internet.

## Installation

Before we can begin programming, we'll need to install GTK and Vala, and gcc
along with their dependencies.

In Windows this not a trivial task by any means, so due the lack of online
information this document will introduce to you this task.

The easiest way in Windows is using distribution software and there are
2 options MSYS2 and cygwin.

### MSYS2

MSYS2 is a minimal system built over MinGW (minimal Gnu Windows). MinGW
is a minimal environment for the Windows systems consist in the compiler
gcc and other utilities (the gnu tool-chain) and libraries to build
software in windows. MSYS2 goes beyond and tries to provide all the system tools
to make easy the development and distribution of software in windows.
Consist in the compiler suite (provided by mingw), a shell provided by bash
some system utilities of the project GNU project and a package manager pacman
to distribute pre-built packages, this is compiled ready to use.

The MSYS2  [Installation guide](http://sourceforge.net/p/msys2/wiki/MSYS2%20installation/). Please read it.

But unfortunately due some current issues you might want to follow this step by step guide.

1) First get the proper MSYS package for your system.

Get the propper installer from here (this is 32 or 64 bits) [http://msys2.github.io/](http://msys2.github.io/)

You can choose the more recent .exe file.

2) After the download execute the .exe.

It should ask for a destination **C:\msys32** or **C:\msys64** respectively.
As an advice the path should be the shortest possible and **without spaces**
because they sometimes cause problems. You can choose another unit but
try to keep the path short and **without spaces**.

Use the recommended options.

After the copy of the files. Tick **Run MSYS2 now**.

It should start automatically the MSYS2 shell
if not go to your installation directory and open **msys2_shell.bat**.

The window opened (the MSYS2 shell) is similar to the Windows' command but uses
a program called bash that is used in Unix systems.

Receives any text command, and if it is valid it will execute it, if is not
it will emit an error message.

3) Write or copy

    # pacman -Sy

This command updates the database of pacman (the package system of MSYS2)
if you don't have internet it will emit an error message.

4) Then for update your essential packages

    # pacman -S --needed filesystem msys2-runtime bash libreadline libiconv libarchive libgpgme libcurl pacman ncurses libintl
    
5) Exit and open your MSYS2 shell

    # pacman-key --init
    # pacman-key --populate msys2
    # pacman-key --refresh-keys

This is done because there's a problem in MSYS2, if you try to update
the packages pacman will claim your packages are corrupted.

6) Update you packages

    # pacman -Su

Sometimes pacman warns you about conflicting files. The output is usually like this:

    #error: failed to commit transaction (conflicting files)
    #crypt: /usr/share/doc/MSYS/crypt.README exists in filesystem
    #Errors occurred, no packages were upgraded.

In that case add the option --force command and try again

    # pacman -Su --force

#### Installing GTK+3 in MSYS2

You will need to install Gtk+3 libraries in order to compile the programs
in this tutorials. Fortunately pacman will help us

On a 32 bits installation the command to do this would be:

    # pacman -S mingw-w64-i686-gtk3 mingw-w64-i686-pkg-config 

On 64 bits


    # pacman -S mingw-w64-x86_64-gtk3 mingw-w64-x86_64-pkg-config


The package mingw-w64-{**version**}-gtk3 installs all the packages needed
for gtk3 and gtk3 itself, the mingw-w64-{**version**}-pkg-config install
pkg-config a tool necesary to locate installed packages in the system,
it warns to the compiler all the info needed to compile correctly the
programs, if not installed it will apear some strange messages.

#### Installing Vala and Gcc in MSYS

Valac it's the vala compiler, in this case transforms the vala files
into C files and then gcc transforms them into executables.

On 32 bits:

    # pacman -S mingw-w64-i686-vala gcc

On 64 bits run:

    # pacman -S mingw-w64-x86_64-vala gcc


## References and Further Reading

* MSYS2 Official site [Online] Available from: [http://msys2.github.io/](http://msys2.github.io/)
  [Accessed 3 November 2014]
  
* MSYS2 Installing guide. [Online] Available from:
  [http://sourceforge.net/p/msys2/wiki/MSYS2%20installation/](http://sourceforge.net/p/msys2/wiki/MSYS2%20installation/)
  [Accessed 3 November 2014]

* Vala Tools. [Online] Available from:
  [https://wiki.gnome.org/Projects/Vala/Tools](https://wiki.gnome.org/Projects/Vala/Tools)
  [Accessed 16 September 2014]
  
## Aditional Reading

* How to build your GTK+ application on Windows. [Online] Available from:
  [http://blogs.gnome.org/nacho/2014/08/01/how-to-build-your-gtk-application-on-windows/](http://blogs.gnome.org/nacho/2014/08/01/how-to-build-your-gtk-application-on-windows/)
  [Accessed 3 November 2014]
  
* Pacman manpages. [Online] Available from: [http://www.archlinux.org/pacman/pacman.8.html](https://www.archlinux.org/pacman/pacman.8.html)
[Accessed 3 November 2014]

* Bash manpages. [Online] Available from: [http://linux.die.net/man/1/bash](http://linux.die.net/man/1/bash)
[Accessed 3 November 2014]