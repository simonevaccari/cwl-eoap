# The Application Package and CWL as a solution for Earth Observation portability

The Application Package is a document that describes the data processing application by providing information about the parameters, software item, executable, dependencies and metadata. This file document ensures that the application is fully portable among all supporting processing scenarios and supports automatic deployment in a Machine-To-Machine (M2M) scenario. 

Most importantly, the Application Package information model allows the deployment of the application as an OGC API — Processes (OGC 18-062) compliant web service.

The Application Package includes the following information:

*  Reference to the executable block that implements the Application functionality
*  Description of its input/output interface

The Application Package uses the Common Workflow Language (CWL) Workflow Description specification as an encoding to describe the Application, its parameters, command-line tools, their runtime environments, their arguments and their invocation within containers.

