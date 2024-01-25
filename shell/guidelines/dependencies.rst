Available commands and dependencies
-----------------------------------

Most of the time, our scripts currently run on a constrained set of
GNU/Linux based systems, where the availability of various GNU
extensions can be assumed on top of the standard commands specified by
POSIX (both additional commands and options to standard ones).

However, there are exceptions - for example for OpenIFS usage, or local
IFS development and testing, on other non-GNU or non-Linux platforms
(notably macOS), even if they are Unix-like and POSIX-compliant.
(Previous HPC systems before the Cray were also not necessarily
GNU/Linux-based.)

Scripts should therefore avoid using GNU-specific commands or extensions
when there is an alternative POSIX command or syntax that is equally
suitable. Where there is significant benefit however (in terms of
simplicity, clarity, performance, reliability etc.), GNU extensions may
be used, but the relevant GNU tools should be clearly documented as a
dependency of the script in case they need to be installed explicitly on
non-GNU-based platforms.

More broadly, *any* external command (or package of them) that's not
part of the POSIX standard should be documented as a dependency of the
script and/or the package of which it is part.
