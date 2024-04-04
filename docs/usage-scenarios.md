Application Package Usage Scenarios

The application package provides a well-defined set of procedures to allow “build to run” operations. 

It covers five different usage scenarios from application testing, validation, and deployment to execution in production that enables:

* Alice to package an application
* Bob to script the execution of application
* Eric to deploy an application on platform Z
* Platform Z to accept the deployment of a new process
* Platform Z to execute a process with specific parameters

The scenarios cover the three “build to run” operations: Build, Deploy and Run.

## Build

Alice builds a container image with her Application and command line tool(s) and respective runtime environments, publishes the container image on a repository and writes the Application Package document with a workflow that invokes the command line tool(s) included in the image.

For Alice, the Application Package is a portable and executable document that:

* References the container image
* Describes the input parameters of the Application
* Maps the input parameters to the command line tool(s) arguments

Bob wants to run Alice’s application and scripts the application with different input parameters in his local machine. He uses two tools to script the execution: a container engine (e.g. Docker) and a CWL runner. The container image used is a controlled execution environment and it’s the same used by Alice to build and test the application.

For Bob, the Application Package is a portable and executable document that:

* Is used by the CWL runner to mount the volumes for the inputs and outputs for the container run
* Is used by the CWL runner to invoke the CLI in a specified way with parameters passed as YAML.
* Isolates the process with no environments configured on Bob’s machine

## Deploy

Eric wants to deploy Alice’s Application Package as a new Processing Service in Platform Z where he has authorization rights.

He uses the Application Package document to create an OGC API — Processes Transaction Extension request to dynamically add a process to a deployed OGC API — Processes server instance.

For Eric, the Application Package is a portable and executable document that:

* Maps the application input parameters to OGC API — Processes Input Parameters
* Identifies the container image to be deployed and corresponding execution unit
* Registers an application in a platform as a process within an OGC API — Processes server instance

Platform Z receives an _OGC API — Processes deployment_ from Eric. The platform uses the Application Package CWL to create a new process in the _OGC API — Processes_ server instance. 

For the Platform Z, the Application Package is a document that:

* Defines the process metadata (including the Input Parameters)
* Identifies the container image to be deployed and corresponding execution unit
* Creates a new process in the OGC API — Processes instance

## Run

Platform Z receives an _OGC API — Processes execution_ request for Eric’s deployed process.

The platform uses the Application Package CWL to retrieve the specified container image, create the container, map the instantiated parameters with the execution values and execute the application.

For the Platform Z, the Application Package is a portable and executable document that:

* Describes the application metadata of the process
* Maps the OGC API — Processes input parameters to the application input parameters
* Identifies the container image to be deployed, the corresponding execution unit, to monitor the execution and retrieve the results