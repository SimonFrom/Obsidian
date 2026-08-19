# 06 — Higher-Order Functions

**Difficulty:** ⭐⭐⭐

## Concepts you'll practise

- Function types: `(n: number) => boolean`
- Passing functions as arguments
- Returning functions from functions
- Typing `map` / `filter` / `reduce` callbacks

## Background

A **higher-order function** takes a function as an argument, returns one, or both.

You type a function value like this:

```ts
type Predicate = (n: number) => boolean;

function keep(numbers: number[], test: (n: number) => boolean): number[] {
  return numbers.filter(test);
}

keep([1, 2, 3, 4], (n) => n % 2 === 0); // [2, 4]
```

A function that **returns** a function:

```ts
function multiplier(factor: number): (n: number) => number {
  return (n) => n * factor;
}
const triple = multiplier(3);
triple(5); // 15
```

## Your task

Create `06/functional.ts` and `06/main.ts`.

In **`06/functional.ts`**, export:

1. `applyToEach(numbers: number[], fn: (n: number) => number): number[]`
   - returns a new array with `fn` applied to every element
2. `countWhere(numbers: number[], test: (n: number) => boolean): number`
   - returns how many elements pass the test
3. `multiplier(factor: number): (n: number) => number`
   - returns a function that multiplies its argument by `factor`
4. `pipe2(f: (n: number) => number, g: (n: number) => number): (n: number) => number`
   - returns a function that runs `f` first, then `g` on the result

In **`06/main.ts`**, import them and, for example, build `const triple =
multiplier(3)`, then `applyToEach([1,2,3], triple)`.

## Test

`06/functional.test.ts`:

```ts
import assert from "node:assert/strict";
import { applyToEach, countWhere, multiplier, pipe2 } from "./functional.js";

assert.deepEqual(applyToEach([1, 2, 3], (n) => n * 10), [10, 20, 30]);
assert.equal(countWhere([1, 2, 3, 4, 5], (n) => n > 2), 3);

const triple = multiplier(3);
assert.equal(triple(5), 15);
assert.deepEqual(applyToEach([1, 2, 3], triple), [3, 6, 9]);

const addOne = (n: number) => n + 1;
const square = (n: number) => n * n;
const addThenSquare = pipe2(addOne, square);
assert.equal(addThenSquare(3), 16); // (3+1)^2

console.log("✅ 06 passed");
```

## Stretch goal

Add `pipe(...fns: Array<(n: number) => number>): (n: number) => number` that
composes any number of functions left-to-right. Hint: `reduce` over the arguments.

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 06/functional.ts
export function applyToEach(
  numbers: number[],
  fn: (n: number) => number,
): number[] {
  return numbers.map(fn);
}

export function countWhere(
  numbers: number[],
  test: (n: number) => boolean,
): number {
  return numbers.filter(test).length;
}

export function multiplier(factor: number): (n: number) => number {
  return (n) => n * factor;
}

export function pipe2(
  f: (n: number) => number,
  g: (n: number) => number,
): (n: number) => number {
  return (n) => g(f(n));
}

// Stretch:
export function pipe(
  ...fns: Array<(n: number) => number>
): (n: number) => number {
  return (n) => fns.reduce((acc, fn) => fn(acc), n);
}
```

</details>
