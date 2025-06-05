# document_agent
flowchart TB
    subgraph "Document Agent"
        direction TB
        subgraph "Input Processing"
            UP["Upload Handler"]
            FV["File Validation"]
            MD["Metadata Extraction"]
        end

        subgraph "Storage Operations"
            S3H["S3 Handler"]
            DBH["Database Handler"]
            PH["Pinecone Handler"]
        end

        subgraph "Document Processing"
            TE["Text Extraction"]
            VE["Vector Embedding"]
            PC["PDF Converter"]
        end

        subgraph "Management Operations"
            DM["Delete Manager"]
            RM["Restore Manager"]
            LM["Lifecycle Manager"]
        end
    end

    subgraph "External Systems"
        S3[("AWS S3")]
        DB[("SQL Database")]
        PDB[("Pinecone DB")]
    end

    %% Input Flow
    File[/"Uploaded File"/] --> UP
    UP --> FV
    FV --> MD

    %% Processing Flow
    MD --> S3H
    MD --> DBH
    MD --> PC
    PC --> TE
    TE --> VE
    VE --> PH

    %% Storage Flow
    S3H --> S3
    DBH --> DB
    PH --> PDB

    %% Management Flow
    DM --> S3H
    DM --> DBH
    RM --> S3H
    RM --> DBH
    LM --> DM
    LM --> RM

    %% Metadata Flow
    Meta[/"Document Metadata"/] --> DBH

    classDef input fill:#f9f,stroke:#333,stroke-width:2px
    classDef storage fill:#bfb,stroke:#333,stroke-width:2px
    classDef process fill:#bbf,stroke:#333,stroke-width:2px
    classDef external fill:#fbb,stroke:#333,stroke-width:2px

    class UP,FV,MD input
    class S3H,DBH,PH storage
    class TE,VE,PC process
    class DM,RM,LM process
    class S3,DB,PDB external
