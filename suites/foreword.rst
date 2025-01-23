========
Foreword
========

At ECMWF we have a comprehensive list of computational workflows called *suites* that
produce both operational and research data. The design of these suites has evolved over
time to a high level of complexity to the workflow management system
(`ECFLow <https://ecflow.readthedocs.io>`_), incorporating a range of different styles and
methodologies. Many recommendations for best practices and standards exist, but mainly
only at working group level, thus serve mostly specific use cases and are not transferable
across ECMWF. Especially the different requirements for research and operational suites
presents challenges in maintaining a consistent approach.

- Research
    - Runs on historical dates, retrieved from archive or another experiment
    - Requires configurability and flexibility
    - On-the-fly debugging
    - Collaboration with a range of users

- Operations/production
    - Runs on latest date using observation retrievals
    - One fixed configuration
    - Locked down from outside interference
    - As efficient as possible
    - Finely tuned to a schedule
    - Features to ensure reliability and avoid or fix common problems
    - Clear documentation (that can be followed by stressed person at 3am)

Designing suites that are incompatible in either can lead to inefficiencies and
difficulties in collaboration, debugging, and transitioning between the two stages of the
suite lifecycle.

This document aims to provide a coherent set of guidelines for suite development across
ECMWF. They were developed by suite developers and users from across the centre to promote
a consistent, structured, and modern approach that is applicable across 
research, development, testing, and operational environments. They are expected to be
useful for new or external users as well as experienced ones alike. While these guidelines should 
serve as a solid foundation for improvements, it is anticipated that they will be further 
refined and expanded over time to adapt to evolving needs and applications.