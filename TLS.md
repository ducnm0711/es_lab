# SSL Configuration

A fully authenticated and secured Elasticsearch Cluster would require following security features:
- SSL/TLS for encrypted communication. 
- User authentication
- RBAC. 

## SSL Configuration

Verification Modes:
- certificate: Only validates that the certificate is signed by a trusted Certificate Authority (CA).
- full: Performs a full validation, which includes checking the hostname against the certificate.

For this demo setup, we will choose full verification mode.

### Prerequisites

- A trusted Certificate Authority (CA).
- A private key for Elastic Search Cluster




## Server-Client Communication