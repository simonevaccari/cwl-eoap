# Application Best Practice

An Application that complies with the Best Practice for Earth Observation Application Package needs to be:

* Executable as a command-line tool.
* Delivered in a container image with all the necessary software, libraries and configuration files.

Here we describe the best practices for Earth Observation applications:

* the command line tools
* how to consider input data, output data 
* how to create a containers

## Command line

The Application is executed as a command-line interface (CLI) tool that runs as a non-interactive executable program: it receives input arguments, performs a computation, and terminates after producing some output.

The Application can have any number of command-line arguments.

When executed, the Application working directory is also the Application output directory. Any file created by the Application should be added under that directory.

### Hands-on

Open the notebook **01 Command line**

## Container

The environment, libraries, binaries and configuration files necessary to execute the command-line tools need to be bundled in a container image.

The example below shows how Docker, one of the available container engine solutions to deliver software in containers, defines all the necessary commands to assemble an image:

```
FROM docker.io/python:3.10-slim

RUN pip install --no-cache-dir rasterio click pystac loguru pyproj shapely && \
    python -c "import rasterio"

ADD app.py /app/app.py

ENTRYPOINT []
```

### Hands-on

Open the notebook **02 Container**