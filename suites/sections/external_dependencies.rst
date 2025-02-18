External dependencies
=====================

Barrier
-------

The barrier family central place to wait for external dependencies, such as:

    - ECMWF NWP: ensemble, control (e.g mc, o, eda suites);
    - input from other Centres: DWD, COSMOS, etc.;
    - observations (BUFR, radar, etc.), data assimilation, ERA5 (https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation).

Triggers
--------
    - could be triggered from another suite and/or in a specific time or day (i.e. when data are expected);
    - usually runs within a time range to check for the availability of the data and/or the completion of a task;
    - can also be used to check the availability of data on disk.

Events
------
    - can set a "no data" or "data ok" event, which will have different instructions in the manual pange.

Warnings
--------
    - can be added to the task as "late_alert", "missing_data", etc. with instructions on what to do in case the data is not available at a certain time;
    - these warning can trigger (e.g. set complete) other parts of the suite in case the data isn't available. 

YMD
--- 
    - reference date for the family/task/group of families. 
    - different parts of the suite can have different YMDs (e.g. lag can be one day behind main)
    - different forecasts will have different intervals (e.g. reforecasts, seasonal, etc.)
    - to be set correctly to the first day of the experiment and used consistently throughout suite, with the right intervals.

Mirrors
-------
    - Can be defined as a reflection of a suite on another server;
    - The idea is that a server can "see" what is happening in another server so the suites in that first serve can progess.
    - It can be implemented at the server level (local, operational, test), so users can follow other suites they need for their experiments;
    - example: https://confluence.ecmwf.int/display/~map/A+simple+mirror+example

ECMWF software packages
-----------------------
    - https://confluence.ecmwf.int/display/UDOC/ECMWF+software+packages+-+FAQs
    - Integrate ECMWF packages to your suite as they are more reliable when dealing with ECMWF products, grib files, BUFR, etc.
