
## Core Utilization

Core utilization represents the percentage of time a CPU core spends executing work rather than remaining idle.

$$
\text{Core Utilization} =
\frac{\text{Total Time}-\text{Idle Time}}{\text{Total Time}}
\times 100
$$

- **Idle time:** Time during which the CPU core is not executing a process or thread.
- **Busy time:** Time during which the CPU core is executing work.

Example:

- Total observation time = 10 seconds
- Idle time = 2 seconds
- Busy time = 8 seconds

$$
\frac{10-2}{10}\times100=80\%
$$

## Overall CPU Utilization

Overall CPU utilization is the average utilization across all CPU cores.

$$
\text{CPU Utilization} =
\frac{\sum_{i=1}^{N}\text{Core Utilization}_i}{N}
$$

Where N is the number of CPU cores.

Example: A machine with 4 cores:

| Core   | Utilization |
| ------ | ----------: |
| Core 1 |        100% |
| Core 2 |         50% |
| Core 3 |         25% |
| Core 4 |         25% |

Overall CPU utilization = 50%.

## Threads and CPU Cores

- A single thread can execute on only one CPU core at a given instant.
- The OS scheduler can migrate a thread between different CPU cores over time.
- Multiple threads can execute in parallel on multiple CPU cores.
- A single thread cannot execute instructions simultaneously on multiple cores.

## Example: Infinite Loop

```typescript
while (true) {}
```

- JavaScript executes this loop on the main thread.
- The thread continuously executes instructions without yielding.
- One CPU core becomes fully occupied, resulting in approximately 100% utilization on that core.
- In Node.js, this blocks the event loop, preventing other JavaScript callbacks and incoming request handlers from executing.

```typescript
import { Worker, isMainThread } from "worker_threads";
import { fileURLToPath } from "url";

if (isMainThread) {
  // Spawn 3 system level threads (because i have 4 cores)
  for (let i = 0; i < 3; i++) {
    new Worker(fileURLToPath(import.meta.url));
  }
} else {
  // Run this in each of the threads
  while (true) {}
}

```
- so if we are using method 1 to calculate cpu usage we will be seeing 300% cpu utilization (method 1 meaning without dividing the number of total cores)
```shell
npx autocannon -m GET -c 20 -d 20 -p 2 "http://localhost:3001/simple"

Running 20s test @ http://localhost:3001/simple
20 connections with 2 pipelining factor


┌─────────┬──────┬──────┬───────┬──────┬─────────┬─────────┬───────┐
│ Stat    │ 2.5% │ 50%  │ 97.5% │ 99%  │ Avg     │ Stdev   │ Max   │
├─────────┼──────┼──────┼───────┼──────┼─────────┼─────────┼───────┤
│ Latency │ 1 ms │ 1 ms │ 2 ms  │ 2 ms │ 1.03 ms │ 0.34 ms │ 19 ms │
└─────────┴──────┴──────┴───────┴──────┴─────────┴─────────┴───────┘
┌───────────┬─────────┬─────────┬────────┬─────────┬─────────┬──────────┬─────────┐
│ Stat      │ 1%      │ 2.5%    │ 50%    │ 97.5%   │ Avg     │ Stdev    │ Min     │
├───────────┼─────────┼─────────┼────────┼─────────┼─────────┼──────────┼─────────┤
│ Req/Sec   │ 19,439  │ 19,439  │ 31,071 │ 35,327  │ 30,092  │ 3,567.74 │ 19,431  │
├───────────┼─────────┼─────────┼────────┼─────────┼─────────┼──────────┼─────────┤
│ Bytes/Sec │ 4.88 MB │ 4.88 MB │ 7.8 MB │ 8.86 MB │ 7.55 MB │ 895 kB   │ 4.88 MB │
└───────────┴─────────┴─────────┴────────┴─────────┴─────────┴──────────┴─────────┘

Req/Bytes counts sampled once per second.
# of samples: 20

602k requests in 20.01s, 151 MB read

```
-c = no. of connections
-p = pipeline
-w = no. of threads that we are going to spawn to run this test on
1. whatever we send through tcp/udp this is received by the network card of that machine (in this case the server we will stress test)
2. -w 2 (this means we are going to spawn 2 threads on the client machine from which we will send the request) (this means we can do two things at the same time on our cpu)
3. -c 6 means we are going to open up 6 connections to the server that means 6 tcp connections in this case 
4. -p 2 how many requests we are going to send immediately at the same time
![[Pasted image 20261124114846.png]] 
5. to know at any given point of time how many request a server is handling (we have to multiply -c * -p) (in this case 12 concurrent request this server is handling)
## Synchronous vs Asynchronous Operations

- **Synchronous (blocking):** The main JavaScript thread waits for the operation to complete before executing subsequent JavaScript.
- **Asynchronous (non-blocking):** The main JavaScript thread initiates an operation and continues executing other JavaScript while waiting for its completion.
- Asynchronous operations may be handled by:
    - Operating system facilities (e.g. network I/O).
    - libuv's thread pool (e.g. certain file system and crypto operations).
    - Other external systems (e.g. database servers).
- Once an operation completes, its callback or continuation is scheduled for execution through the event loop.