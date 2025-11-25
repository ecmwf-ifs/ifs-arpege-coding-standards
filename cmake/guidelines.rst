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
  * `FIAT <https://github.com/ecmwf-ifs/fiat>`_ — I/O abstraction and data utilities
  * `OOPS <https://git.ecmwf.int/projects/OOPS/repos/oops>`_ — Object-Oriented Prediction System

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

- The **IFS master build** (`ifs-source/CMakeLists.txt`)
- IFS internal subprojects (e.g. `ifsobs`, `odb`, ...)
- Independent IFS component repositories (e.g. `ecrad`, `ecwam`, `ectrans`, `fiat`, `oops`)
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

Mixed-Language / Complex Component Repositories
-----------------------------------------------
Some repositories (e.g. **FIAT**, **OOPS**) contain both Fortran and C or C++ sources.
For such projects:

- Ensure all used languages are declared in ``project()``.
- Refer to the section :ref:`hardening-safety-flags` for guidance on compiler
  hardening flags, safe flag handling, and how to avoid modifying global CMake
  variables directly.
- Keep mixed-language structure clear by grouping source files and targets
  logically (e.g., C utilities in ``src/c/``, Fortran modules in ``src/fortran/``).

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

Features controlled by ``ecbuild_add_option`` can be enabled or disabled at
configure time by passing a corresponding CMake variable on the command line::

  cmake -DUSE_NETCDF=ON  path/to/source
  cmake -DUSE_NETCDF=OFF path/to/source

Each option creates two variables:

- ``USE_NETCDF`` – user-facing toggle
- ``HAVE_NETCDF`` – internal result after evaluating the ``CONDITION``

Only ``HAVE_<FEATURE>`` should be used to guard code or subdirectories.

Plugins and Optional Modules
----------------------------
Repositories such as **OOPS** define optional plugin modules or tools.
Follow this pattern:

- Declare one ``FEATURE`` per plugin or optional component.
- Wrap target definitions in feature guards::

    if(HAVE_PLUGIN_FOO)
      add_subdirectory(plugins/foo)
    endif()

- Ensure a clean dependency graph when plugins are disabled.
- Document each plugin’s purpose and dependencies in comments.

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


.. _hardening-safety-flags:

Hardening and Safety Flags
--------------------------

Compiler hardening, warning, and safety-related flags may originate from
three distinct layers of the IFS build environment:

Flag Sources and Precedence
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following layers determine the final compiler flags applied to a target:

.. code-block::

    ┌──────────────────────────────────────────────┐
    │  Platform Toolchain (ifs-bundle)             │
    │  * compiler selection                        │
    │  * MPI wrappers                              │
    │  * optimisation defaults                     │
    └──────────────────────────────────────────────┘
                     ↓
    ┌──────────────────────────────────────────────────────────┐
    │  IFS Master Flags (ifs-source/cmake/compile_flags.cmake) │
    │  * language-wide Fortran/C/C++ flags                     │
    │  * global warning levels                                 │
    │  * cross-component consistency                           │
    └──────────────────────────────────────────────────────────┘
                     ↓
    ┌──────────────────────────────────────────────────┐
    │  Component-Specific Flags (e.g. ecwam/fiat/oops) │
    │  * local warning policies                        │
    │  * experimental or safety-related options        │
    │  * project-maintained exceptions                 │
    └──────────────────────────────────────────────────┘
                     ↓
    target_compile_options()  (per-target adjustments)

General Guidelines
~~~~~~~~~~~~~~~~~~

- **Do not** modify the raw CMake variables (``CMAKE_C_FLAGS``,
  ``CMAKE_Fortran_FLAGS``, etc.), because:

  * they apply globally across the entire IFS bundle, and
  * they bypass ecbuild’s feature-testing and compiler compatibility checks.

- Use ``ecbuild_add_c_flags`` and ``ecbuild_add_fortran_flags`` when the
  intention is to extend the *project-wide* compile flags for a repository.

- Use ``target_compile_options()`` only when adjusting flags for a
  *specific* library or executable.

Project-Specific Flags Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Several component repositories legitimately carry their own flag modules,
typically to enforce safety, debugging, or warning behaviour specific to the
project. Examples include:

- ``ecwam/cmake/ecwam_compile_flags.cmake``
- ``fiat/cmake/fiat_compiler_warnings.cmake``
- ``oops/cmake/oops_compiler_flags.cmake``

These modules:

- Must clearly document the purpose of each flag
  (e.g. numerical precision, stack protection, strict C++ warnings).
- Should contain **only** flags relevant to that component, not flags intended
  for the entire IFS ecosystem.
- Are evaluated **after** toolchain and IFS master flags, meaning they may
  refine or override behaviour locally.

IFS Master Flags
~~~~~~~~~~~~~~~~

When building a component inside the full IFS build, the file

``ifs-source/cmake/compile_flags.cmake``

provides the canonical set of global build flags for consistency across the
entire system. Component repositories should not duplicate these settings.

Switchable Hardening Options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If strong hardening or warning levels are introduced (e.g.
``-fstack-protector-strong``, ``-Werror``), they must:

- be placed in the project's dedicated compile-flags module,
- be guarded by an option (e.g. ``ENABLE_STRICT_FLAGS``),
- be disabled by default unless the project mandates otherwise,
- document clearly the motivation (e.g. “FIAT requires strict C safety
  rules for low-level systems components”).


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

The utilities described in this section refer to helper macros provided by the
IFS build system. These are **not part of ecbuild**, and are **only available
when a component is built inside the IFS tree** (i.e. via ``add_subdirectory``).

External component repositories such as **ecRad**, **ecWAM**, **ecTrans**,
**FIAT**, and **OOPS** typically provide their own equivalents under their
``cmake/`` directory when required.

IFS-Provided Macros
-------------------

The following macros originate from ``ifs-source/cmake`` and are available only
when the project is built as part of IFS:

- ``ifs_target_fortran_module_directory`` — Set a consistent module output and
  install directory for a target.
- ``ifs_target_compile_definitions_FILENAME`` — Add a preprocessor definition
  containing the source filename.
- ``add_symlink`` — Create a post-build symbolic link to a target.

Standalone component repositories **must not rely on these macros**. If similar
behaviour is needed, they should provide their own project-specific versions
(e.g. ``ecwam_target_fortran_module_directory.cmake``,
``ectrans_target_fortran_module_directory.cmake``).


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

Installation and Packaging
==========================

Pkg-Config Support
------------------
The use of ``ecbuild_pkgconfig`` is **deprecated** for IFS components.
This functionality was historically used for integration with non-CMake build systems
but is no longer maintained or required; standard CMake exports handled by
``ecbuild_install_project`` are sufficient.

Dual Build Mode Support
-----------------------
Sub-components, i.e. projects built via ``add_subdirectory`` invocations rather than
via a separate bundle entry, should support both embedded and standalone builds::

  if(CMAKE_PROJECT_NAME STREQUAL "ifs")
    message(STATUS "Building ${PROJECT_NAME} as part of IFS")
  else()
    ecbuild_declare_project()
  endif()

Fully standalone projects, e.g. **ecWAM**, should not support embedded builds.

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

This standard reflects current practice across IFS, ifsobs, ODB, FIAT, OOPS,
and the component repositories (ecRad, ecWAM, ecTrans).
Future evolution should aim to:

- Move fully to modern CMake idioms (``target_*`` over global flags)
- Limit compiler-specific conditionals to dedicated modules
- Provide consistent packaging and installation metadata across all components
- Maintain strict compatibility with ecbuild and CMake ≥ 3.20
- Identify commonalities where new ecbuild macros may be shared across projects
