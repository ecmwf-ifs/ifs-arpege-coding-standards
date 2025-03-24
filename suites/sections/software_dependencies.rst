Software dependencies
=====================

Software dependencies are an important part of the suite design as they drive the reproducibility of the suite. If software 
dependencies are not managed properly, it becomes very difficult to reproduce previous suite runs or test new versions of the 
software. Software versions should be explicit, meaning the version should be specified in the suite configuration rather than 
relying on the software’s default version. Making the version explicit also allows testing the suite with different software 
versions, facilitating pre-operational testing of new software versions.

A suite usually interacts with software in two ways:
    - Deploying the software in a directory local to the suite or in a shared location (in case the suite acts as a software 
      deployer)
    - Loading the environment containing the software within the suite

Software deployment
-------------------

Software deployment is usually handled in a "setup" or "init" family of the suite, which runs only once at the beginning of the 
suite. Deployment should be driven by the suite configuration. The version and location of the software should be specified in 
the suite configuration to enable reproducibility and testing of different versions. If possible, the configuration should 
support various deployment methods, such as conda environments, modules, etc.

For operational suites, local deployment of the software should be minimised. The development team should handle deployment and 
testing of the software in a shared location. This approach allows testing in a shared environment, reduces software duplication 
on the system, and improves reproducibility by making multiple software versions available simultaneously.

Additionally, the suite can be used to deploy the software to a shared environment. In this case, the software should first be 
built and tested locally in a separate task and deployed to the shared location only if the tests are successful.

Note that if the suite can run on multiple hosts, the software should be deployed on all the hosts.

Loading the software environment
--------------------------------

As with software deployment, loading the software environment within the ecFlow task should be driven by the suite 
configuration. The suite configuration should support different environment versions and types (e.g., modules, conda 
environments, Python virtual environments).

Loading the environment should be straightforward, and software dependencies should be loaded automatically. Complex environment 
setups, such as setting numerous environment variables or modifying the PATH, should be avoided.

Best practices for software compilation
---------------------------------------

Avoid compiling or building software within the tasks where it runs. This reduces the suite’s reproducibility and makes isolated 
testing of the software challenging. If software compilation is necessary, it should be done within the "setup" or "init" family 
of the suite. Loading the environment containing the software should then be a straightforward process.

Software versions should not be hardcoded in the task scripts but should be loaded from the suite configuration. This allows 
testing different software versions from the same code without modifying the task scripts.

ECMWF software packages
-----------------------
ECMWF has a number of software packages that can be used to retrieve, process, and disseminate data. When working with ecFlow 
suites, netCDF data, GRIB files, BUFR, etc., it is recommended to use native tools such as ecCodes, earthkit, pyflow, anemoi, 
Metview, and others. Some of the advantages of using ECMWF software packages are:

- They are more reliable when dealing with ECMWF products;
- Most of these packages are maintained by ECMWF and have a good support system;
- They are optimized to work with ECMWF data and products;
- There are a number of examples and tutorials on how to use these packages, which can be found on CDS or the ECMWF website;
