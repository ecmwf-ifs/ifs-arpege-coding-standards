Families
--------

- families are used to structure the tasks of the suite
- families can be used to give tasks a visual and execution hierarchy
- use descriptive but short names
- there are several standard families most suites should have, such as:
    - setup: tasks to install the suite's software and data dependencies
    - admin: tasks for suite maintainance and other manually run tasks or toggles
    - barrier: a family to hold the next execution date for operational suites
    - lag: tasks that lag behind time-critical tasks, such as archiving and cleaning
