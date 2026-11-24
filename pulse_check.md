# Backend Engineering Challenges

## 1. Generating Latency Graphs

**Challenge:** How do we generate a latency graph for a monitored website?

- Every health check should record the response time (latency) of the request.
- Store the latency along with the timestamp of the health check.
- Use the historical data to generate a time-series graph.

**Data required:**
- Website / monitor ID
- Response time (in milliseconds)
- Timestamp
- Region from which the check was performed
- Status (success / failure)

The graph can display latency changes over time, helping users identify slowdowns and performance issues.

---

## 2. Storing Region-Specific Monitoring Data

**Challenge:** How do we store health-check results from multiple geographical regions without overwriting each other's data?

### Initial Approach

Run separate pollers for different regions:

1. One poller on a server in India.
2. One poller on a server in the USA.

Each poller periodically checks the target website and stores the result in the database.

### Problem: Data Collision

Suppose both pollers monitor the same website.

If we store only a success/failure entry against the website ID, the results from different regions may overwrite each other.

**Initial solution: Add a `region` column**

Include a region identifier in every monitoring record.

However, this introduces a design limitation if regions are represented as separate columns (e.g., `india_status`, `usa_status`).

- Adding a new region requires changing the database schema.
- The table becomes difficult to maintain as the number of regions grows.
- It doesn't scale well when regions are dynamic.

### Better Solution: Separate Tables

Use two separate tables:

**1. `regions`**

Stores information about available monitoring regions.

| id | name | location |
|---|---|---|
| 1 | India | Mumbai |
| 2 | USA | Virginia |

**2. `region_status`**

Stores health-check results for each region.

| id | monitor_id | region_id | status | latency | checked_at |
|---|---|---|---|---|---|
| 1 | 101 | 1 | UP | 120 | timestamp |
| 2 | 101 | 2 | UP | 240 | timestamp |

- `region_id` references the `regions` table.
- Each health-check result is associated with a specific monitor and region.
- Adding a new region only requires inserting a new record into the `regions` table.
- No database schema modification is necessary.

**Further consideration:** Since monitoring produces time-series data, a time-series database can be considered for storing and querying historical health-check results.

PostgreSQL can also handle this workload initially. A specialized time-series solution can be introduced when data volume and query requirements justify it.

---

## 3. Avoiding Duplicate Health Checks

**Challenge:** What happens when multiple users monitor the same website?

Suppose:
- User A starts monitoring `example.com`.
- User B also starts monitoring `example.com`.
- Both users expect regular health-check updates.

A naive implementation might create two independent monitoring jobs, causing the same website to be checked multiple times from the same region.

### Problem

- Unnecessary HTTP requests.
- Wasted server and network resources.
- Increased load on the target website.
- Duplicate monitoring data.
- Inefficient use of polling workers.

### Possible Solution: Shared Monitoring

Instead of creating a separate health-check job for every user:

1. Maintain a shared monitor for each unique website and monitoring configuration.
2. Multiple users can subscribe to the same monitor.
3. Run only one health check per monitoring interval, per region.
4. Store the result centrally.
5. Distribute the result to all users subscribed to that monitor.

**Important:** A monitor's identity should account for its configuration, not just its URL.

For example, the same URL might need separate monitors if users configure different:
- HTTP methods
- Request headers
- Expected status codes
- Check intervals
- Monitoring regions

This approach avoids redundant work while allowing multiple users to share monitoring results.. multiple users hitting or tracking the same website no need to hit it multiple times 

## 4. Concurrent Polling and Worker Distribution

**Challenge:** How do we execute multiple health checks concurrently instead of relying on a single thread?

### Problem: Sequential Request Processing

- A single-threaded poller processes requests sequentially.
- This limits the number of health checks it can execute efficiently.
- JavaScript's event loop provides asynchronous I/O, but CPU-bound work and concurrency management require additional consideration.
- A single poller may become a bottleneck as the number of monitored websites increases.

### Possible Solution: Concurrent Workers

Distribute health-check requests across multiple workers.

One possible approach is to use **Go (Golang)**:

- Go provides goroutines, which are lightweight concurrent execution units.
- A large number of health checks can be handled concurrently.
- Go's runtime schedules goroutines across available OS threads and CPU cores.
- This makes it suitable for building highly concurrent polling services.

**Important:** JavaScript also supports concurrent I/O and worker threads. Go is an alternative worth exploring, not a strict requirement for concurrency.

### Multiple Pollers

