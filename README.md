# High Availability Containerized Web Application on GCP

This project demonstrates the deployment of a high-availability web application stack using Docker, LVM, HAProxy, and Google Cloud Platform (GCP). The stack includes WordPress (3 replicas), MySQL with persistent storage via LVM, and a load balancer (HAProxy). It was developed for COP3604 – System Administration Using Unix.

---

## Features

- Dockerized WordPress and MySQL containers
- High availability with 3 front-end replicas
- Load balancing via HAProxy
- Persistent database storage using LVM
- Secret management using environment variables
- Deployed on a GCP Compute Engine VM

---

## Architecture

[ Clients ]
     |
[ Load Balancer ]
     |
-----------------------------
|     |     |     |
WP1  WP2  WP3   <- WordPress replicas
     |
  [ MySQL ]
     |
  [ LVM Disk ]

---

## Deployment Instructions

1. Clone the repository:

git clone https://github.com/HunterM26/ha-containerized-gcp-project.git
cd ha-containerized-gcp-project

2. Create a `.env` file based on the provided example:

cp .env.example .env

Edit `.env` and set your values:

MYSQL_DATABASE=wordpress  
MYSQL_USER=wpuser  
MYSQL_PASSWORD=wppass  
MYSQL_ROOT_PASSWORD=rootpass  

3. Launch the stack with 3 WordPress replicas:

docker-compose up -d --scale wordpress=3

4. Visit the site in your browser:

http://<YOUR-GCP-EXTERNAL-IP>

---

## Load Balancing Test

Tested with Apache Benchmark:

ab -n 100 -c 10 http://<YOUR-GCP-IP>/

Confirmed load distribution across all WordPress replicas using `docker stats`.

---

## Failover Test

Stopped one replica with:

docker stop <container_id>

Site continued working through remaining replicas via HAProxy.

---

## Folder Structure

.
├── docker-compose.yml  
├── haproxy/  
│   └── haproxy.cfg  
├── .env.example  
├── README.md  
└── report/  
    └── HA_Project_Report.pdf  
---

## References

https://docs.docker.com/  
https://www.haproxy.org/  
https://cloud.google.com/compute/  
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/managing_storage_devices

## Author

Hunter  
COP3604 – System Administration Using Unix  
Fall 2025
