======================================================
IFS and IFS Component CMake Coding Standard
======================================================

Overview
========

This document defines the coding style and conventions for CMake build scripts
used in the **IFS system** and its **component repositories**, including but not
limited to:

- The central **IFS** build system and its integrated subprojects (e.g. ifsobs, odb)
- External but aligned components such as:

  * `ecRad <https://github.com/ecmwf-ifs/ecrad>`_ — radiation scheme
  * `ecWAM <https://github.com/ecmwf-ifs/ecwam>`_ — wave model
  * `ecTrans <https://github.com/ecmwf-ifs/ectrans>`_ — spectral transform library

All these projects share a common build and packaging framework based on
**ecbuild** and follow consistent CMake idioms and project structure.

This standard complements the existing **Fortran**, **Shell**, and **Suites**
standards in the
`IFS–ARPEGE Coding Standards <https://github.com/ecmwf-ifs/ifs-arpege-coding-standards>`_
repository.

The goal is to maintain a **consistent, portable, and maintainable** build
configuration style across all IFS-related code bases—whether built within
the main IFS or as a standalone external project.

---

Scope
=====

This standard applies to:

- The **IFS master build** (`ifs/CMakeLists.txt`)
- IFS internal subprojects (e.g. `ifsobs`, `odb`, `fiat`)
- Independent IFS component repositories (e.g. `ecrad`, `ecwam`, `ectrans`)
- All supporting CMake modules under the ``cmake/`` directory

It does **not** prescribe configuration for generic third-party libraries,
though integration via `ecbuild_find_package()` should follow the same style.

---

General Principles
==================

Style Philosophy
----------------
- Prefer **clarity over cleverness**: explicit CMake commands over nested logic.
- Follow **ecbuild** conventions where available; do not reinvent equivalents.
- Keep configuration **self-contained** per repository.
- Ensure consistency between the central IFS build and the standalone builds
  of extracted components.
- Use **comments liberally** — CMake is configuration code that should be easy to read.

File Naming and Structure
-------------------------
- Each buildable directory must include a ``CMakeLists.txt`` file.
- Project-specific helper modules reside in a ``cmake/`` directory.
- File naming uses lowercase, underscore-separated names (``ifs_target_flags.cmake``).
- Module naming should include project context, e.g. ``ecwam_target_setup.cmake``.

---

Minimum Requirements and Project Setup
======================================

Required Version
----------------
Specify the minimum required version explicitly::

  cmake_minimum_required(VERSION 3.20 FATAL_ERROR)

Use the lowest version that supports the required features.

Project Declaration
-------------------
Declare the project name, supported languages, and optional version::

  project(ecwam VERSION 1.3.0 LANGUAGES Fortran C)

If the project builds both standalone and as part of IFS, wrap configuration logic::

  if(NOT CMAKE_PROJECT_NAME STREQUAL "ifs")
    ecbuild_declare_project()
  endif()

Ecbuild Integration
-------------------
Find and require ecbuild early::

  find_package(ecbuild 3.4 REQUIRED)

Always use ecbuild macros (``ecbuild_add_library``, ``ecbuild_add_option``)
in preference to raw CMake equivalents.

---

Structure and Layout
====================

Top-Level CMakeLists.txt
------------------------
A well-structured top-level CMake file follows this order:

1. ``cmake_minimum_required`` and ``find_package(ecbuild)``
2. ``project()`` declaration
3. Feature toggles via ``ecbuild_add_option``
4. Compiler and flag setup
5. Package discovery
6. Build targets and subdirectories
7. Packaging and install directives
8. ``ecbuild_print_summary``

Example (ecRad-like structure)::

  cmake_minimum_required(VERSION 3.20 FATAL_ERROR)
  find_package(ecbuild 3.4 REQUIRED)
  project(ecrad LANGUAGES Fortran)

  ecbuild_add_option(FEATURE SINGLE_PRECISION DESCRIPTION "Build single precision variant" DEFAULT OFF)

  ecbuild_enable_fortran(REQUIRED MODULE_DIRECTORY ${PROJECT_BINARY_DIR}/module)

  add_subdirectory(src)
  add_subdirectory(tests)

  ecbuild_install_project(NAME ${PROJECT_NAME})
  ecbuild_print_summary()

---

Options and Feature Flags
=========================

Feature Declaration
-------------------
Declare optional components using ``ecbuild_add_option``::

  ecbuild_add_option(
    FEATURE USE_NETCDF
    DESCRIPTION "Enable NetCDF I/O"
    DEFAULT ON
    CONDITION NetCDF_FOUND
  )

