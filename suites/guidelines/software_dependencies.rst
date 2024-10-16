Software dependencies
==================

Software dependencies is an important part of a suite design as it drives the reproducibility of the suite.
If the software dependencies are not managed properly, it becomes very difficult to reproduce previous suite runs or to test new versions of the software.
Software versions should be explicit. This means that the software version should be specified in the suite configuration and not rely on the default version of the software.
Making the version explicit also allows to test the suite with different versions of the software, allowing pre-operational test of new software versions.

A suite usually interact with software in two ways:
    - deploying the software in directory local to the suite or to a shared location
    - load the environment containing the software within the suite

Software deployment
----------------

The software deployment is ususally done in a "setup" or "init" family of the suite, which is ran only once at the beginning of the suite.
The deployment should be driven by the suite configuration.
The version and the location of the software should be specified in the suite configuration to allow reproducibility and testing of different versions. 
If possible, the configuration should allow different ways of deploying the software (conda environment, module, etc.).

For operational suites, local deployment of the software should be avoided as much as possible and the deployment and testing of the software should be done by the development team in a shared location.
This allows to test the software in a shared environment but also avoids duplication of the software on the system.
It also improves reproducibility as the suite could replace previous software installations by the latest ones, while deployment of software in shared locations usually gives access to multiple versions of the sotware at once.

Finally the suite can be used to deploy the software to a shared environment. In this case, the software should first be built and tested locally in a separate task and only deployed to the shared location if the tests are successful.

Loading the software environment
----------------

As for the software deployment, loading the software environment within the ecflow task should be driven by the suite configuration.
Different versions and types of environments (module, conda, python venv, etc.) should be supported by the suite configuration.
Loading the environment should be straightforward software dependencies should be loaded automatically. Complex setting of the environment should be avoided, such as setting environment variables or modifying the PATH.


To avoid
----------------

Compiling or building software within the tasks where it runs should be avoided. This makes the suite less reproducible and makes it very difficult to test the software in isolation.
If the software needs to be compiled, it should be done in the "setup" or "init" family of the suite, and loading the environment containing the software should be straightforward.
