Documentation
=============

Documenting the suite for different audiences
---------------------------------------------

For operators (1st line)
~~~~~~~~~~~~~~~~~~~~~~~~
The primary documentation for operators is found in the ecFlow manual pages. This documentation should include:

- Step-by-step instructions for common operational tasks.
- Troubleshooting guides for common issues.
- Contact information for 2nd line support - link to oncall page.

For analysts (2nd line)
~~~~~~~~~~~~~~~~~~~~~~~
Analysts will find relevant documentation in both the ecFlow manual pages and the repository README and code.
This should include:

- Detailed descriptions of the suite's functionality.
- Examples of typical analysis workflows.
- Code snippets demonstrating how to interact with the suite programmatically.

For developers
~~~~~~~~~~~~~~
Developers should refer to the repository README and code comments for comprehensive documentation. This should cover:

- Code architecture and design principles.
- Guidelines for contributing to the codebase.
- Detailed comments within the code explaining complex logic and algorithms.

For scientists
~~~~~~~~~~~~~~
Scientists require a high-level description of the suite, which should be documented on a Confluence page.
This should include:

- An overview of the suite's purpose and capabilities.
- Descriptions of key scientific algorithms and models used.
- Links to relevant research papers and technical documentation.

Operators perspective
---------------------
In this section we give a view on all the pages and systems monitored by operators 24/7 as documented on https://confluence.ecmwf.int/display/PS/Suites+Monitored+by+Operators

.. figure:: _img/ecflow.png
   :alt: ecFlow Interface
   :align: center
   :target: https://confluence.ecmwf.int/display/PS/Suites+Monitored+by+Operators
   :width: 50%

   Overview of the ecFlow interface that operators use to monitor and control suite tasks. This is the main interface of interaction during cycle runs 4x a day.

.. figure:: _img/ssh.png
   :alt: ecFlow Interface for SSH accesses
   :align: center
   :target: https://confluence.ecmwf.int/display/PS/Suites+Monitored+by+Operators
   :width: 200%

   Overview of the ecFlow interface that operators use to monitor and control suite ssh tasks in different categories of jobs.

.. figure:: _img/hpc_users.png
   :alt: HPC Users Monitor
   :align: center
   :target: https://atos-stats.ecmwf.int:3000/d/AlbyPPf4z/hpc-users?orgId=1&from=now-6h&to=now&timezone=utc&var-username=.%2A
   :width: 200%

   Displays real-time usage statistics for HPC users. Allows operators to catch users who are (accidentally) draining resources and endangering operations.

.. figure:: _img/hpc_elephant.png
   :alt: HPC Elephant View
   :align: center
   :target: https://atos-stats.ecmwf.int:3000/d/fdvdmcsmizxtsd/elephant?orgId=1&from=now%2Fd&to=now%2F1d&timezone=utc&var-clusters=aa&var-partition=all&refresh=1m
   :width: 200%

   Illustrates resource usage across HPC nodes (the so-called “Elephant” view) with a focus on the two operational clusters that contain the time-critical runs of the data assimilation, IFS forecast and dissemination.

.. figure:: _img/hpc_node_usage.png
   :alt: HPC Node Usage
   :align: center
   :target: https://atos-stats.ecmwf.int:3000/d/e13ab66a-5663-49b5-af7d-cf307a707aa0/node-usage?orgId=1&from=now-6h&to=now&timezone=utc&var-partition=par&refresh=1m
   :width: 200%

   Shows detailed CPU, memory, and job distribution on HPC nodes.

.. figure:: _img/hpss_gui.png
   :alt: HPSS GUI
   :align: center
   :target: https://confluence.ecmwf.int/display/SHIFT/Starting+HPSSGUI+and+HPSSMON
   :width: 50%

   Graphical interface for the HPSS (High Performance Storage System).

.. figure:: _img/hpss.png
   :alt: HPSS Monitor
   :align: center
   :target: http://hpssmon.ecmwf.int:5001
   :width: 300%

   Monitoring tool for ongoing HPSS (High Performance Storage System) transfers and storage usage.

.. figure:: _img/acq_monitor.png
   :alt: Acquisition Monitor
   :align: center
   :target: https://acq-monitor.ecmwf.int/do/monitoring
   :width: 200%

   Monitors the acq ECPDS server used for SAPP (observation data acquisition).

.. figure:: _img/diss_monitor.png
   :alt: DISS Monitor
   :align: center
   :target: https://diss-monitor.ecmwf.int/do/monitoring
   :width: 200%

   Monitors the diss ECPDS server used for the main dissemination.

.. figure:: _img/aux_monitor.png
   :alt: Auxiliary Monitor
   :align: center
   :target: https://aux-monitor.ecmwf.int/do/monitoring
   :width: 200%

   Monitors the aux ECPDS server used for opendata and CAMS.

.. figure:: _img/xdiss_monitor.png
   :alt: XDIS Monitor
   :align: center
   :target: https://xdiss-monitor.ecmwf.int/do/monitoring
   :width: 200%

   Specialized view for monitoring XDIS processes.

.. figure:: _img/service_status.png
   :alt: Service Status
   :align: center
   :target: https://status.ecmwf.int
   :width: 200%

   High-level dashboard for various service statuses.

