# NSD-Based DNS Server

---

## Overview

This repository contains configuration files for setting up a DNS server using NSD (Name Server Daemon) in a Docker container.

### **Reason for Choosing NSD**

While BIND has been widely used for DNS implementations, its frequent vulnerabilities and challenging security management led us to select NSD as a superior alternative for the following reasons:

- Enhanced security compared to BIND.
- Specialization in read-only operations, ensuring fast and stable performance in large-scale environments.
- Simpler configuration compared to BIND.

Reference:
[NSD vs BIND Comparison](https://qiita.com/ohhara_shiojiri/items/497aaba989151fa84b3d)

### **System Architecture**

The system receives name resolution queries from clients (localhost) and returns IP addresses from the authoritative DNS server back to the clients.

![System Architecture Diagram](https://github.com/KeishiNishio/Docker_DNSserver/blob/main/%20systemimage1.png)

## Setup Instructions

1. **Clone the Repository**: First, clone this repository to your local machine.

    ```bash
    git clone https://github.com/KeishiNishio/Docker_DNSserver.git
    ```
    
2. **Build the Docker Image**: Next, use the provided `Dockerfile` to build the Docker image.

    ```bash
    docker-compose build
    ```
    
3. **Launch the Docker Container**: Start the Docker container with the following command.

    ```bash
    docker-compose up -d
    ```
    
With these steps, the NSD-based DNS server will be up and running.

### Verifying Functionality

Executing the following command should return the corresponding IP address:

```bash
dig @localhost example.com
```

## File Structure

- `docker-compose.yml`: Contains Docker configurations for the NSD service.
- `Dockerfile`: Directs the installation of NSD and the placement of configuration files.
- `nsd.conf`: The configuration file for NSD.
- `zonefile.zone`: The DNS zone file for the `example.com` domain.

## Other Useful Commands

1. Delete all Docker cache system-wide:

    ```bash
    docker system prune --all --force
    ```

2. Check if containers belong to the configured network:

    ```bash
    docker network inspect nginx-network
    ```

3. Check the status of containers:

    ```bash
    docker ps -a
    ```

4. Stop all existing containers:

    ```bash
    docker stop $(docker ps -aq) || true
    ```

5. Remove all existing containers:

    ```bash
    docker rm -f $(docker ps -aq) || true
    ```

## Development Environment

This project is developed in the following environment:

- **Operating System**: macOS
- **Docker**:
  - Version: 24.0.5
- **Docker Compose**:
  - Version: 2.23.3
