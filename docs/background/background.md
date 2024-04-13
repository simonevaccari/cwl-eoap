# Background

How was CWL selected as the open standard to support the packaging of Earth Observation applications?

There are several activities and these are depicted below:


``` mermaid
flowchart LR
    subgraph References["`**References**`"]
      subgraph Testbed-13
        tb13["`EP Application Package Engineering Report (17-023)
            Application Deployment and Execution Service Engineering Report (17-024)
            Cloud Engineering Report (17-035)`"]
    end
    subgraph Testbed-14
        tb14["`Application Package Engineering Report (18-049r1)
            ADES & EMS Results and Best Practices Engineering Report (18-050r1)`"]
    end
    subgraph Testbed-15
        tb15["`Catalogue and Discovery Engineering Report (19-020r1)`"]
    end
    end
    subgraph eoap["`**Earth Observation Applications Pilot**`"]
        eoaper["`20-042 Terradue Engineering Report
                20-045 CRIM Engineering Report
                20-038 European Union Satellite Centre Engineering Report
                20-043 EOX Consortium Engineering Report
                20-037 Pixalytics Engineering Report
                20-034 Spacebel Engineering Report`"]
        summary-er["EO Applications Pilot 
        Summary Engineering Report (20-073)"]
        eoaper --> summary-er
    end
    Testbed-13 --> eoap
    Testbed-14 --> eoap
    Testbed-15 --> eoap
    summary-er --> bp["`Best Practice for Earth Observation Application Package (20-089)`"] 
    bp --> tb16["`Testbed-16: Earth Observation Application Packages with Jupyter Notebooks`"]
    bp --> tb17["`Testbed 17: COG/Zarr Evaluation Engineering Report`"]
    bp --> tb18["`Testbed-18: Testbed-18: Reproducible FAIR Best Practices Engineering Report`"]
```    


This section provides the background activities that lead to this selection.


