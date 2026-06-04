# Project: Web infrastructure design

A progressive series of whiteboard-style infrastructure diagrams for hosting `www.foobar.com`, covering everything from a basic single-server stack to a scaled, secured, and monitored production setup.

## Tasks

### 0. Simple web stack

A single-server infrastructure hosting the full stack: Nginx, an application server, and a MySQL database. The domain `www.foobar.com` resolves to the server's IP via a DNS A record.

**Key concepts:** LAMP/LEMP stack, DNS resolution, what a web server vs. application server does, SPOF, downtime on maintenance.

---

### 1. Distributed web infrastructure

Introduces a load balancer (HAProxy) distributing traffic across two servers, each running the full stack. A MySQL Primary-Replica cluster handles data persistence.

**Key concepts:** Round-robin load balancing, Active-Active vs. Active-Passive setup, Primary-Replica replication, remaining SPOFs (the load balancer itself).

---

### 2. Secured and monitored web infrastructure

Adds security and observability to the distributed setup: three firewalls, an SSL certificate (HTTPS), and monitoring clients on each machine reporting to Sumo Logic.

**Key concepts:** Firewall placement, SSL termination at the load balancer (and why it's a problem), monitoring data collection, QPS tracking, risks of a single write node.

---

### 3. Scale up

Splits each component onto its own dedicated server (web, app, database) and adds a second HAProxy node configured as a cluster to eliminate the load balancer as a SPOF.

**Key concepts:** Horizontal scaling, separation of concerns, HAProxy cluster synchronization, independent scaling of each tier.
