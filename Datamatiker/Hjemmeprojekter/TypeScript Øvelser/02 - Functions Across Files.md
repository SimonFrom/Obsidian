# 02 — Functions Across Files (import & export)

**Difficulty:** ⭐ (the core skill)

## Concepts you'll practise

- Splitting code into modules
- **Named exports** vs a **default export**
- Importing functions from another file
- Keeping a "utility" file separate from your "main" file

## Background

Real projects don't live in one giant file. You put related functions in their own
file (a *module*) and import them where needed.

**Named exports** — you can have many per file, and you import them by name:

```ts
// mathUtils.ts
export function add(a: number, b: number): number { return a + b; }
export function subtract(a: number, b: number): number { return a - b; }
```

```ts
// main.ts
import { add, subtract } from "./mathUtils.js";
console.log(add(2, 3));
```

**Default export** — one per file, imported with any name you like:

```ts
// logger.ts
export default function log(message: string): void {
  console.log(`[LOG] ${message}`);
}
```

```ts
// main.ts
import log from "./logger.js";
log("hi");
```

## Your task

Create **three** files in a `02/` folder.

**`02/mathUtils.ts`** — a utility module with named exports:

- `add(a: number, b: number): number`
- `subtract(a: number, b: number): number`
- `multiply(a: number, b: number): number`
- `divide(a: number, b: number): number` — return `a / b`

**`02/logger.ts`** — a module with a **default** export:

- a function `log(message: string): void` that prints `"[LOG] <message>"`.

**`02/main.ts`** — imports from both files and uses them:

```ts
import { add, multiply } from "./mathUtils.js";
import log from "./logger.js";

log(`2 + 3 = ${add(2, 3)}`);
log(`4 * 5 = ${multiply(4, 5)}`);
```

Run it: `npx tsx 02/main.ts`

## Test

`02/mathUtils.test.ts`:

```ts
import assert from "node:assert/strict";
import { add, subtract, multiply, divide } from "./mathUtils.js";

assert.equal(add(2, 3), 5);
assert.equal(subtract(10, 4), 6);
assert.equal(multiply(4, 5), 20);
assert.equal(divide(20, 4), 5);

console.log("✅ 02 passed");
```

Run: `npx tsx 02/mathUtils.test.ts`

## Stretch goal

Add a `power(base: number, exponent: number): number` to `mathUtils.ts`, export it,
and add an assertion for it in the test (`power(2, 10) === 1024`).

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 02/mathUtils.ts
export function add(a: number, b: number): number {
  return a + b;
}
export function subtract(a: number, b: number): number {
  return a - b;
}
export function multiply(a: number, b: number): number {
  return a * b;
}
export function divide(a: number, b: number): number {
  return a / b;
}
export function power(base: number, exponent: number): number {
  return base ** exponent;
}
```

```ts
// 02/logger.ts
export default function log(message: string): void {
  console.log(`[LOG] ${message}`);
}
```

```ts
// 02/main.ts
import { add, multiply } from "./mathUtils.js";
import log from "./logger.js";

log(`2 + 3 = ${add(2, 3)}`);
log(`4 * 5 = ${multiply(4, 5)}`);
```

</details>
