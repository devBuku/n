# Queue

1. FIFO = First In, First Out.
2. We do not need a doubly linked list here.
3. We pop from the head: constant time operation.
4. We insert at the tail: constant time operation.
5. Pushing and popping in a queue are constant time operations.
6. `enqueue`, `dequeue`, `peek` (what is my first element without popping it off?).
7. Enqueue means adding something to the queue or pushing it to the queue.
8. Dequeue means removing or popping something from the queue.

### Code

```ts
type Node<T> = {
    value: T;
    next?: Node<T>;
};

export default class Queue<T> {
    public length: number;
    private head?: Node<T>;
    private tail?: Node<T>;

    constructor() {
        this.head = this.tail = undefined;
        this.length = 0;
    }

    enqueue(item: T): void {
        const node = { value: item } as Node<T>;
        this.length++;

        if (!this.tail) {
            this.tail = this.head = node;
            return;
        }

        // 1. First set the next of the queue and tell the tail to point to the new node.
        // 2. Then make the new node the tail itself.
        this.tail.next = node;
        this.tail = node;
    }

    dequeue(): T | undefined {
        if (!this.head) return undefined;

        this.length--;
        const head = this.head;
        this.head = this.head.next;

        if (!this.head) this.tail = undefined;

        // Free memory here if we were using a traditional language (language with manual memory 
        // management).
        // In TypeScript, the garbage collector handles memory once the node is no longer referenced.

        head.next = undefined;
        return head.value;
    }

    peek(): T | undefined {
        return this.head?.value;
    }
}
```

[[frontend_masters]]