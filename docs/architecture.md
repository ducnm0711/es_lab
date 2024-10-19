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

The solution is hosted on Google Cloud Platform (GCP), utilizes Cloud Load Balancer to distribute traffic, ensures seamless request routing to active nodes.

Elasticsearch Cluster: 
- Comprises three master nodes for high availability, tolerates node failures without downtime.
- Each master node has a dedicated SSD for efficient storage.
- Performance: SSDs provide high I/O performance and low latency.

