# Application Package Best Practice

A Package that complies with the _OGC Best Practice for Earth Observation Application Package_ needs to:

* Be a valid CWL document with a single Workflow Class and at least one CommandLineTool Class
* Define the command-line and respective arguments and container for each CommandLineTool
* Define the Application parameters
* Define the Application Design Pattern
* Define the requirements for runtime environment

The Workflow class steps field orchestrates the execution of the application command line and retrieves all the outputs of the processing steps.

## CWL Document

The CWL Document references the Application parameters with the CWL class _Workflow_ and the command lines tools and arguments with the CWL _CommandLineTool_ classes.

```yaml
cwlVersion: v1.1
$graph:
- class: Workflow
  id: crop
  ...

- class: CommandLineTool
  id: crop-cl
  ...
```

## Command-Line Tool

As stated previously, the command-line tool is a non-interactive executable program that reads some input, performs a computation, and terminates after producing some output.

The CWL _CommandLineTool_ class defines the actual interface of the command-line tool and its arguments according to the CWL CommandLineTool standard.

The CWL explicitly supports the use of software container technologies, such as Docker or Podman, to enable portability of the underlying analysis tools. 

The Application Package needs to explicitly provide for each command-line tool the container requirements defining the container image needed.

The CWL requirement `DockerRequirement` indicates that the component must be run in a container, and specifies how to fetch or build the image.

```yaml
cwlVersion: v1.2

$graph:
- class: Workflow
  id: crop
  ...

- class: CommandLineTool
  id: crop-cl
  
  requirements:
    DockerRequirement:
      dockerPull: localhost/crop:latest
```

The field inputs defines the list of input parameters of the command-line that control how to run the tool. Each parameter has an id for the name of parameter, and a type field describing what types of values are valid for that parameter (e.g. `string`, `int`, `double`, `null`, `File`, `Directory`, `Any`). Additionally, if there are command-line bindings not directly associated with input parameters (e.g. fixed values or environment run-time values), the field arguments can also be used.

```yaml
cwlVersion: v1.2

$graph:
- class: Workflow
  id: crop
  ...
  
- class: CommandLineTool
  id: crop-cl
  
  requirements:
    DockerRequirement:
      dockerPull: localhost/crop:latest

  baseCommand: ["python", "-m", "app"]
  arguments: []
  inputs:
    item:
      type: string
      inputBinding:
          prefix: --input-item
    aoi:
      type: string
      inputBinding:
          prefix: --aoi
    epsg:
      type: string
      inputBinding:
          prefix: --epsg
    band:
      type: string
      inputBinding:
          prefix: --band
  ...
```

When the command-line is executed under CWL, the starting working directory is the designated output directory. The underlying tool or script records its results in the form of files created in the output directory.

All the outputs of the command line tool are retrieved at this level.

```yaml
cwlVersion: v1.2

$graph:
- class: Workflow
  id: crop
  ...
  
- class: CommandLineTool
  id: crop-cl
  
  requirements:
    DockerRequirement:
      dockerPull: localhost/crop:latest

  baseCommand: ["python", "-m", "app"]
  arguments: []
  inputs:
    item:
      type: string
      inputBinding:
          prefix: --input-item
    aoi:
      type: string
      inputBinding:
          prefix: --aoi
    epsg:
      type: string
      inputBinding:
          prefix: --epsg
    band:
      type: string
      inputBinding:
          prefix: --band
  outputs:
    cropped:
      outputBinding:
          glob: '*.tif'
      type: File
```

## Application

The CWL Workflow class defines the Application as an analysis task represented by a directed graph describing a sequence of operations that transform an input data set to output.

The Workflow class includes four basic blocks: identification, inputs, steps and outputs.

For the identification block, the CWL _Workflow_ class supports the definition of a unique identifier (id), a short human-readable title (label) and a long human-readable description (doc) of the Application.

```yaml
cwlVersion: v1.2
$graph:
- class: Workflow
  id: main
  label: Water bodies detection based on NDWI and the otsu threshold
  doc: Water bodies detection based on NDWI and otsu threshold applied to a single Sentinel-2 COG STAC item
```

For the inputs, the CWL _Workflow_ class supports the definition of the input parameters of the process. Each input parameter has a corresponding identifier (the field’s name), title (label), abstract (doc) and a type (type) that is mandatory according to the CWL _Workflow_ specification.

```yaml
cwlVersion: v1.2
$graph:
- class: Workflow
  id: main
  label: Water bodies detection based on NDWI and the otsu threshold
  doc: Water bodies detection based on NDWI and otsu threshold applied to a single Sentinel-2 COG STAC item
  requirements: []
  inputs:
    aoi:
      label: area of interest
      doc: area of interest as a bounding box
      type: string
    epsg:
      label: EPSG code
      doc: EPSG code
      type: string
      default: "EPSG:4326"
    bands:
      label: bands used for the NDWI
      doc: bands used for the NDWI
      type: string[]
      default: ["green", "nir"]
    item:
      doc: Reference to a STAC item
      label: STAC item reference
      type: string
```

The workflow is managed by the steps field of the Workflow class that links the corresponding parameters with arguments of the command-line class defined in the previous section.

