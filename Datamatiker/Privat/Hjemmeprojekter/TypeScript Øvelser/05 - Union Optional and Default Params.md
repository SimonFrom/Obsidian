# 05 — Union Types, Optional & Default Parameters

**Difficulty:** ⭐⭐⭐

## Concepts you'll practise

- Union types: `"asc" | "desc"`, `string | number`
- Optional parameters with `?`
- Default parameter values
- **Narrowing** — checking which type you actually have before using it

## Background

A **union** means "one of these types":

```ts
type Direction = "asc" | "desc";
function move(dir: Direction) { /* ... */ }
```

**Optional** parameters may be omitted (their type becomes `T | undefined`):

```ts
function greet(name: string, title?: string): string {
  return title ? `${title} ${name}` : name;
}
```

**Default** parameters supply a value when the caller passes nothing:

```ts
function repeat(text: string, times: number = 2): string {
  return text.repeat(times);
}
```

**Narrowing** — when a value could be more than one type, check before using it:

```ts
function format(value: string | number): string {
  if (typeof value === "number") {
    return value.toFixed(2); // here TS knows value is a number
  }
  return value.toUpperCase(); // here TS knows value is a string
}
```

## Your task

Create `05/format.ts` (utilities) and `05/main.ts` (uses them).

In **`05/format.ts`**, export:

1. `formatValue(value: string | number): string`
   - if it's a number, return it fixed to 2 decimals (`"3.50"`)
   - if it's a string, return it uppercased
2. `sortNumbers(numbers: number[], direction?: "asc" | "desc"): number[]`
   - sorts ascending by default; descending when `"desc"` is passed
   - **must not mutate** the input array (copy it first with `[...numbers]`)
3. `joinWords(words: string[], separator: string = ", "): string`
   - joins words using the separator, default `", "`

In **`05/main.ts`**, import and demonstrate each — including calling `sortNumbers`
both with and without the direction argument.

## Test

`05/format.test.ts`:

```ts
import assert from "node:assert/strict";
import { formatValue, sortNumbers, joinWords } from "./format.js";

assert.equal(formatValue(3.5), "3.50");
assert.equal(formatValue("hello"), "HELLO");

assert.deepEqual(sortNumbers([3, 1, 2]), [1, 2, 3]);
assert.deepEqual(sortNumbers([3, 1, 2], "desc"), [3, 2, 1]);

// original array must be untouched
const original = [3, 1, 2];
sortNumbers(original);
assert.deepEqual(original, [3, 1, 2]);

assert.equal(joinWords(["a", "b", "c"]), "a, b, c");
assert.equal(joinWords(["a", "b", "c"], " - "), "a - b - c");

console.log("✅ 05 passed");
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 05/format.ts
export function formatValue(value: string | number): string {
  if (typeof value === "number") {
    return value.toFixed(2);
  }
  return value.toUpperCase();
}

export function sortNumbers(
  numbers: number[],
  direction: "asc" | "desc" = "asc",
): number[] {
  const copy = [...numbers];
  copy.sort((a, b) => (direction === "asc" ? a - b : b - a));
  return copy;
}

export function joinWords(words: string[], separator: string = ", "): string {
  return words.join(separator);
}
```

Note: I made `direction` a default parameter (`= "asc"`) rather than optional
(`?`). Either works here; a default is nicer because the body never has to handle
`undefined`.

</details>
