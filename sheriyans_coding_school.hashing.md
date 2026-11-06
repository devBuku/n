
- **Hashing**: A process or technique of converting a value into a fixed-size code (hash code) that can be used to store and access data quickly.
    - Hash codes are not guaranteed to be unique. Different values can produce the same hash code. This is called a collision.

| Operation | Array                     | HashMap / HashTable |
| --------- | ------------------------- | ------------------- |
| Searching | O(n) (Unsorted Array)     | Average: O(1)       |
| Insertion | O(n) (including shifting) | Average: O(1)       |
| Deletion  | O(n) (including shifting) | Average: O(1)       |

- JavaScript objects use hashing under the hood to access properties quickly in typical implementations.

    ```ts
    const student = {
      name: "buku",
      age: 69,
    };

    console.log(student.name);
    ```

- object, map, set

# Set

- A collection that stores unique values. Unlike an array, adding the same value multiple times does not create duplicates.

```ts
let set = new Set();
set.add(19);
set.add(20);
set.add(21);
set.add(21);
set.add(19);
set.add(19);
set.add(19);
set.add(19);
set.add(19);
set.add(3);

console.log(set);
// output:
// Set(4) { 19, 20, 21, 3 }
```

# Map

1. A `Map` stores entries as key-value pairs.
2. Keys are unique. Setting an existing key updates its value.
3. Values can be duplicated or unique.

```ts
let map = new Map();

map.set("firstName", "ali");
map.set("firstName", "buku"); // Updates "firstName" to "buku".
map.set("age", 19);
map.set("isPassed", true);
map.set("contact", [1234, 1324]);

// Use a key to access, update, or remove an entry.
map.delete("contact");
console.log(map.has("isPassed"));
console.log(map.size);

for (let [key, value] of map) {
  console.log(key + " -> " + value);
}
```

## Map vs. object

- Keys: A Map can use values of any type as keys. Object property keys are strings or symbols.
- Key order: A Map iterates over entries in insertion order. Object property order follows JavaScript's property-order rules.
- Size: A Map has a size property. For objects, use Object.keys(obj).length to count own enumerable string-keyed properties.
- Iteration: A Map is directly iterable. Objects are not directly iterable; use methods such as Object.keys(), Object.values(), or Object.entries().
- Frequent changes: Map is useful when entries are frequently added and removed. Objects are often a natural fit for fixed-shape records and data represented by named properties.
- Built-in behavior: Objects inherit from a prototype by default; Map stores entries separately from object properties.

## Some more points about Map
- A `Map` does not require values to be unique; multiple keys can refer to the same value.
- `Map` keys are unique by key identity/value. Setting the same key again replaces its value without adding another entry.
- “Objects are good for static data, but maps are better for frequent addition and removal” is a useful rule of thumb, not a strict performance guarantee.

[[hash_tables]] 
[[javascript_objects]]
[[set_in_javascript]]