Preferred shell options
-----------------------

The following are proposed as a standard set of options to enable in all
scripts, except where there is a clear and documented need to do
otherwise:

-  ``set -e`` / ``-o errexit``: exit immediately when a command fails.
   This reduces the verbosity of error handling, similar to using a
   limited form of exceptions rather than C-like code.
-  ``set -u`` / ``-o nounset``: exit immediately when using an undefined
   variable. This helps to catch the use of undefined or mistyped
   variables, similar to using "``implicit none``" in Fortran.
-  ``set -o pipefail``: consider a pipeline failed if *any* of its
   commands fail, rather than ignoring errors in all but the *last*
   command.
-  ``shopt -s inherit_errexit``: this disables the Bash peculiarity
   where "``set -e``" gets reset within ``$(...)`` command
   substitutions.

These can be combined for brevity:

::

    set -eu -o pipefail
    shopt -s inherit_errexit

The following are worthy of consideration in cases where they are
appropriate:

-  ``shopt -s extglob``: allow various extended globbing syntaxes.
-  ``shopt -s failglob``: exit immediately if a glob matches no
   filenames.
-  ``shopt -s nullglob``: globs that match no files expand to an empty
   string rather than themselves.

Finally, where verbose tracing of the script's execution is desired,
"``set -x`` / ``-o xtrace``" should be used.

More rarely, e.g. when tracking down hard-to-find parsing errors,
"``set -v`` / ``-o verbose``" may also be useful to output a trace as
the script is *read* rather than as it is executed, but this should not
normally be left on in production scripts.

"``set -a`` / ``set -o allexport``" may be used sparingly and locally,
if a whole list of variables are to be exported, but should *always* be
reset immediately afterwards to avoid polluting the environment with
unnecessary variables from the rest of the script.