```yaml
cwlVersion: v1.2

$graph:
- class: Workflow
  id: main
  label: Water bodies detection based on NDWI and the otsu threshold
  doc: Water bodies detection based on NDWI and otsu threshold applied to a single Sentinel-2 COG STAC item
  requirements: []
  inputs:
    aoi:
      label: area of interest
      doc: area of interest as a bounding box
      type: string
    epsg:
      label: EPSG code
      doc: EPSG code
      type: string
      default: "EPSG:4326"
    bands:
      label: bands used for the NDWI
      doc: bands used for the NDWI
      type: string[]
      default: ["green", "nir"]
    item:
      doc: Reference to a STAC item
      label: STAC item reference
      type: string
  outputs:
  ...
  steps:
    node_detect:
      run: "#detect-water-body"
      in:
        item: item
        aoi: aoi
        epsg: epsg
        band: bands
      out:
        - stac-catalog
```

For the outputs, the Workflow class includes the outputs section. This is a list of output fields where each field consists of an identifier and a data type.


```yaml
cwlVersion: v1.2

$graph:
- class: Workflow
  id: main
  label: Water bodies detection based on NDWI and the otsu threshold
  doc: Water bodies detection based on NDWI and otsu threshold applied to a single Sentinel-2 COG STAC item
  requirements: []
  inputs:
    aoi:
      label: area of interest
      doc: area of interest as a bounding box
      type: string
    epsg:
      label: EPSG code
      doc: EPSG code
      type: string
      default: "EPSG:4326"
    bands:
      label: bands used for the NDWI
      doc: bands used for the NDWI
      type: string[]
      default: ["green", "nir"]
    item:
      doc: Reference to a STAC item
      label: STAC item reference
      type: string
  outputs:
  - id: stac_catalog
    outputSource:
      - node_detect/stac-catalog
    type: Directory
  steps:
    node_detect:
      run: "#detect-water-body"
      in:
        item: item
        aoi: aoi
        epsg: epsg
        band: bands
      out:
        - stac-catalog
```

## Application Additional Metadata

The Application Package can include additional metadata in CWL descriptions and developers should provide a minimal amount of authorship information to encourage correct citation.

It is recommended to include additional metadata in the Application Package using schema.org class Person to define the author and contributions and properties like citation, codeRepository, dateCreated and license as seen in the next example.

```yaml
cwlVersion: v1.0
$namespaces:
  s: https://schema.org/
s:softwareVersion: 1.0.0
schemas:
  - http://schema.org/version/9.0/schemaorg-current-http.rdf
$graph:
- class: Workflow
  id: main
  label: Water bodies detection based on NDWI and the otsu threshold
  doc: Water bodies detection based on NDWI and otsu threshold applied to a single Sentinel-2 COG STAC item
  ...
```

## Resources for the runtime environment

CWL provides a mechanism for expressing runtime environment resource requirements with the simple rule:

* min is the minimum amount of a resource that must be reserved to schedule a job. If min cannot be satisfied, the job should not be run.
* max is the maximum amount of a resource that the job shall be permitted to use. If a node has sufficient resources, multiple jobs may be scheduled on a single node provided each job’s "max" resource requirements are met. If a job attempts to exceed its "max" resource allocation, an implementation may deny additional resources, which may result in job failure.

Hardware resources are expressed with the CWL "ResourceRequirement" allowing the definition of:

* coresMin for the minimum reserved number of CPU cores
* coresMax for the maximum reserved number of CPU cores
* ramMin for the minimum reserved RAM in mebibytes
* ramMax for the maximum reserved RAM in mebibytes

This definition covers most of the application resource requirements needs.​

If appropriate the Application Package can define resources for the runtime environment with ResourceRequirement class either at the level of each CommandLineTool classes or at the level of the Workflow class (that will be inherited to all CommandLineTool classes)

```yaml
...
- class: CommandLineTool
  id: detect-water-body
  requirements:
      ResourceRequirement:
        coresMax: 1
        ramMax: 512
...
```

## Hands-on

Open the notebook _03 Package_ for a hands-on with a CWL runner

## Requirement classes

**Requirement 7**	req/app-pck/cwl

The Application Package SHALL be a valid CWL document with a "Workflow" class and one or more "CommandLineTool" classes.

**Requirement 8**	req/app-pck/clt

The Application Package CWL CommandLineTool classes SHALL contain the following elements:
 
* Identifier ("id")
* Command line name ("baseCommand")
* Input parameters ("inputs")
* Environment requirements ("requirements")
* Docker information ("DockerRequirement")

**Requirement 9**	req/app-pck/wf

The Application Package CWL Workflow class SHALL contain the following elements:

* dentifier ("id")
* Title ("label")
* Abstract ("doc")

**Requirement 10** req/app-pck/wf-inputs

The Application Package CWL Workflow class "inputs" fields SHALL contain the following elements:

* Identifier ("id")
* Title ("label")
* Abstract ("doc")

**Requirement 11** req/app-pck/metadata

The Application Package CWL Workclass classes SHALL include additional metadata as defined in the Table 1 of the _OGC Best Practice for Earth Observation Application Package_

**Recommendation 1** rec/app-pck/fan-out

For applications with the fan-out design pattern, the Application Package CWL Workclass class MUST include the "ScatterFeatureRequirement" class in the "requirements" section and include the "scatterMethod" in the corresponding input of the step.