Always provide a clear description and a logical default.

Conditional Logic
-----------------
- Avoid nested ``if()`` hierarchies.
- Use standard boolean variables like ``HAVE_<FEATURE>`` or ``ENABLE_<FEATURE>``.
- Keep feature logic close to where it is used.

---

Compiler and Build Settings
===========================

Fortran Flags
-------------
Enable module directories::

  ecbuild_enable_fortran(REQUIRED MODULE_DIRECTORY ${PROJECT_BINARY_DIR}/module)

Add compiler-specific options through ``ecbuild_add_fortran_flags()``::

  if(CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
    ecbuild_add_fortran_flags("-fPIC -ffree-line-length-none")
  endif()

Cross-Project Flag Propagation
------------------------------
IFS component repositories may inherit compiler flags from a parent build
using helper functions like ``ifs_propagate_flags(proj)``.
Encapsulate such logic in ``cmake/compile_flags.cmake`` to avoid duplication.

---

Targets and Directories
=======================

Target Declaration
------------------
Always use ecbuild helpers for defining libraries and executables::

  ecbuild_add_library(
    TARGET ecrad
    SOURCES src/*.F90
    PUBLIC_LIBS eckit
    PRIVATE_LIBS fiat
  )

For components providing both shared and static libraries, ensure conditional handling::

  if(BUILD_SHARED_LIBS)
    ecbuild_add_library(TARGET ecrad TYPE SHARED ...)
  else()
    ecbuild_add_library(TARGET ecrad TYPE STATIC ...)
  endif()

Fortran Module Management
-------------------------
Use the shared macro ``ifs_target_fortran_module_directory`` for consistent
Fortran module paths and installation.

---

Dependencies and Linking
========================

Package Discovery
-----------------
Locate dependencies consistently::

  ecbuild_find_package(NAME eccodes VERSION 2.0.0)
  ecbuild_find_package(NAME NetCDF COMPONENTS Fortran)

Linking
-------
Link libraries with explicit scope::

  target_link_libraries(ecrad PRIVATE NetCDF::NetCDF_Fortran PUBLIC eckit)

Handling Missing Dependencies
-----------------------------
Disable unavailable features with a warning::

  if(NOT eccodes_FOUND)
    ecbuild_warn("ecCodes not found — BUFR functionality disabled")
  endif()

---

Paths, Variables, and Conventions
=================================

Variable Naming
---------------
- Use UPPERCASE for global or exported variables (``ECRAD_LIBRARIES``).
- Use lowercase for temporary or local variables.
- Prefix variables with the project name to prevent conflicts.

Paths
-----
Use variable-based directories (``${PROJECT_SOURCE_DIR}``, ``${INSTALL_BIN_DIR}``)
and avoid hardcoded paths.

---

Custom Utilities
================

Adding Symlinks
---------------
Use ``add_symlink(target symlink)`` to create post-build links::

  add_symlink(ecrad_tool.x ecrad_tool)

File-Level Macros
-----------------
Use ``ifs_target_compile_definitions_FILENAME`` to embed filename metadata in builds.

Flag Propagation
----------------
Use ``ifs_propagate_flags`` to align component compiler settings with IFS.

---

Examples of Good Practice
=========================

Minimal Component Project
-------------------------
::

  cmake_minimum_required(VERSION 3.20 FATAL_ERROR)
  find_package(ecbuild 3.4 REQUIRED)
  project(ectrans LANGUAGES Fortran)

  ecbuild_enable_fortran(REQUIRED MODULE_DIRECTORY ${PROJECT_BINARY_DIR}/module)

  ecbuild_add_library(TARGET ectrans SOURCES src/*.F90 PUBLIC_LIBS eckit PRIVATE_LIBS fiat)

  ecbuild_install_project(NAME ectrans)
  ecbuild_print_summary()

Standalone or Embedded Build
----------------------------
A component may support both modes::

  if(CMAKE_PROJECT_NAME STREQUAL "ifs")
    message(STATUS "Building as part of IFS")
  else()
    ecbuild_declare_project()
  endif()

---

Evolution and Best Practice
===========================

This standard reflects current practice across IFS, ifsobs, ODB, and the
component repositories (ecRad, ecWAM, ecTrans).
Future evolution should aim to:

- Move fully to modern CMake idioms (``target_*`` over global flags)
- Limit compiler-specific conditionals to dedicated modules
- Provide consistent packaging and installation metadata across all components
- Maintain strict compatibility with ecbuild and CMake ≥ 3.20

---

**Document Status:** *Draft – unified standard for IFS and Component Repositories*
