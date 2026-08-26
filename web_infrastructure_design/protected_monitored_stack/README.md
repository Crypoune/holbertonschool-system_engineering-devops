# Protected and Monitored Stack

This project extends the redundant web tier by adding firewalls, encrypted transport, and a monitoring system.

The architecture is represented in [`protected_monitored_stack.mmd`](protected_monitored_stack.mmd).

## Firewall

A firewall controls and filters network traffic between different parts of an infrastructure. It can allow or block traffic according to security policies and can help restrict which systems are allowed to communicate with a service.

A firewall does not encrypt application traffic, detect every type of attack, replace application-level security, or provide monitoring by itself.

Three firewalls are shown in this architecture: one before the load balancer and one before each web/application group.

## HTTPS

HTTPS is used to protect communication between the user and the entry point of the infrastructure.

HTTPS uses TLS to encrypt data in transit, helping protect requests and responses from being read or modified while they travel across the network.

In this architecture, TLS termination can occur at the load balancer. If TLS is terminated there and encryption is not continued beyond the load balancer, an internal network hop can remain unencrypted.

## Monitoring System

The monitoring system collects operational data from the infrastructure so that system health, traffic, and resource usage can be observed.

Monitoring clients or agents running on systems can collect metrics such as request counts, response times, CPU usage, memory usage, or other service-level measurements. These metrics are then sent to a monitoring system where they can be stored, displayed, and analyzed.

This architecture only models the metric flows. It does not install monitoring agents or configure alerts.

## QPS Metrics

QPS means **Queries Per Second**. It measures how many queries or requests a service processes during one second.

QPS can help identify changes in traffic and capacity. A sustained increase in QPS can indicate higher demand, while a large drop can indicate a traffic change or a service problem. Comparing QPS with available resources can also help identify when a system is approaching its capacity.

The diagram labels one web-server metric flow as `QPS metrics`.

## Remaining Write-Availability Risk

The database architecture still has one writable primary database.

The **Database primary (MySQL)** remains a write-availability risk because both application paths depend on it for database operations. The replica provides a copy of the primary data, but this design does not implement automatic failover.

If the writable primary becomes unavailable, database writes may no longer be possible even though a replica exists.

## Scaling and Maintenance Limitations

When web, application, and database services are closely colocated in the same infrastructure, scaling and maintenance become harder to perform independently.

For example, increasing web capacity may require changes to infrastructure that also hosts application or database services. Maintenance on shared infrastructure can also affect multiple service layers at the same time.

Separating these layers would make independent scaling and maintenance easier, but would require additional infrastructure and operational complexity.

## Limitations

This architecture improves protection and observability at a conceptual level, but it does not implement firewall rules, certificates, monitoring agents, or alerting.

HTTPS is shown to represent encrypted transport from the user to the entry point, but encryption is not automatically guaranteed on every internal hop.

The writable database primary remains a risk to database write availability, and colocating multiple service layers makes independent scaling and maintenance more difficult.
