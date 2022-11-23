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

Good code should not require large amounts of comments to be intelligible.
Nevertheless targetted explanataions of particular segments of interest are desirable.

Routine call signatures or interfaces should be designed with care, respecting library contouring
and avoiding excessive argument counts. Derived types can be used to shorten call signatures
where appropriate.

Naming of new variables, routines and modules should help the reader understand code as efficiently as
possible. *Renaming of legacy / existing code?*

Rules are organised into general language rules (Lx), IFS-specific rules (Iy) and stylistic
points (Sz).

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
