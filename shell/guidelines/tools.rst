Use of shellcheck and shfmt
---------------------------

The "`shellcheck <https://www.shellcheck.net/wiki/Home>`__" tool can
catch many undesirable practices in shell scripts, including those
discouraged in the Google guide. All scripts should pass its validation
with the all checks enabled and warnings *sparingly* disabled per line
or file where it is really necessary.

The "`shfmt <https://github.com/mvdan/sh>`__" tool can be used to
auto-format shell scripts (much like "black" does for Python) in a
manner consistent with the Google guide, and (unless there are strong
arguments for an exception) all scripts should follow this with an
appropriate set of options (``-i 2 -ci -s``). Editor configuration files
should also be available to support correct formatting as scripts are
written.

There will be a need for some wrapping around these tools, (a) to define
the prescribed set of options to use, (b) to support shell scripts
embedded in ``.ecf`` task wrappers, and (c) to provide appropriate
skeleton config.*.h files to ensure that all the config variables aren't
falsely shown as undefined. These wrappers will probably live either
directly in ifs-scripts, or in ifs-git-tools (like "git ifsnorms" for
Fortran).

Attention should be paid during pull request reviews to whether any new
exceptions to the rules enforced by these tools are justified.

At some point, when we have a "clean" set of scripts, we could consider
enforcing these checks via Git commit hooks, but that's a decision for
the future rather than now. In the meantime, we should aim to check for
regressions against them (i.e. new violations that weren't already
there) via the continuous integration system.
