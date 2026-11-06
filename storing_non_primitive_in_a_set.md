# Using a Set to Store Unique Pairs and Triplets in TypeScript

## 1. The problem

While solving 3Sum [[15]] using three nested loops, the same triplet can be found multiple times in different orders.

For example, these are all the same triplet:

```ts
[1, -1, 0]
[0, 1, -1]
[-1, 0, 1]
```

We only want to store the triplet once.

We can use a `Set` to keep track of the triplets we have already found.

## 2. Why can't we store arrays directly in a Set?

==JavaScript compares objects and arrays by reference, not by their contents.==

```ts
const set = new Set<number[]>();

set.add([1, 2]);
set.add([1, 2]);

console.log(set.size); // 2
```

Even though both arrays contain the same numbers, they are two different array objects in memory.

Therefore, the `Set` considers them different values.

We need to convert the array into a value that can be compared by its contents.

## 3. Convert the pair or triplet into a string

One simple solution is to convert the array into a string using `join()`.

```ts
const pair = [2, 3];

const key = pair.join(",");

console.log(key); // "2,3"
```

Now we can store the string in a `Set<string>`.

```ts
const set = new Set<string>();

set.add("2,3");

console.log(set.has("2,3")); // true
```

Strings are compared by value, so the `Set` can detect duplicate keys.

## 4. Why sorting is necessary

Consider these two pairs:

```ts
[2, 3]
[3, 2]
```

They contain the same numbers, but their string representations are different.

```ts
"2,3"
"3,2"
```

To make sure the same pair always produces the same key, we sort the array before converting it into a string.

```ts
const pair1 = [2, 3];
const pair2 = [3, 2];

pair1.sort((a, b) => a - b);
pair2.sort((a, b) => a - b);

console.log(pair1.join(",")); // "2,3"
console.log(pair2.join(",")); // "2,3"
```

The same idea applies to triplets.

```ts
const triplet = [1, -1, 0];

triplet.sort((a, b) => a - b);

console.log(triplet); // [-1, 0, 1]
console.log(triplet.join(",")); // "-1,0,1"
```

Sorting puts the numbers in a consistent order. Joining converts them into a string that can be used as a unique key.

## 5. Applying this to brute-force 3Sum

The brute-force approach uses three nested loops to check every possible combination of three different indices.

If the sum of the three numbers is zero, we have found a valid triplet.

We then:

1. Sort the triplet.
2. Convert it into a string.
3. Check whether the `Set` already contains that key.
4. If it does not, add the key to the `Set` and the triplet to the result.

```ts
function threeSum(nums: number[]): number[][] {
    const set = new Set<string>();
    const result: number[][] = [];

    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            for (let k = j + 1; k < nums.length; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    const triplet = [
                        nums[i],
                        nums[j],
                        nums[k],
                    ];

                    triplet.sort((a, b) => a - b);

                    const key = triplet.join(",");

                    if (!set.has(key)) {
                        set.add(key);
                        result.push(triplet);
                    }
                }
            }
        }
    }

    return result;
}
```

Example:

```ts
threeSum([-1, 0, 1, 2, -1, -4]);

// [
//   [-1, -1, 2],
//   [-1, 0, 1]
// ]
```

## 6. Understanding the Set operations

| Operation | Meaning |
|---|---|
| `new Set<string>()` | Creates an empty Set of strings |
| `set.has(key)` | Checks whether the key already exists |
| `set.add(key)` | Adds the key if it is not already present |

The important point is that the `Set` stores the string representation of the triplet, not the triplet array itself.

## 7. Time and space complexity

Let `n` be the length of the input array.

- Three nested loops: `O(n³)`
- Sorting a triplet: `O(1)`, because it always contains exactly three numbers.
- Creating the string key and checking the Set: `O(1)` for this fixed-size triplet.

Overall time complexity: `O(n³)`.

The result and Set can each store up to `O(n³)` triplets in the worst case.

Overall auxiliary space complexity: `O(n³)`, excluding the returned result.

## Key takeaway

A `Set` cannot directly detect duplicate arrays by their contents because arrays are compared by reference.

To store unique pairs or triplets:

1. Sort the numbers to get a consistent order.
2. Convert the array into a string using `join()`.
3. Use the string as the key in the `Set`.

This technique is useful whenever you need to store unique combinations of values in JavaScript or TypeScript.