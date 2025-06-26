Documentation
=============

Documenting the suite for different audiences
---------------------------------------------

For operators (1st line)
~~~~~~~~~~~~~~~~~~~~~~~~
The primary documentation for operators is found in the ecFlow manual pages. This documentation should include:

- Step-by-step instructions for common operational tasks.
- Troubleshooting guides for common issues.
- Contact information for 2nd line support - link to on-call page.

For analysts (2nd line)
~~~~~~~~~~~~~~~~~~~~~~~
Analysts will find relevant documentation in both the ecFlow manual pages and the repository README and code. This should
include:

- Detailed descriptions of the suite's functionality.
- Examples of typical analysis workflows.
- Troubleshooting guides for a more complex rerun and diagnosis of issues.
- Code snippets demonstrating how to interact with the suite programmatically.

For developers
~~~~~~~~~~~~~~
Developers should refer to the repository README and code comments for comprehensive documentation. This should cover:

- Code architecture and design principles.
- Guidelines for contributing to the codebase.
- Detailed comments within the code explaining complex logic and algorithms.

For scientists
~~~~~~~~~~~~~~
Scientists require a high-level description of the suite, which should be documented on a Confluence page. This should
include:

- An overview of the suite's purpose and capabilities.
- Descriptions of key scientific algorithms and models used.
- Links to relevant research papers and technical documentation.

Operators' perspective
----------------------
Operators who monitor and support operational suites around the clock cannot have a full
understanding of the operational components that are running. They have limited visibility on the tasks run and can only
consult higher-level dashboards and GUIs to issue warnings and call out relevant analysts for remedial action. Here's an example
linking delay alarms with a splunk dashboard:

.. figure:: _img/check_alarms.png
   :alt: ecFlow check alarms
   :align: center
   :width: 300px

   Check alarms implemented through cron jobs help to warn operators when a task is late. The statistics of runtimes can be
   either fixed times every day or be based on persistent runtime statistics kept on the HPC. 
   Combining with the "network traffic basic" plot in the following splunk dashboard allows operators to understand where the delay comes from.

.. figure:: _img/splunk_ecpds.png
   :alt: Splunk Web API
   :align: center
   :width: 600px

   `Splunk <https://www.splunk.com>`_ dashboards for the dissemination system health.

Task purpose, criticality, and failure procedures
-------------------------------------------------
Document what each task does, how critical it is and what to do when it fails. Each task within the suite should be
documented with the following information:

- **Task Name**: A clear and descriptive name.
- **Function**: A brief description of what the task does.
- **Criticality**: An assessment of how critical the task is to the overall suite.
- **Failure Procedures**: Detailed instructions on what to do if the task fails, including:

  - Common failure modes and their causes.
  - Steps to diagnose and resolve the issue.
  - Contact information for further assistance if needed.

Clear naming of families and tasks
----------------------------------
Families and tasks should be named clearly and descriptively to reflect their function. Naming conventions should be
consistent and follow these guidelines:

- **Families**: Use short, descriptive names that indicate the purpose of the family (e.g., `setup`, `admin`, `barrier`,
  `lag`).
- **Tasks**: Task names should clearly indicate their function without unnecessary abbreviation. Ensure that the
  granularity of tasks is appropriate, balancing runtime efficiency with clarity of purpose.
