### Approach

1. `x[i] <= x[i + 1]` for every adjacent pair means the array is sorted.
2. In a single iteration, the largest unsorted element ends up at the end of the array.
3. An array with one element is always sorted.

### Code

```ts
export default function bubble_sort(arr: number[]): void {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                let tmp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = tmp;
            }
        }
    }
}
```
