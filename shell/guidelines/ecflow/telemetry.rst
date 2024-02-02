ecFlow telemetry and trapping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is essential that ecFlow task wrappers do an ``ecflow_client --init``
at the start, ``--complete`` on successful completion, and implement
proper trapping to ensure an ``--abort`` on all failures. This should
generally be done by standard “boilerplate” included at the start and
end of the task wrapper, rather than in an ad-hoc way. Trapping should
be set up as early as possible, and special care taken to ensure that
any preceding setup/configuration code will not cause the script to exit
without calling ``ecflow_client --abort``.

It is not normally necessary for scripts *called from* or *sourced from*
the task wrapper to implement trapping themselves, unless required for
their own internal cleanup – if a called script fails or receives a
signal, the task wrapper will see this as a non-zero return code (which
it can handle or abort on explicitly or via “\ ``set -e``\ ” invoking
the wrapper's traps).

Sourced scripts, all functions in Bash, and POSIX ``()`` functions in
Korn shell, will inherit the trapping environment in which they are
called, and failures within them will behave similarly to those in the
body of the script. Changes to traps will propagate back out to the
calling environment, so must be carefully restored on all exit paths.
Such scripts and functions should therefore not change the traps unless
this is precisely their intended and documented purpose.

Called scripts, and ``function``-keyword functions in Korn shell, have
their own trapping environment, so may freely implement their own local
trap handlers without explicitly restoring them on exit, although this
is not necessary simply to maintain correct trapping in the task as a
whole, because the calling script will see and handle the non-zero
return code as outlined above. While trapping within a standalone called
script may be safe and useful, local traps should *not* be set within
functions unless a specific exception is agreed, because this is a
Korn-shell-specific behaviour – if the script is run in Bash, they will
behave the same as POSIX ``()`` functions and changes to the trap will
propagate back out to the caller, potentially breaking the task
wrapper's trapping.
