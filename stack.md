# Stack

1. LIFO (Last In, First Out), like a stack of plates.
2. Pushing and popping happen from one side, so both operations take constant time, O(1).
3. `push` and `pop` in a stack are like `enqueue` and `dequeue` in a queue, but a stack uses LIFO while a queue uses FIFO.
4. JavaScript arrays use `push()` and `pop()` for stack operations. `shift()` and `unshift()` are for removing from and adding to the beginning of an array, so they are not ideal for a stack.

```ts
type Node<T> = {
    value: T;
    prev?: Node<T>;
};

export default class Stack<T> {
    public length: number;
    head?: Node<T>;

    constructor() {
        this.head = undefined;
        this.length = 0;
    }

    push(item: T): void {
        const node = { value: item } as Node<T>;
        this.length++; // must increase length before if statement 

        if (!this.head) {
            this.head = node;
            return;
        }

        node.prev = this.head;
        this.head = node;
    }

    pop(): T | undefined {
        if (!this.head) return undefined;

        const head = this.head;
        this.head = this.head.prev;
        this.length--;

        return head.value;
    }

    peek(): T | undefined {
        return this.head?.value;
    }
}
```

[[frontend_masters]]