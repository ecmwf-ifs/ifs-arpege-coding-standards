General principles
==================

What is a suite?
----------------
- **A definition of the suite structure including tasks and their dependencies:** This is captured in the ecFlow *.def*
  file, which is uploaded to an ecFlow server and displayed in the ecFlow UI.
- **Scripts and software to run the tasks:** Each task needs a *.ecf* script which is expanded by ecFlow and can call
  other shell scripts or executables.
- **Code to generate and deploy the definition and scripts:** This is typically a Python script based on
  :doc:`suite building packages <suite_construction>` which reads a configuration file and generates the ecFlow *.def*
  file and the *.ecf* scripts.

TL;DR recommendations
---------------------
- Keep it simple: Complexity should always be weighed against maintainability and clarity for other users.
- :doc:`Tasks and scripts <tasks>` should be self-contained without side effects.
- :doc:`Document <documentation>` tasks in the manual pages enough so operators understand their purpose.
- :doc:`For suite construction <suite_construction>` use configuration files where possible.
- Use version control for suite definitions, scripts, and configuration files.