.. figure:: _img/opsview.png
   :alt: Opsview Monitoring
   :align: center
   :target: https://opsview.ecmwf.int
   :width: 200%

   Comprehensive monitoring tool for all operational services and infrastructure with a simple color-coded health status.

.. figure:: _img/infoboard.png
   :alt: Service Status
   :align: center
   :target: https://infoboard.ecmwf.int
   :width: 200%

   Infoboard with announcements of system sessions and potential service degradations.

.. figure:: _img/service_catalogue.png
   :alt: Service Status
   :align: center
   :target: https://sites.ecmwf.int/services/catalogue/
   :width: 200%

   New interface portal which will serve as gateway to all other services' monitoring in the near future.

.. figure:: _img/jira.png
   :alt: Jira Integration
   :align: center
   :target: https://jira.ecmwf.int/secure/Dashboard.jspa?selectPageId=12722
   :width: 200%

   Shows JIRA tickets from users of importance to the shift teams.

.. figure:: _img/confluence.png
   :alt: Confluence Documentation
   :align: center
   :target: https://confluence.ecmwf.int/display/SHIFT/Dissemination+Products+new
   :width: 200%

   Confluence page references for detailed suite documentation.

.. figure:: _img/eccharts.png
   :alt: ecCharts Interface
   :align: center
   :target: https://eccharts.ecmwf.int
   :width: 150%

   The ecCharts service.

.. figure:: _img/preprocess_jobs.png
   :alt: Preprocess Jobs
   :align: center
   :target: https://apps.ecmwf.int/data-layer/preprocess?status=aborted&action=all
   :width: 150%

   Shows the flow and status of preprocess jobs.

.. figure:: _img/eccmd.png
   :alt: ecmwf ec batch Jobs
   :align: center
   :target: http://boleccmd.ecmwf.int:8090
   :width: 200%

   Shows the monitoring of all batch jobs of the ecaccess interface for Member States and other users.

.. figure:: _img/mars_web_api.png
   :alt: MARS Web API
   :align: center
   :target: https://apps.ecmwf.int/webapi-activity/
   :width: 200%

   Interface for interacting with MARS via the Web API.

.. figure:: _img/mars_activity.png
   :alt: MARS Activity
   :align: center
   :target: https://apps.ecmwf.int/mars-activity
   :width: 150%

   Real-time overview of MARS data retrieval and archiving activity.

.. figure:: _img/splunk_webapi.png
   :alt: webMARS API splunk dashboard
   :align: center
   :target: https://splunk.ecmwf.int/en-US/app/ecmwf_mars/web_api_now?form.system=bol-webmars-private-prod
   :width: 200%

   Splunk dashboards for web MARS API.

.. figure:: _img/mars_web_software.png
   :alt: MARS Web API
   :align: center
   :target: https://bol-monitoring.ecmwf.int/monitoring/#!/hashtags/detail?h=va-webmars-service
   :width: 200%

   Application level monitoring of MARS services.

.. figure:: _img/mars_web_hardware.png
   :alt: MARS Web API
   :align: center
   :target: https://bol-monitoring.ecmwf.int/monitoring/#!/hashtags/detail?h=va-webmars-infra
   :width: 200%

   Hardware level monitoring of MARS services.

.. figure:: _img/marsadm.png
   :alt: MARS Administration
   :align: center
   :width: 200%

   Interface for monitoring MARS tasks running on each MARS instance, i.e. od, rd, er, th, ms and sc.

.. figure:: _img/ecfsadm.png
   :alt: ECFS Administration
   :align: center
   :width: 400%

   Interface for monitoring ECFS (ECMWF File Storage) tasks.

.. figure:: _img/splunk_ecpds.png
   :alt: Splunk Web API
   :align: center
   :target: https://ecpds-metrics.ecmwf.int/d/rYdddlPWk2/hosts?orgId=1&from=now-24h&to=now&timezone=browser&var-datasource=PBFA97CFB590B2093&var-job=node_exporter&var-service=$__all&var-component=$__all&var-node=bodh1ecpdmv-04:9100&var-diskdevices=%5Ba-z%5D%2B%7Cnvme%5B0-9%5D%2Bn%5B0-9%5D%2B%7Cmmcblk%5B0-9%5D%2B&refresh=1h
   :width: 1000%

   Splunk dashboards for the full ECPDS system health.

.. figure:: _img/necj.png
   :alt: NECJ Monitor
   :align: center
   :target: https://confluence.ecmwf.int/display/SHIFT/ATOS+-+necj+%3A+new+tool
   :width: 200%

   Specialized monitoring of jobs running on the HPC and their runtime deviations from stored statistical averages.

.. figure:: _img/open_nms.png
   :alt: HPC Open NMS
   :align: center
   :target: http://hpc-opennms.ecmwf.int/
   :width: 200%

   Monitoring of the network's component health.

Task purpose, criticality, and failure procedures
-------------------------------------------------
Document what each task does, how critical it is and what to do when it fails. Each task
within the suite should be documented with the following information:

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

- **Families**: Use short, descriptive names that indicate the purpose of the family (e.g., `setup`, `admin`, `barrier`, `lag`).
- **Tasks**: Task names should clearly indicate their function without unnecessary abbreviation.
  Ensure that the granularity of tasks is appropriate, balancing runtime efficiency with clarity of purpose.