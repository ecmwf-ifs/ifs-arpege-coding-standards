.. IFS_coding_guidelines documentation master file, created by
   sphinx-quickstart on Wed Aug 31 09:39:39 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to the IFS coding guidelines!
=====================================

Developments to the Arpege/IFS codebase should aim to adhere to the guidelines 
and rules presented in this document.

Guidelines and rules described here are designed to make code easier to read,
easier to maintain, easier to extend, and less prone to bugs. 
Rules are also written so as to allow implementation of automatic checking.

Good code should not require large amounts of comments to be intelligible.
Nevertheless targetted explanataions of particular segments of interest are desirable.

Routine call signatures or interfaces should be designed with care, respecting library contouring
and avoiding excessive argument counts. Derived types can be used to shorten call signatures
where appropriate.

Naming of new variables, routines and modules should help the reader understand code as efficiently as
possible. *Renaming of legacy / existing code?*

Some of the rules relate to the idea of Single Column code, where algorithmic tasks can be expressed
independently of horizontal position, and no horizontal dependencies exist. Code which maps to this 
concept can be modified at compile time by tooling (Loki, Fxtran) in line with architecture-specific
requirements. Such tooling relies heavily on code formatting to determine required transformations, 
explaining the prescriptive rules for these areas of code.

Rules are organised into general language rules (Lw), IFS-specific rules (Ix), stylistic
points (Sy), and Single-Column related rules (SCz).

.. toctree::
   :maxdepth: 2
   :caption: List of rules:

   rules/r1
   rules/r2
   rules/r3
   rules/r4
   rules/r5
   rules/r6
   rules/r7
   rules/r8
   rules/r9
   rules/r10
   rules/r11
   rules/r12
   rules/r13
   rules/r14
   rules/r15
   rules/r16
 
Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
