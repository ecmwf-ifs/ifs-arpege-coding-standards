Data management
===============

Effective data management is crucial for the performance and reliability of any
computational suite. This section outlines best practices for handling data within tasks,
between tasks, and across the entire suite. Proper data management ensures that tasks run
efficiently, data integrity is maintained, and critical outputs are archived and retrievable.

Data within and between tasks
-----------------------------
Most tasks read and write data of various formats and sizes. Tasks need to be designed to
handle this input and output data efficiently and reliably.
These best practices for data handling have been established at ECMWF to achieve this:

- Ensure input data is available on a fast and reliable filesystem, e.g. in a previous
  data retrieval task.
- Fast filestems for task output, such as a temporary or scratch space.
- Consider data formats, data chunking and data compression when designing a task.
- Clean working directories before you start the task, as a previous run or previous date
  cycle could have left files behind.
- Store frequently-accessed invariant data (static data) on disk or other highly available data storage and document its expected structure in the task or suite documentation, ideally using a consistent naming convention.
- Treat static data as a dependency of the suite and use some form of version control or snapshots for reproducibility.


Retrieving data from remote services, databases and archives
------------------------------------------------------------
Data retrieval tasks are common bottlenecks and points of failure in suites and therefore
require careful design. Time-consuming retrievals can slow down suites significantly, if not
implemented efficiently. They should be kept out of the critical path of the suite unless
they are :doc:`time-critical <time_criticality>` themselves. Often this is done by keeping
them in a separate repeat loop family to allow them to run in parallel with the main
loop. Effective data chunking is key and should be tailored to the data retrieval service
used.

Failure-prone retrievals must handle and document errors adequately
to ensure either the suite itself or the operator can recover from them. Giving concise
instructions to the user or operator for the common retrieval failures in the
:ecflow-docs:`manual page <glossary.html#term-manual-page>` is essential for these tasks.

Dissemination
-------------
Dissemination tasks have to be on the critical path of the suite to ensure timely delivery
of the suite outputs. They should be designed to be as efficient as possible and to
minimize the risk of failure. These tasks should have their `defstatus` set by an admin
event toggle and switched off by default. This ensures dissemination is only switched in
the operational suite when needed and skipped in testing.

Archiving data
--------------
Ensure the main outputs of a suite are archived properly, i.e. stored on reliable long-term
storage rather than expensive, fast file systems. As archival often is a slow, serial process
keep the tasks outside of the critical path of the suite. Similar to data retrieval tasks,
they should be implemented on separate repeat loops (the :ref:`lag family <standard families>`)
to allow them to run independently of the main loop. Once archived, data should be cleaned on disk if it is no longer required
for the rest of the suite. It is often useful to keep this data available in operations to
recover from failures or to rerun parts of the suite. This should be enabled with appropriate
to clean this data.


ECMWF archival and dissemination services
-----------------------------------------
At ECMWF, the following services are commonly used for data retrieval and archival. They
have their own best practices and guidelines (linked below) which should be followed when
using them in a suite:

- the **Meteorological Archival and Retrieval System (MARS)** is the main data archive at ECMWF

  - :ecmwf-confluence:`general user documentation <x/Ax66Ag>`
  - :ecmwf-confluence:`the MARS retrieve action  <x/Ax66Ag>`
  - :ecmwf-confluence:`guidlines for writing efficent request in suite scripts <x/1hN-AQ>`

- the **Fields Database (FDB)** is a faster database for the most common and recent meteorological fields

  - :ecmwf-confluence:`documentation and examples <x/5CZ-AQ>`

- **ECMWF's File Storage system (ECFS)** is a file storage system for large volumes of unstructured data

  - :ecmwf-confluence:`general documentation <x/tQijAg>`

- **ECMWF Production Data Store (ECPDS)** is the main data dissemination service at ECMWF

  - :ecmwf-confluence:`general documentation <x/5DgVBw>`
