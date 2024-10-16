External dependencies
---------------------

Barrier task
- A central place to wait for external dependencies, such as:
    - ECMWF NWP: ensemble, control (e.g mc, o, eda suites);
    - input from other Centres: DWD, COSMOS, etc.;
    - observations, data assimilation, ERA5.
- Triggers:
    - could be triggered from another suite and/or in a specific time or day (i.e. when data are expected);
    - usually runs within a time range to check for the availability of the data and/or the completion of a task;
    - can also be used to check the availability of data on disk.
- Events:
    - can set a "no data" or "data ok", which will have different instructions in the manual pange.
- Warnings:
    - can be added to the task as "late_alert", "missing_data", etc. with instructions on what to do in case the data is not available at a certain time;
    - these warning can trigger (e.g. set complete) other parts of the suite in case the data isn't available. 

- YMD to be set correctly to the first day of the experiment and used consistently throughout suite, with the right intervals.

Mirrors
- Defined as a tool for one suite to "see" other suites it depends on. 
- It can be implemented at a server level (local, operational, test), so users can follow other suites they need for their experiments.
- https://confluence.ecmwf.int/display/~map/A+simple+mirror+example
- https://confluence.ecmwf.int/display/~maes/Ecflow