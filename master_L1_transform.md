graph TD
    Start((Start Pipeline)) --> Lookup["Get Level1 Transform Instances\nLookup Activity"]

    subgraph ControlDB["Control Database - Azure SQL"]
        SP["ELT.GetTransformInstance_L1\nStored Procedure"]
    end

    Lookup --> SP
    SP --> Lookup

    Lookup --> ForEach{"ForEach Level 1\nTransform Instance"}

    subgraph Parallel["Parallel Processing\nBatch 4"]
        ChildPipeline["Level 1 Transform\nExecute Pipeline"]
    end

    ForEach --> ChildPipeline

    ChildPipeline --> Params["Pass Parameters\nL1TransformInstanceID\nIngestID\nComputePath\nWorkspace IDs"]

    ChildPipeline --> End((End Iteration))
    End --> Final((Finish))

    style Start fill:#f9f,stroke:#333
    style Final fill:#f9f,stroke:#333
    style Lookup fill:#d4edda,stroke:#28a745
    style ForEach fill:#fff3cd,stroke:#ffc107
    style ChildPipeline fill:#cce5ff,stroke:#004085
