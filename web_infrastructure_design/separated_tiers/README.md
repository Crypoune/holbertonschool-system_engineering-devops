# Separated Tiers

This project separates the web, application, and database layers into distinct tiers and introduces a second load balancer to conceptually remove the load balancer as a single point of failure.

The architecture is represented in [`separated_tiers.mmd`](separated_tiers.mmd).

## Comparison with the Single-Server Design

In the single-server design, the web server, application server, application code, and database all depend on the same server. A failure or maintenance operation affecting that server can therefore affect the entire application.

In the separated-tier design, the web, application, and database services are placed into distinct infrastructure groups. The load-balancer pair provides two entry points instead of a single load balancer, reducing the load balancer's single-point-of-failure risk at a conceptual level.

## Independent Scaling

Separated tiers allow each layer to be scaled independently according to its workload.

For example, if the web tier receives more traffic but the application tier does not require additional capacity, additional web servers can be added without having to scale the application or database tier at the same time.

Likewise, additional application servers can be added when application processing becomes the bottleneck, while the database tier can be scaled according to database-specific requirements.

This flexibility is more difficult in a single-server architecture because all services share the same infrastructure.

## Maintenance Isolation

Separating the tiers can reduce the impact of maintenance on unrelated components.

For example, maintenance or replacement of a web server does not require the database server to be taken offline. Likewise, application-tier maintenance can be performed without directly modifying the web-tier or database-tier infrastructure.

The extent of this isolation depends on the actual infrastructure and dependencies between services, but separated tiers provide a clearer boundary between components.

## Evidence-Based Capacity Planning

The number of instances in each tier should be based on measured demand, expected traffic and growth, failure tolerance, and a justified safety margin.

Instance counts should not simply be copied from an architecture diagram or increased to the maximum possible number without evidence. Additional instances increase infrastructure cost and operational complexity, so capacity should be sized according to observed workload and expected requirements.

Capacity planning should therefore use real measurements and realistic growth assumptions to determine how many instances are appropriate for each tier.

## Remaining Limitations

The database tier still has a writable primary database. The replica provides a second copy of the data, but this design does not implement automatic database failover, so the primary remains a risk to write availability.

The separated architecture also increases infrastructure and operational cost because multiple tiers, servers, load balancers, and database replication must be managed.

The load-balancer pair addresses the load balancer SPOF only conceptually. No routing protocol or automatic failover mechanism is implemented in this design.
