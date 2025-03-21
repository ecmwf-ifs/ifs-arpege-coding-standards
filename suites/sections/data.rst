Data
====

Intro paragraph
How we handle data within suite, in tasks and between tasks

Data within and between tasks
-----------------------------
Most tasks read and write data of various formats and sizes. Tasks need to be designed to
handle this input and output data efficiently and reliably, both within and between tasks.
There are best practices for data handling to ensure this within a suite:

- Ensure input data is available on a fast and reliable filesystem, e.g. in a previous
  data retrieval task.
- Fast filestems for task output, such as a temporary or scratch space.
- Consider data formats, data chunking and data compression when designing a task.
- Clean working directories before you start the task, as a previous run or previous date
  cycle could have left files behind.

  - Allows time between runs to debug problems or rerun tasks
  - Ensure important outputs are archived

- Avoid accessing off-line data (e.g. MARS and ECFS) in time critical path

  - Store frequently-accessed invariant data on disk and document expected structure
  - Fetch and archive in parallel with the main modelling/processing work

- Back up critical data that is costly to recreate and keep it synchronized with the suite

  - Treat data as a dependency of the suite and use some form of version control or snapshots for reproducibility.


Retrieving data from remote services, databases and archives
------------------------------------------------------------
- should not be in critical path
- MARS
    - User Documentation: https://confluence.ecmwf.int/display/UDOC/MARS+user+documentation
    - examples: https://confluence.ecmwf.int/display/UDOC/MARS+example+requests
    - writing a request: https://confluence.ecmwf.int/display/MARS/Guidelines+for+writing+MARS+requests+in+suite+scripts
- FDB
    - Documentation and examples: https://confluence.ecmwf.int/display/FDB/Home
- ECFS
    - User Documentation: https://confluence.ecmwf.int/display/UDOC/ECFS+user+documentation
- ECPDS
    - ECMWF Production Data Store (ECPDS): https://confluence.ecmwf.int/pages/viewpage.action?pageId=118831332


Archiving data
--------------
- should not be in critical path
- efficiency of ecfs and mars can impact suite design/structure
- links to mars and ecfs best practices
    - https://confluence.ecmwf.int/display/UDOC/Operational+Data%3A+Guidelines+to+write+efficient+MARS+requests
- MARS archiving
    - 
- ECFS archiving
- FDB - Fields Database
    - FDB procedures: https://confluence.ecmwf.int/display/FAB/FDB+Procedures
    - Github: https://github.com/ecmwf/fdb
    - FDB service: https://confluence.ecmwf.int/display/FAB/FDB+service


Dissemination
-------------
Needs to be in critical path

- dissemination
    - ECPDS - https://aux-monitor.ecmwf.int/do/login
    - https://confluence.ecmwf.int/display/~maar/Create+new+stream+and+user+in+aux-monitor.ecmwf.int
    - FTP - https://confluence.ecmwf.int/display/UDOC/FTP+Service+-+Internal+users+to+provide+files+for+external+access#FTPServiceInternaluserstoprovidefilesforexternalaccess-HowdoImakeuseoftheFTPserviceinBologna?

