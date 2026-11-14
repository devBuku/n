## What is Object Pooling?

Object Pooling is a **creational design pattern** where we maintain a collection (pool) of reusable objects instead of creating and destroying objects every time we need them.

The idea is simple:

Instead of allocating a new object for every request, we reuse an existing object by assigning it new values.

This can reduce:
- Frequent memory allocations.
- Unnecessary object creation and destruction.
- Garbage collection pressure.

## How does it work?

Object pooling generally follows this lifecycle:

1. **Create:** Initialize a pool with a certain number of objects (or create them when needed).
2. **Acquire:** When a resource is needed, get an available object from the pool.
3. **Use:** Assign the required values and perform the operation.
4. **Reset:** Clear the object's previous state.
5. **Release:** Return the object to the pool so it can be reused.

If the pool is empty, we can either create a new object or wait until an existing object becomes available. This depends on the implementation.

## Without Object Pooling vs With Object Pooling

### Without Object Pooling

Every time we need an object:

- Create a new object.
- Use it.
- Stop referencing it after the operation.
- Let the garbage collector reclaim its memory when possible.

### With Object Pooling

- Maintain reusable objects in a pool.
- Acquire an available object.
- Use it.
- Reset its previous state.
- Return it to the pool.

**Main difference:** We reuse existing objects instead of repeatedly allocating new ones.

## TypeScript Implementation

Let's implement a simple object pool.

### 1. Create the object

```ts
class User {
    name = "";
    age = 0;

    reset() {
        this.name = "";
        this.age = 0;
    }
}
```

The `reset()` method clears the previous object's state before reuse.

### 2. Create the Object Pool

```ts
class UserPool {
    private pool: User[] = [];

    acquire(): User {
        return this.pool.pop() ?? new User();
    }

    release(user: User): void {
        user.reset();
        this.pool.push(user);
    }
}
```

Understanding the methods:

- `acquire()`: Gets an available object from the pool. If the pool is empty, creates a new one.
- `release()`: Resets the object and puts it back into the pool.

### 3. Use the Pool

```ts
const pool = new UserPool();

const user = pool.acquire();

user.name = "Shubhayan";
user.age = 22;

// Perform some operation using the user.

pool.release(user);
```

The next time we call `acquire()`, we can reuse the previously released object.

## Important Things to Remember

1. **Reset the object:** Always clear previous state before returning an object to the pool.
2. **Avoid shared references:** Never reuse an object while another part of the application is still using it.
3. **Handle pool exhaustion:** Decide whether to create another object, wait, or reject the request.
4. **Limit pool size:** An unlimited pool can consume unnecessary memory.
5. **Measure performance:** Object pooling is not automatically faster.

## Real-World Use Cases

| Use case | What gets reused? |
|---|---|
| Game development | Bullets, particles, enemies |
| Database connection pool | Database connections |
| Network programming | Buffers |
| High-performance systems | Frequently allocated objects |

**Example: Database Connection Pool**

Imagine an application receiving 100 requests simultaneously.

Creating 100 separate database connections can be expensive.

Instead, we maintain a pool of database connections.

1. A request acquires an available connection.
2. Executes its database queries.
3. Releases the connection.
4. Another request can reuse it.

Note: Database connection pooling is resource pooling, which follows the same general idea as object pooling.

## When Should We Use Object Pooling?

Use it when:
- Objects are expensive to create.
- Objects are created and destroyed frequently.
- Allocation and garbage collection are measurable performance bottlenecks.
- Objects have a predictable lifecycle and can safely be reset.

Avoid it when:
- Objects are cheap to create.
- Objects are short-lived and garbage collection handles them efficiently.
- Managing the pool introduces more complexity than the performance benefit.

Modern garbage collectors are highly optimized. Object pooling can sometimes make performance worse because pooled objects remain in memory and require additional management.

## Interview Questions

**1. What is Object Pooling?**

Object pooling is a creational design pattern where we maintain a collection of reusable objects. Instead of creating new objects every time, we acquire an existing object, use it, reset it and return it to the pool.

**2. Why do we use Object Pooling?**

To reduce frequent memory allocations, object creation overhead and garbage collection pressure, especially in performance-critical applications.

**3. What happens when the pool is empty?**

Depending on the implementation, we can create a new object, wait for an object to become available, or reject the request.

**4. Is Object Pooling always beneficial?**

No. Modern garbage collectors efficiently handle many short-lived objects. Pooling introduces additional complexity and can increase memory usage. We should use it when profiling shows a real performance benefit.

**5. What is the difference between Object Pooling and Connection Pooling?**

Object pooling reuses objects, whereas connection pooling reuses expensive connections such as database connections. Both follow the same resource reuse principle.



---

## Quick Revision

**Object Pooling = Create once, reuse many times.**

Lifecycle:

`Acquire → Use → Reset → Release → Reuse`

Main purpose: Reduce repeated allocation and resource creation overhead.

Remember: Don't optimize blindly. Measure first.