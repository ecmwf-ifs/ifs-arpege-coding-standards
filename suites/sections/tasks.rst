Tasks
=====

Clarity of function
-------------------

In principle, tasks should perform a single logical function.
The task name should clearly indicate this function without
unnecessarily obtuse abbreviation.

However, the granularity should not be so fine that their actual runtime
is dwarfed by overheads, nor so coarse that individual tasks take longer
than an hour.

A task should have consistent parallelism throughout: it is wasteful of
computing resources to do a significant amount of serial work within a
parallel task, keeping many CPUs or whole nodes idle. Such work should
normally be split out into separate serial tasks connected by
appropriate triggers.


Re-runnability
--------------

Tasks should be re-runnable in a way that is reproducible and idempotent.
Running with the same configuration and inputs should always produce the
same outputs.  
They should not fail, nor produce different output, because they have
already been run (either in full or in part).
Running a task multiple times should not impact the rest of the suite.

To achieve this, it's recommended to use a clean temporary working
directory during each task, copying outputs to a shared location for
other tasks to pick up on successful completion.

If any special user action is required when re-running a task, this
should be clearly documented in the manual: e.g. if another task must
also be re-run, or a variable set to prevent re-using corrupt outputs
from the previous run.


Error handling
--------------

Tasks should have thorough and consistent error checking and reporting.
Anticipated error conditions should be checked and produce clear and
meaningful error messages.
Unanticipated errors should also be detected and reported, for example
using ``set -e`` in the shell.

Correct error traps / signal handling should be maintained in the
top-level task process to ensure that it notifies ecFlow on all abort
paths.

External scripts or programs called by the task should also have
appropriate error checking, and be maintained with their own test
suites.
      

Top-level ecFlow task scripts
-----------------------------

Tasks should be shell scripts at the top level (preferably Bash, following the
applicable standards (e.g. `these for IFS-related tasks <https://sites.ecmwf.int/docs/ifs-arpege-coding-standards/shell/>`_).
These should be either produced at suite creation time by a tool like
pyFlow, or deployed from a version control system like Git.

These scripts should be as simple as possible, with complex work
devolved to external well-tested scripts or programs in a more suitable
language.
