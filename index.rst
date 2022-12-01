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

Guidelines and recommendations
------------------------------

* Good code should not require large amounts of comments to be intelligible. 
  Nevertheless targeted explanations of particular segments of interest are desirable.
  Each source file should have a homogeneous coding style.

* Contours of a routine or module should be considered with care, avoiding excessive length
  or complexity.

* Routine call signatures or interfaces should be designed with care, respecting library 
  contouring. Interfaces that are not internal to a component should privilege as much as 
  possible native fortran datatypes rather than derived types.

* Naming of new variables, routines and modules should help the reader understand code as efficiently 
  as possible. *Renaming of legacy / existing code?*

* Large arrays should be declared as allocatable, to avoid excessive stack usage. 
  Small arrays, and in particular those declared in tight code (this should be avoided wherever 
  possible!) should be automatic, to benefit from faster stack handling.

* If an allocatable variable can be used rather than a pointer, opt for the allocatable for 
  safety reasons.

* In order to make domain decomposition easier to follow, global variable names are suffixed by G, 
  while subdomain-local variables are suffixed by L.

* Different meteorological data formats are used at ECMWF and Meteo-France. 
  The choice between these formats should be based on logical keys LARPEGEF or LARPEGEF_xx 
  (and not LECMWF).

* Aladin routines that are counterparts of IFS/Arpege ones should have the same name but 
  prefixed with E. 
  Aladin counterparts to IFS/Arpege SUxxx setup routines should be named SUE.

* Output that should appear in the main text output file should be written to NULOUT.
  Output to NULOUT must be deterministic and should not change according to the 
  parallel distribution or the time at which the job is run. 
  Error messages should be written to unit NULERR.

* Conditional clauses with multiple cases should be handled with SELECT CASE rather than IF 
  statements followed by multiple ELSEIF statements.

* If execution is to be aborted by the code, a call to ABOR1, with a meaningful message, 
  should be used.
 

 

Rules (as checked by norms checker)
-----------------------------------

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

   rules/L1
   rules/L2
   rules/L3
   rules/L4
   rules/L5
   rules/L6
   rules/L7
   rules/L8
   rules/L9
   rules/L10
   rules/I1
   rules/I2
   rules/I3
   rules/I4
   rules/I5
   rules/I6
   rules/I7
   rules/SC1
   rules/SC2
   rules/SC3
   rules/SC4
   rules/S1
   rules/S2
   rules/S3
   rules/S4
   rules/S5
   rules/S6

   obsolescent/index

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
