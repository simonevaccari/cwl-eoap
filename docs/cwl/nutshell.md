# Common Workflow Language

The Common Workflow Language (CWL) is a set of open standards for describing analysis workflows and tools in a way that makes them portable and scalable across a variety of software and hardware environments, from workstations to cluster, cloud, and high-performance computing (HPC) environments.

The CWL targets data-intensive processing scenarios and makes these portable and scalable across platforms capable of interpreting and execute the processes by describing:

* A runtime environment
* A Workflow (Directed Acyclic Graph or “DAG”)
* Command line tool(s)
* Parameter of the process
* Inputs/outputs

The CWL contains two main specifications:

- The `CommandLineTool` Description Specification that specifies the document schema and execution semantics for wrapping and executing command line tools 
- The `Workflow` Description Specification that specifies the document schema and execution semantics for composing workflows from components such as command line tools and other workflows. 

The CWL file is able to reference the application container images and also allow the definitions of the Application parameters, input/output interface and the overall process offering parameters.

Each input to a command line tool has a name and a type (e.g., File, string) and developers are encouraged to include documentation and labels for all components. Metadata about the command line tool descriptions can contain well-defined hints or mandatory requirements such as which software container to use or how much compute resources are required (memory, number of CPU cores, disk space, and/or the maximum time or deadline to complete the step or entire workflow.)

The CWL execution model is explicit: each command line tool’s runtime environment is explicit and any required elements must be specified by the CWL tool-description author (in contrast to hints, which are optional). Each tool invocation uses a separate working directory, populated according to the CWL tool description, e.g., with the input files explicitly specified by the workflow author. Some applications can expect particular filenames, directory layouts, and environment variables, and there are additional constructs in the CWL Command Line Tool standard to satisfy these needs.

The CWL standards use a declarative syntax, facilitating polylingual workflow tasks. By being explicit about the run-time environment and any use of software containers, the CWL standards enable portability and reuse while also providing a separation of concerns between workflow authors and workflow platforms.

The execution block (i.e. Application Artefact) describes the ‘software’ component that represents the execution unit in a specific container image to be executed or specific workflow script that can be invoked on the processor directly. Based on the context information provided with the processor, the execution block maps how the container image can be parameterized or tailored.

A container image is an immutable, static file containing the dependencies for the creation of a container. These dependencies may include a single executable binary file, system libraries, system tools, environment variables, and other required platform settings (Cloud Native Glossary).

In overall, a container image describes a container environment whereas a container is an instance of that environment, ran by a container engine (e.g. Docker Engine). It is possible to run multiple containers from the same image, and all of them will contain the same software and configuration, as specified in the image.


There are multiple community or commercially supported systems that support the CWL standards for executing workflows and a list of free and open-source implementations of the CWL standards 