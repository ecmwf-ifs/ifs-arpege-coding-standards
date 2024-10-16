Documentation
-------------

Documenting the Suite for Different Audiences
---------------------------------------------

For Operators (1st Line)
~~~~~~~~~~~~~~~~~~~~~~~~
The primary documentation for operators is found in the ecFlow manual pages. This documentation should include:
- Step-by-step instructions for common operational tasks.
- Troubleshooting guides for common issues.
- Contact information for 2nd line support - link to oncall page.

For Analysts (2nd Line)
~~~~~~~~~~~~~~~~~~~~~~~
Analysts will find relevant documentation in both the ecFlow manual pages and the repository README and code. This should include:
- Detailed descriptions of the suite's functionality.
- Examples of typical analysis workflows.
- Code snippets demonstrating how to interact with the suite programmatically.

For Developers
~~~~~~~~~~~~~~
Developers should refer to the repository README and code comments for comprehensive documentation. This should cover:
- Code architecture and design principles.
- Guidelines for contributing to the codebase.
- Detailed comments within the code explaining complex logic and algorithms.

For Scientists
~~~~~~~~~~~~~~
Scientists require a high-level description of the suite, which should be documented on a Confluence page. This should include:
- An overview of the suite's purpose and capabilities.
- Descriptions of key scientific algorithms and models used.
- Links to relevant research papers and technical documentation.

Operators Perspective
---------------------
.. image:: path/to/screenshot.png
   :alt: Screenshot of Operator's Screen
   :align: center


Document What Each Task Does, How Critical It Is, and What to Do When It Fails
------------------------------------------------------------------------------
Each task within the suite should be documented with the following information:
- **Task Name**: A clear and descriptive name.
- **Function**: A brief description of what the task does.
- **Criticality**: An assessment of how critical the task is to the overall suite.
- **Failure Procedures**: Detailed instructions on what to do if the task fails, including:
  - Common failure modes and their causes.
  - Steps to diagnose and resolve the issue.
  - Contact information for further assistance if needed.

Clear Naming of Families and Tasks
----------------------------------
Families and tasks should be named clearly and descriptively to reflect their function. Naming conventions should be consistent and follow these guidelines:
- **Families**: Use short, descriptive names that indicate the purpose of the family (e.g., `setup`, `admin`, `barrier`, `lag`).
- **Tasks**: Task names should clearly indicate their function without unnecessary abbreviation. Ensure that the granularity of tasks is appropriate, balancing runtime efficiency with clarity of purpose.