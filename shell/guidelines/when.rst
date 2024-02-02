When (not) to use shell
-----------------------

Shell scripting should be primarily considered for individual short
simple scripts, with more structured languages preferred for larger and
more complex code.

Shell is particularly suited to being the top-level wrapper or glue that
sets up the appropriate environment before executing a more complex
program, or a simple sequence of them. (For example, an ecFlow task
wrapper that extracts the relevant parameters from ecFlow variables and
passes them to a complex executable; or something like ``run_parallel``
which sets up the appropriate environment for srun.)

However, it is much less suitable for complex logic and data/text
processing. Tasks like the generation of one set of configuration files
from another (e.g. generating Fortran namelists based on a suite or
experiment configuration, possibly modulated by knowledge of current
state) can generally be expressed more clearly and robustly by calling
out to a more structured language such as Python. (Conversely, Python is
*not* generally a good choice as the wrapper which forks subprocesses,
even more so when parallelisation is required.)

It is therefore recommended that, while shell may be the appropriate
choice for the outermost layer and simple subtasks, this should delegate
to something else (typically Python, unless it’s handled by a compiled
language) for complex processing and logic.

Separate guidance will also be developed for Python, but in general
Python components need to have good interfaces, test coverage and
documentation to ensure that (as with compiled Fortran code) they are
unlikely to need on-the-fly debugging or internal modification in an
operational context.
