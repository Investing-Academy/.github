# Project Architecture

```mermaid
graph TB
    subgraph Infrastructure["Infrastructure Layer"]
        MongoDB[(MongoDB Database)]
        DockerCompose[Docker Compose Orchestration]
    end

    subgraph ETL["ETL Processing Layer"]
        SalesETL[Sales Data Pipeline]
        StudentsETL[Students Data Pipeline]
        StudentsCalc[Students Analytics Engine]
        TeachersPayments[Teachers Payments Processor]
        BackupRestore[Backup & Restore Service]
    end

    subgraph DataSources["Data Sources"]
        Gmail[Gmail API]
        WhatsApp[WhatsApp Integration]
    end

    subgraph Storage["Data Storage"]
        SalesSheet[Sales Google Sheet]
        StudentsSheet[Students Google Sheet]
        TeachersSheet[Teachers Payments Sheet]
    end

    Gmail -->|Read Leads| SalesETL
    SalesETL -->|Update| SalesSheet
    
    WhatsApp -->|Read Messages| StudentsETL
    StudentsETL -->|Write Data| MongoDB
    StudentsETL -->|Update| StudentsSheet
    
    MongoDB -->|Query Statistics| StudentsCalc
    StudentsCalc -->|Update Analytics| StudentsSheet
    
    MongoDB -->|Query Payments Data| TeachersPayments
    TeachersPayments -->|Update Payments| TeachersSheet
    
    BackupRestore -->|Backup| MongoDB
    BackupRestore -->|Restore| MongoDB
    
    DockerCompose -.->|Orchestrates| SalesETL
    DockerCompose -.->|Orchestrates| StudentsETL
    DockerCompose -.->|Orchestrates| StudentsCalc
    DockerCompose -.->|Orchestrates| TeachersPayments
    DockerCompose -.->|Orchestrates| BackupRestore
    DockerCompose -.->|Manages| MongoDB

    classDef container fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#fff
    classDef database fill:#1a365d,stroke:#2c5282,stroke-width:2px,color:#fff
    classDef source fill:#22543d,stroke:#38a169,stroke-width:2px,color:#fff
    classDef storage fill:#742a2a,stroke:#e53e3e,stroke-width:2px,color:#fff
    
    class SalesETL,StudentsETL,StudentsCalc,TeachersPayments,BackupRestore,DockerCompose container
    class MongoDB database
    class Gmail,WhatsApp source
    class SalesSheet,StudentsSheet,TeachersSheet storage
```


# websites - appscript

```mermaid
graph TB
    subgraph Sales["Sales ETL Pipeline"]
        A[Gmail Data Extraction Service]
    end
    
    subgraph Students["Students ETL Pipeline"]
        B[WhatsApp Integration Service]
    end
    
    subgraph Payments["Teacher Payments ETL Pipeline"]
        C[Google Sheets Sync Service<br/>Bidirectional data synchronization<br/>for lesson updates]
    end
    
    D[MongoDB Database<br/>Central data repository]
    E[Lead Management Portal<br/>CRM website for leads]
    F[Student Management System<br/>Web-based administration portal]
    G[Teacher Payment Portal<br/>Financial management interface]
    
    A -->|Extract sales data| D
    B -->|Process student updates| D
    D -->|Provide student data| B
    C -->|Read/Write operations| D
    E -->|Auto-sync lead signups| F
    D -->|Supply data| F
    D -->|Supply data| G
```
