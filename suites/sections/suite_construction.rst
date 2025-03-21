Suite construction
==================

ecFlow suites can be built in many ways, including by directly composing a 
:ecflow-docs:`definition file <glossary.html#term-suite-definition>`
or using ecFlow's Python API to generate suites programmatically. 

These methods can be combined with various tools and frameworks to facilitate the construction and 
management of more complex workflows. This chapter will present what to consider when using one of these 
tools to build more standardised and maintainable suites.

`ecFlow` workflows can be defined in a very generic way. While the structure and dependencies across nodes are all well-defined and 
must be managed in an :doc:`appropriate way <structure>`, the application complexity can be hidden in the scripts and headers
used in each one of the tasks. For simple workflows, just a handfull of files would be enough to define the whole suite. However, as the
the application grows, the use of tools and frameworks built on top of the base ecFlow API help make the general project more maintainable.

Introducing pyflow
------------------

Along the many years of operational use of the `ecFlow` framework, several tools and frameworks have been developed to facilitate the construction of suites.
For new developments, :pyflow-docs:`pyflow <content/introduction.html>` is the recommended 
framework to follow and it is the one that will be presented in this chapter. 
`pyflow` provides an extensive documentation with many examples and best practices recommendations and it is, therefore, a good starting point for 
those not familiar with the framework. This document is not a replacement for the official documentation, but rather a summary of the key points to consider when 
building ecFlow suites.

When using a suite building framework, it's important to treat it as a development project. This generally means that:

- The suite should be version controlled, tested, and documented.
- The suite should be treated as a software project, with the suite building framework as a compiler to generate both the definition file and scripts.
- The suite should be developed in a modular way, with each part of the suite being testable in isolation where possible. 

Main principles
---------------

More specifically, `pyflow` encourages some key principles to follow when building suites:

- **Object-Oriented Design**: Encapsulate behavior in Python classes, use inheritance and composition. :pyflow-docs:`Examples <content/introductory-course/object-oriented-suites.html>` and :pyflow-docs:`here <content/introductory-course/additional-examples.html>`

.. code-block:: python

    with pyflow.Suite("suite"):
        DeploymentFamily(config)
        with pyflow.Family("tests", FLAG=123) as f:
            (
                RunUnitTests(config)
                >>
                IntegrationTests(config, name="localhost", host="localhost")
                >>
                IntegrationTests(config, name="cloudserver", host="cloudserver")
            )

Classes can inherit from base classes reusing the base structure and common functionality while overriding context specific ones.

.. code-block:: python

    class BaseTest(pyflow.AnchorFamily):
        def __init__(self, name, **kwargs):
            super().__init__(name, **kwargs)
            with self:
                (
                    Initialisation()
                    >>
                    self.build_test()
                    >>
                    Cleanup()
                )

    class CustomTest(BaseTest):
        def build_test(self):
            return pyflow.Task("run_test", script="run_test.sh")


- **Configuration Management**: Use configurator objects to manage different suite deployments. Parameterize using Python objects, not script conditionals. `Examples <https://pyflow-workflow-generator.readthedocs.io/en/latest/content/introductory-course/configuring-suites.html>`_
- **Dettached Deployment**: Separate deployment concerns, where the workflow will be managed and executed, from suite generation. `See tracksuite <https://github.com/ecmwf/tracksuite>`_

Script Handling
---------------

There are specific recommendations for writing `:ifs-standards-docs: scripts <shell/guidelines/ecflow/structure.html>` and managing :ref:`tasks`, but the following are some general principles to follow when using `pyflow`:

- **Script Handling**: Maintain scripts with suites, avoid side-effects, and ensure scripts are testable in isolation. `See more <https://pyflow-workflow-generator.readthedocs.io/en/latest/content/introductory-course/script-handling.html>`_
- **Script Sources**: Use templated and composable scripts for flexibility and maintainability, but avoid excessive complexity. Body may be composed of snippets assembled together by `pyflow`.
- **Concise Deployment**: Use `AnchorFamily` only where necessary to avoid creating complex deployed file trees. [#f2]_
- **ecFlow variables**: Use shell syntax for appropriate defaults. Define variables at the top most node level as possible to avoid redefinition of variables. [#f1]_

Extra resources
---------------

Here are some other references for other suite building tools that are available in the `ecFlow` ecosystem:

    - :wellies-docs:`pyflow-wellies`: A library of common patterns and utilities for `pyflow` including YAML-based configuration, 
      commonly used script snippets, and execution environment management.
    
    - `tracksuite <https://github.com/ecmwf/tracksuite>`_: A tool for git-based deployment of `ecFlow` suites, including support to multi-user 
      environments, remote deployment and cloud-based backup of deployments.
    
    - `pySuite <https://confluence.ecmwf.int/display/IFS/pysuite%3A+IFS+suite+definitions+in+Python>`_: A modular object-oriented framework for generating IFS suites, built on pyFlow in a limited way. 

.. rubric:: Footnotes

.. [#f1] `pyflow` enforces the use of shell variables in the body of scripts and has its own inspection step to create appropriate environment variables.
.. [#f2] `pyflow` enforces uniqueness of task names and script-to-task mapping. The library's `AnchorFamily` class creates a new "root" for the script file tree, allowing scripts with same name to co-exist.
