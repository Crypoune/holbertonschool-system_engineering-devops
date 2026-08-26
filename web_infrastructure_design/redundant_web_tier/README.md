# Redundant Web Tier

This project extends the previous single-server design by introducing a load balancer, two redundant web/application paths, and a primary/replica database architecture.

The architecture is represented in [`redundant_web_tier.mmd`](redundant_web_tier.mmd).

## Load Balancer and Redundant Paths

The load balancer, **HAProxy**, was added to distribute incoming traffic between two web/application paths.

Two redundant paths were added so that the application tier is no longer dependent on a single web server and application server. If one path becomes unavailable, the other can continue serving requests. Having two active paths can also increase the amount of traffic the application can handle.

## Active-Active vs Active-Passive

The two web/application paths use an **active-active** configuration. Both paths are active at the same time and can receive requests from the load balancer.

In an **active-passive** configuration, only one path normally handles traffic while the other remains on standby and takes over after a failure.

Active-active can provide both higher availability and additional capacity, while active-passive mainly provides redundancy and standby capacity.

## Load Distribution Method

A load balancer can use **round robin** to distribute requests.

With round robin, requests are sent to the available web/application paths in turn. For example, the first request can go to node 1, the second to node 2, the third back to node 1, and so on.

## Database Primary and Replica

The **Database primary (MySQL)** is the writable database used by the application servers. The application paths send their database operations to the primary.

The **Database replica (MySQL)** maintains a copy of the primary database through replication. The replica is not treated as a writable database in this architecture.

The `replication` connection represents data being copied from the primary database to the replica.

Replication provides a secondary copy of the data, but replication alone does not provide automatic failover or guarantee database write availability if the primary fails.

## Remaining Single Points of Failure

The architecture still contains important single points of failure.

The **Load balancer (HAProxy)** remains a SPOF because it is the single entry point for traffic. If the load balancer fails, requests cannot reach either web/application path.

The **Database primary (MySQL)** also remains a SPOF for writes. Both application paths depend on the primary for database operations, and the replica is not configured to take over automatically.

## Security and Monitoring Limitations

This design does not implement **HTTPS**, so encrypted communication is not provided.

It does not include **firewalls** to restrict and control network traffic.

It also does not include **monitoring** to detect failures, resource problems, or abnormal behavior.

These omissions are important limitations for a production environment.

## Benefits and Cost of Redundancy

Redundancy can improve **availability** because the loss of one web/application path does not necessarily make the application unavailable.

It can also improve **capacity** because multiple application paths can process traffic at the same time.

However, redundancy increases infrastructure and operational cost. Additional servers, a load balancer, database replication, configuration, maintenance, and operational procedures all add complexity and require additional resources.
