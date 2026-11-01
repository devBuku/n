Search a **sorted array** by repeatedly dividing the search range in half.

## 1. Inclusive vs Exclusive

The key difference is whether `end` is a valid index or one past the last index.

|            | Inclusive        | Exclusive           |
| ---------- | ---------------- | ------------------- |
| `end`      | Last valid index | One past last index |
| Initialize | `length - 1`     | `length`            |
| Loop       | `st <= end`      | `st < end`          |
| Move left  | `end = mid - 1`  | `end = mid`         |
| Move right | `st = mid + 1`   | `st = mid + 1`      |

==**Remember:** Inclusive includes `end`; exclusive excludes `end`.==

## 2. Implementation (Exclusive)

```ts
function bs(haystack: number[], needle: number): boolean {
    let st = 0;
    let end = haystack.length;

    while (st < end) {
        const mid = Math.floor(st + (end - st) / 2);

        if (haystack[mid] === needle) return true;
        else if (haystack[mid] < needle) st = mid + 1;
        else end = mid;
    }

    return false;
}
```

## 3. Interview reminders

- Empty array? Loop doesn't run; return `false`.
- Always calculate `mid` using `st + (end - st) / 2`.
- Never use `st = mid` or `end = mid - 1` in the exclusive version.
- Time: `O(log n)` | Space: `O(1)`

**Quick recall:** Exclusive means `[st, end)` — `st` is included, `end` is excluded.