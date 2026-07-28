# 01 — Basic Types & Your First Typed Function

**Difficulty:** ⭐ (warm-up)

## Concepts you'll practise

- Type annotations on variables (`string`, `number`, `boolean`)
- Typing a function's parameters and its return value
- Letting TypeScript catch a mistake for you

## Background

In TypeScript you annotate values with a type after a colon:

```ts
let age: number = 27;
let name: string = "Simon";
let isStudent: boolean = true;
```

A typed function declares the type of each parameter and what it returns:

```ts
function double(n: number): number {
  return n * 2;
}
```

The `: number` after the parentheses is the **return type**. TypeScript will warn
you if you accidentally return the wrong thing.

## Your task

Create a single file `01/main.ts` with these functions:

1. `greet(name: string): string` — returns `"Hello, <name>!"`.
2. `add(a: number, b: number): number` — returns the sum.
3. `isEven(n: number): boolean` — returns `true` if `n` is even.
4. `describeAge(age: number): string` — returns `"You are <age> years old."`.

At the bottom of the file, call each one and `console.log` the result so you can
see it work:

```ts
console.log(greet("Simon"));
console.log(add(2, 3));
```

Run it:

```bash
npx tsx 01/main.ts
```

> **Try this to feel the type checker:** temporarily call `add("2", 3)` and see
> TypeScript complain before you even run the code. Then change it back.

## Test

Create `01/main.test.ts` and copy this in. Then run `npx tsx 01/main.test.ts`.

```ts
import assert from "node:assert/strict";
import { greet, add, isEven, describeAge } from "./main.js";

assert.equal(greet("Simon"), "Hello, Simon!");
assert.equal(add(2, 3), 5);
assert.equal(isEven(4), true);
assert.equal(isEven(7), false);
assert.equal(describeAge(27), "You are 27 years old.");

console.log("✅ 01 passed");
```

> **Note on `./main.js`:** with `"module": "NodeNext"`, imports use the `.js`
> extension even though your file is `main.ts`. That's normal — TypeScript resolves
> it. If you'd rather not deal with that yet, set `"module": "ESNext"` and
> `"moduleResolution": "Bundler"` in your tsconfig and import from `"./main"`.

For the test to import them, your functions in `main.ts` must be **exported**.
That's a sneak peek at the next exercise — add `export` in front of each:

```ts
export function greet(name: string): string { ... }
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 01/main.ts
export function greet(name: string): string {
  return `Hello, ${name}!`;
}

export function add(a: number, b: number): number {
  return a + b;
}

export function isEven(n: number): boolean {
  return n % 2 === 0;
}

export function describeAge(age: number): string {
  return `You are ${age} years old.`;
}

console.log(greet("Simon"));
console.log(add(2, 3));
console.log(isEven(4));
console.log(describeAge(27));
```

</details>
