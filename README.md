# small-sed
by Eric S. Raymond, <esr@snark.thyrsus.com> and Rene Rebe <rene@exactcode.de>

This is a smaller, cheaper, and faster SED utility. Minix uses it. GNU used to use it, until they built their own sed around an extended (some would say over-extended) regexp package and it is used for embedded tasks (for example by the T2 SDE - https://t2sed.org).

The original sed 1.0 was written in three pieces; sed.h, sedcomp.c, sedexec.c. Some Minix hacker ran them together into a single-file version, mnsed.c which is not supported and shipped these days; if changes are needed for Minix please send a patch to the normal source.

# Revision History
The 1.2 version (9 Oct 1996) add mnsed's support for detecting truncated hold spaces. The mnsed version is missing one feature in of the 1.2 version; support of +. Also, the multiple-file I/O is organized slightly differently.

The 1.3 version added a bug fix by Tom Oehser, and the `L' command.  Also this program is now distributed under GPL.

The 1.5 version incooperated a lot of bug fixes by Rene Rebe as well as a real test suite. Also the function declaration and definition have been converted from the K&R C to ANSI C.

The 1.6 version includes support for the n'th match for the substitude command as well as support for predefined character classes and only writes lines with newline if one was present in the input line (compatible with GNU sed).

The 1.7 version fixed a segmentation fault with empty regular expressions, not to leak other buffer content for groups of commands and escaping numerical seperators in regular expressions by disabling obscure code. Additionally compilation with older compilers as well as warnings with the latest gcc versions have been corrected.

The 1.8 version fixes matching of some escaped characters (a regression introduced with \+ star matching), \+ star matching to corretly copy and mark the internal bytecode representation, back references inside lhs regular expressions matching (to work at all) and marking the correct regular expression for star matches.

The 1.9 version included a microoptimization shaving some bytes off the binary and some cpu cycles at run time, reusing the previous regular expressions for empty ones, predefined character classes with control characters, handling of escaped ampesands and support for backreference \0 and Kleene star operator on groups.

The 1.10 version fixed a special case of grouped star matching where \+1..n overwrote the last match, not to infinite loop on certain zero match grouped star cases and not to crash on w(rite to file). The version also no longer falls into the conservative end-of-file matching mode when just end-of-line matching was used.

The 1.11 version again fixed w(rite to file) handling to correctly honor /dev/stdout and /dev/stderr as GNU sed does and thus keep the streams in sync. Some unused variables have been removed and a two diagnostics fixed to be printed correctly.

The 1.12 version fixed the l(ist) command to actually work, some tiny optimizations have been performed as well as some more compiler warnings fixed.

The 1.13 version address some pedantic compiler warnings, improves the Makefile and renamed the getline() function, as glibc-2.10 introduced its own.

The 1.14 version fixes a C++-style comment, and clarifies the license as BSD-like in agreement with Eric S. Raymond.

The 1.15 version fixes some Kleene star operator relates bugs and includes some code cleanups.

The 1.16 version implemented 8-bit clean character groups.

The 1.17 version added support for #n, and fixed support for multiple -e command line arguments.

## Original file listing
Makefile	-- how to build sed
sed.h		-- declarations and structures
sedcomp.c	-- sed pattern compilation
sedexec.c	-- sed program execution
minised.1		-- source for the man page
tests/		-- a small set of sed tests

For some releases the man page in the man format, or surf to:

   https://exactcode.de/oss/minised/
   http://www.catb.org/~esr/

for updates of this software. There is a sed FAQ kept at these
locations:

   http://www.dreamwvr.com/sed-info/sed-faq.html
   
## Line Endings
The text and source files in this repository originally used CR line endings, as usual for Apple II text files, but they have been converted to use LF line endings because that is the format expected by Git. If you wish to move them to a real or emulated Apple II and build them there, you will need to convert them back to CR line endings.

If you wish, you can configure Git to perform line ending conversions as files are checked in and out of the Git repository. With this configuration, the files in your local working copy will contain CR line endings suitable for use on an Apple II. To set this up, perform the following steps in your local copy of the Git repository (these should be done when your working copy has no uncommitted changes):

1. Add the following lines at the end of the `.git/config` file:
```
[filter "crtext"]
	clean = LC_CTYPE=C tr \\\\r \\\\n
	smudge = LC_CTYPE=C tr \\\\n \\\\r
```

2. Run the following commands to convert the existing files in your working copy:
```
rm .git/index
git checkout HEAD -- .
```

Alternatively, you can keep the LF line endings in your working copy of the Git repository, but convert them when you copy the files to an Apple II. There are various tools to do this.  One option is `udl`, which is [available][udl] both as a IIGS shell utility and as C code that can be built and used on modern systems.

Another option, if you are using the [GSPlus emulator](https://apple2.gs/plus/) is to host your local repository in a directory that is visible on both your host computer, and the emulator via the excellent [Host FST](https://github.com/ksherlock/host-fst).

[udl]: http://ftp.gno.org/pub/apple2/gs.specific/gno/file.convert/udl.114.shk

## File Types
In addition to converting the line endings, you will also have to set the files to the appropriate file types before building ORCA/Modula-2 on a IIGS.

For each of the different groups of code, there is a`fixtypes` script (for use under the ORCA/M shell) that modifies the file and aux type of all source and build scripts, *apart from the fixtures script itself!*

So, once you have the files from the repository on your IIGS (or emulator), within the ORCA/M shell, execute the following command on each `fixtypes` script:

    filetype fixtypes src 6