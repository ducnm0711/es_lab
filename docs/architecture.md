# Elasticsearch Cluster Architecture

```mermaid
graph TD
    subgraph GCP
        direction TB
        LB[Cloud Load Balancer]
        
        subgraph "Elasticsearch Cluster"
            direction TB
            M1[Master Node 1] --> SSD1[Dedicated SSD Disk]
            M2[Master Node 2] --> SSD2[Dedicated SSD Disk]
            M3[Master Node 3] --> SSD3[Dedicated SSD Disk]
        end
    end

    LB --> M1
    LB --> M2
    LB --> M3

```

Provider node: `ges-ldap-01` (read & write)

Consumer nodes: `ges-ldap-02`, `ges-ldap-03` (read-only)

Replication is asynchronous, for every 100 operations or 10 minutes, which ever is sooner.


