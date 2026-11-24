# Recursion

## What is Recursion?

The simplest way to think of recursion is a function that **calls itself** until the problem is solved.

This usually involves what is called a **base case**.

> A base case is the condition at which recursion stops. It prevents the function from calling itself indefinitely.

## How Recursion Works

A recursive function generally has two important components:

1. **Base case:** Defines when the recursion should stop.
2. **Recursive case:** The function calls itself with a smaller or simpler version of the original problem.

### Important

Don't think of recursion as just two separate steps:

1. Write the base case.
2. Make the recursive call.

Instead, understand how each recursive call moves the problem closer to the base case.

**Every recursive call should make progress towards termination.**

## Asynchronous Recursion in JavaScript

JavaScript also supports asynchronous recursion.

A function can call itself after an asynchronous operation completes, such as after a Promise resolves.

Example:

```ts
async function countdown(n: number): Promise<void> {
    if (n === 0) { // base case
        console.log("Done!");
        return;
    }

    console.log(n);

    await new Promise(resolve => setTimeout(resolve, 1000));

    await countdown(n - 1);
}

countdown(5);
```


[[frontend_masters]]