A single poller is insufficient when monitoring a large number of websites.

- Increase the number of pollers.
- Distribute monitoring jobs across pollers.
- Maintain an offset or scheduling mechanism to determine which monitors each poller should process.

### Problems with a Naive Polling Architecture

1. **Poller failure**
   - If a poller crashes, scheduled health checks may be missed.
   - This results in missing entries in `region_status`.

2. **No retry mechanism**
   - Failed health checks or processing failures may not be retried.
   - Temporary network or worker failures can result in lost monitoring data.

3. **No buffering**
   - A sudden increase in monitoring jobs can overwhelm the pollers.
   - There is no mechanism to absorb temporary workload spikes.

4. **No throttling**
   - Pollers may send too many requests simultaneously.
   - This can overload workers, network resources, or target websites.

---

## 5. Horizontal Scaling

**Challenge:** A single machine cannot handle millions of health-check requests efficiently.

### Problem

As the number of monitored websites increases:

- Request volume increases.
- CPU, memory, and network resources become bottlenecks.
- A single machine eventually reaches its capacity.
- The entire monitoring system becomes dependent on one machine.

### Possible Solution: Horizontal Scaling

Instead of continuously increasing the resources of one machine (vertical scaling), distribute the workload across multiple machines.

- Run multiple polling workers across different machines.
- Distribute jobs among workers.
- Allow additional workers to join as the workload increases.
- Scale the system independently of the API server.

This allows the monitoring system to handle increasing workloads without depending on a single machine.

---

## 6. Acknowledgement-Based Queue System

**Challenge:** How do we ensure that monitoring jobs are not permanently lost when a worker crashes or fails to process a job?

### Problem with Direct Worker Processing

Suppose a poller sends a job to a worker.

1. The worker picks up the job.
2. The worker crashes before completing the health check.
3. The job is lost.
4. No other worker knows that the job needs processing.

This can result in missed health checks.

### Solution: Message Queue with Acknowledgements

Introduce a message queue between the pollers and workers.

**Workflow:**

1. The poller schedules a health-check job.
2. The job is pushed into a queue.
3. A worker pulls the job from the queue.
4. The worker performs the health check.
5. If processing succeeds, the worker sends an acknowledgement (ACK).
6. The queue removes the acknowledged job.

**What happens if a worker fails?**

- The worker does not acknowledge the job.
- The queue detects that the job was not completed within the configured visibility or acknowledgement timeout.
- The job becomes available for processing again.
- Another worker picks it up and retries the operation.

### Benefits

- Failed jobs can be retried.
- Temporary worker failures do not permanently lose jobs.
- Work can be distributed across multiple workers.
- Queues act as buffers during traffic spikes.
- Workers can scale independently of pollers.

**Important concepts to explore:**
- Message acknowledgement (ACK/NACK)
- Visibility timeout
- Retry policies
- Exponential backoff
- Dead Letter Queues (DLQ)
- At-least-once delivery
- Idempotency

### Applying the Same Architecture to Notifications

The acknowledgement-based queue architecture can also be used for the notification system.

For example:

1. A health check detects that a website is DOWN.
2. A notification job is pushed into a notification queue.
3. A notification worker picks up the job.
4. The worker sends an email or other notification.
5. On success, it acknowledges the job.
6. On failure, the job becomes available for retry.

This prevents temporary email-provider failures or notification-worker crashes from permanently losing notifications.

---

## Key Architectural Takeaways

| Problem | Possible solution |
|---|---|
| Sequential request processing | Concurrent workers / goroutines |
| Single poller bottleneck | Multiple pollers |
| Poller failure | Durable job scheduling |
| Missed health checks | Retry mechanism |
| Traffic spikes | Message queue / buffering |
| Excessive requests | Rate limiting / throttling |
| Single-machine limitation | Horizontal scaling |
| Worker crashes | Acknowledgement-based queues |
| Lost notifications | Notification queue with retries |

## Topics to Learn While Building

- Goroutines and channels in Go
- Concurrency vs parallelism
- Worker pools
- Message queues and delivery guarantees
- ACK/NACK mechanisms
- Retry strategies and exponential backoff
- Rate limiting and backpressure
- Horizontal vs vertical scaling
- Idempotent job processing
- Dead Letter Queues

==Betterstack as a company makes sure they are not down then that will be an oxymoron, in that case nothing can be done==


# More plans / Features:
1. Escalation policy (if i don't take the call when the server is down / the manager will receive a call if he also does not take the call / the call is sent to CEO and so on)