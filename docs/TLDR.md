# Deployment of a Highly Available Authenticated Elasticsearch Cluster on GCP

## Choice of Tech Stack:
- **Operating System:** Ubuntu 20.04 LTS
- **Containerization:** Docker for deployment
- **Storage:** Dedicated SSD Persistent Disks for data storage
- **Security:** TLS for secure communication and Basic Authentication for user access
- **Elasticsearch Version:** 8.15.3

## Actual Solution:

### Local DEV Environment
1. **Configure TLS**

   There are 2 verification modes for SSL:
   - certificate: Only validates that the certificate is signed by a trusted Certificate Authority (CA).
   - full: Performs a full validation, which includes checking the hostname against the certificate.

   For this demo setup, we will choose full verification mode:
   - Generate SSL certificates for each Elasticsearch node using the provided `ca.crt` and `ca.key` files.
   - Utilize [elasticsearch-certutil](https://www.elastic.co/guide/en/elasticsearch/reference/current/certutil.html#certutil-cert) to create the cert bundle:
      ```
      docker run --rm -v $(pwd)/certs:/certs docker.elastic.co/elasticsearch/elasticsearch:8.15.3 \
      bin/elasticsearch-certutil cert \
      --ca-cert /certs/ca.crt --ca-key /certs/ca.key \
      --pem --in /certs/instance.yml \
      --out /certs/bundle.zip
      ```

2. **Deploy Elasticsearch Cluster:**
   - Use the official Elasticsearch Docker image for version **8.15.3**.
   - Run the `dev.docker-compose.yml` file.

3. **Verify Elasticsearch Cluster:**
   - Verify SSL.
   - Verify Index sharding config.
   - Manually shutdown 1-2 nodes to test high availability, node failures tolerance capability.

### GCP PRD Environment
1. **Infra Setup:**

   Create VM Instances:
   - Set up **three `n1-standard-2`** nodes (1 core + 3.5 GB RAM each) on GCP.
   - Install Docker on each VM instance.
   - Mount SSD Disk to `/data` mountpoint.

2. **Configure TLS**
   Copy the certificates from local dev env to each VM, make sure that:
   - Cert files are read-only (600)
   - Docker user has read permission

3. **Deploy Elasticsearch:**
   - Create a `prd.docker-compose.yml` file to define the Elasticsearch services in each node

4. **Cost Management:**
   - Regularly monitor GCP usage to ensure it stays within the $300 budget. Utilize GCP pricing calculators to estimate costs for VM instances and storage.

## Time Spent:
- **Planning and Research:** 2 hours
- **DEV Setup and Configuration:** 3 hours
- **PRD Setup and Configuration:** 2 hours
- **Testing and Validation:** 2 hours
- **Documentation:** 3 hours
- **Total Time:** Approximately 12 hours

## Optional:
- **Monitoring:** Grafana, Prometheus, Loki for monitoring and logging visualization.
- **Backup:** Snapshot functionality of GCP Persistent Disks