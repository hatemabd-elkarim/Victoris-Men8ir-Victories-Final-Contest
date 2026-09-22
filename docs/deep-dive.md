## Architecture Choice

We chose a **microservices architecture** because the system has clear domains such as authentication, orders, products and notifications.

Each service can be developed and scaled independently. For example, during periods of heavy order activity, the Orders and Robots services can have more instances without scaling the other services.

The trade-off is increased complexity in service-to-service communication, deployment, monitoring, and data consistency compared to a modular monolith.

## Scaling

At high scale, the first problem is that a service instance can become a bottleneck in CPU, memory, and concurrent requests.

We use **horizontal scaling** with multiple stateless instances behind a load balancer. During rush times, such as Black Friday, New Year, Summer Sales and periods when there are Huge Sales heavily, we can add more instances to the services receiving higher traffic.

**Trade-off:** this improves throughput and availability, but increases infrastructure complexity. Statelessness also means that services cannot depend on local server state.

As the number of instances increases, another problem is that requests can still overload a specific service if traffic is uneven. The load balancer distributes requests across the available instances to avoid sending all traffic to one instance.

## Database

At the beginning, a centralized relational database is simpler and makes it easier to maintain consistency between users, orders, robots, and others.

At high scale, the first problem is the increasing number of database reads, especially from the orders and products feed.

We can first optimize queries and add proper indexes. If read traffic continues to grow, we can introduce **read replicas** and send read-heavy requests to them while writes continue going to the primary database.

**Trade-off:** read replicas improve read scalability but introduce **replication lag**, meaning a recent update may not immediately appear on a replica.

For operations where **read-after-write consistency** is important, such as immediately viewing a newly listed product or newly created order, we can read from the primary database instead.

For large files such as images and test videos, we use **object storage** instead of storing the files directly in the database. The database stores only references to these files.

## Cache

The product feed is highly read-heavy, so repeatedly querying the database for the same popular products can eventually make the database a bottleneck.

We can cache frequently requested data such as:

- Popular products
- Robot allocated tasks

This reduces database load and improves response time.

**Trade-off:** cached data can become stale. For example, if a product data changed, the old version is still present in the cache.
We can use **TTL** and invalidate important cache entries when the underlying data changes.

However, when a popular cache entry expires, many requests may try to rebuild it at the same time. This creates a **cache stampede** and can overload the database.

We can reduce this by refreshing popular entries before expiration and using **request coalescing** so that only one request rebuilds the cache entry while the others wait for the result.

## Message Broker, Queue & Workers

Tasks allocation algorithm can be expensive, especially when many orders are created or updated at the same time.

If allocation is done synchronously, the customer order needs to wait for the algorithm to allocate tasks for available robots.

We can use a message broker to hold and distribute background tasks between services and workers. After an order is created or updated, the Order Service sends a task to the broker instead of calling the algorithm directly.

Order service -> Message broker -> Allocation Algorithm -> database

The broker acts as a buffer between the services and workers, allowing tasks to wait when workers are busy.

Trade-off: the order creation response becomes faster and allocation can scale independently, but the results are no longer guaranteed to be immediately available.

At high scale, the next problem is queue backlog. If orders arrive faster than workers can process them, the queue keeps growing.

We can add more workers based on queue size to increase processing capacity.

This allows the Tasks allocation workers to scale independently from the rest of the system.
