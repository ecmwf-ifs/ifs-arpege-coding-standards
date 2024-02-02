Which shell to use
------------------

While there are a variety of different shells around, each with its own
nice features, no other shell has achieved the ubiquity of Bash. It is
the default login shell on most modern Linux systems, leading to
widespread familiarity, and readily available on almost all other
Unix-like platforms.

Greater portability *can* be achieved by sticking to POSIX-standard
shell syntax, which should behave the same in any POSIX-compatible
shell, but this comes at the cost of an extremely restricted feature set
(lacking arrays, ``[[ ... ]]`` conditional syntax, function-local
variables etc.) Outside of specific very simple scripts that may require
portability to exotic or embedded systems which don't support Bash, this
is unlikely to be a good trade-off.

It might be possible to define a supported set of shells, and restrict
to a common subset of their extended functionality and syntax (possibly
with a function library to encapsulate differences). However, it's not
necessarily clear *which* other shells ought to be included here, and
may come at the price of significant complexity in both design and
ongoing testing. (Such an approach might have a *short-term* role while
new scripts co-exist with legacy ksh ones with common includes, however,
as discussed in the migration plan.)

**It is therefore recommended that Bash (version 4.4+) is the default
choice for all shell scripts**, except in rare cases where they must run
on esoteric systems where only a POSIX-compatible ``/bin/sh`` can be
assumed to be available.
