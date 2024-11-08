ecFlow Suite Builders
======================

Overview
--------

ecFlow suites can be built in many ways, including by directly composing a [definition file](https://ecflow.readthedocs.io/en/latest/glossary.html#term-suite-definition)
or using ecFlow's Python API to generate suites programmatically. These methods can be combined with various tools and frameworks to facilitate the construction and management
of more complex workflows. This chapter introduces various tools and methodologies for building ecflow suites, emphasizing the importance of standardization and best practices.

Types of ecflow Suites
----------------------

1. **Native Python ecflow suites**: Directly written in Python.
2. **Python-generated .def files**: Suites generated with scripts using ecFlow's Python API.
3. **Native Python ecflow suites with interface layers**: Enhanced with additional layers like `ecf.py` or `comfies`.
4. **Pure pyFlow suites**: Object-oriented, Pythonic ecflow suites.
5. **pyFlow suites with interface layers**: Enhanced with tools like Pysuite, Wellies, and Tracksuite.

Tools for Suite Design
----------------------

- **pyFlow**: A high-level language for generating suites and scripts, promoting maintainability and best practices.
- **Wellies**: Provides extension tools for configuring and deploying pyFlow suites.
- **Tracksuite**: A deployment tool that tracks suite deployment in shared environment through Git.
- **Pysuites**: A framework for building IFS experiments.

Main features
-------------

The main aspects to be considered when building ecFlow suites with pyFlow are:

- **Object-Oriented Design**: Encapsulate behavior in Python classes, use inheritance and composition.
- **Configuration Management**: Use configurator objects to manage suite configurations.
- **Script Handling**: Maintain scripts with suites, avoid side-effects, and ensure scripts are testable in isolation.
- **Script Sources**: Use templated and composable scripts for flexibility and maintainability, but avoid excessive complexity.

Key Messages
------------

1. Treat suites as software; use pyFlow as a compiler.
2. Manage complexity with object-oriented design.
3. Parameterize using Python objects, not script conditionals.
4. Ensure scripts are testable in isolation.
5. Migrate sub-trees (AnchorFamilies) independently.
6. Separate deployment concerns from suite generation.

Resources
---------

- [pyFlow Documentation](https://pyFlow-workflow-generator.readthedocs.io/en/latest/content/introduction.html)
- [Wellies Documentation](https://pyFlow-wellies.readthedocs.io/latest/)
- [Tracksuite GitHub](https://github.com/ecmwf/tracksuite)
