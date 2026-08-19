# 07 — Generics

**Difficulty:** ⭐⭐⭐⭐

## Concepts you'll practise

- Generic functions with a type parameter `<T>`
- Why generics beat `any` (they keep type information)
- Generic constraints (`<T extends ...>`)
- Reusing a generic utility module across files

## Background

A generic lets a function work with *any* type while still being type-safe:

```ts
function first<T>(items: T[]): T | undefined {
  return items[0];
}

first([1, 2, 3]);       // inferred as number | undefined
first(["a", "b"]);      // inferred as string | undefined
```

Compare with `any`, which throws the type away:

```ts
function firstAny(items: any[]): any { return items[0]; }
const x = firstAny(["a"]); // x is `any` — no help, no safety
```

A **constraint** restricts what `T` can be:

```ts
function longest<T extends { length: number }>(a: T, b: T): T {
  return a.length >= b.length ? a : b;
}
longest("hello", "hi");        // ok, strings have .length
longest([1, 2, 3], [1]);       // ok, arrays have .length
// longest(3, 4);              // error, numbers have no .length
```

## Your task

Create `07/generics.ts` and `07/main.ts`.

In **`07/generics.ts`**, export:

1. `firstOrUndefined<T>(items: T[]): T | undefined`
2. `lastOrUndefined<T>(items: T[]): T | undefined`
3. `pair<A, B>(a: A, b: B): [A, B]` — returns a tuple
4. `pluck<T, K extends keyof T>(items: T[], key: K): T[K][]`
   - returns an array of the values at that key
   - example: `pluck(people, "name")` returns a `string[]`

`pluck` is the tricky one — `keyof T` means "any key of T", and `T[K]` is the type
of the value at that key. This is where generics really earn their keep.

In **`07/main.ts`**, import them and try `pluck` on an array of objects.

## Test

`07/generics.test.ts`:

```ts
import assert from "node:assert/strict";
import { firstOrUndefined, lastOrUndefined, pair, pluck } from "./generics.js";

assert.equal(firstOrUndefined([10, 20, 30]), 10);
assert.equal(firstOrUndefined<number>([]), undefined);
assert.equal(lastOrUndefined(["a", "b", "c"]), "c");

assert.deepEqual(pair("age", 27), ["age", 27]);

const people = [
  { name: "Ada", age: 36 },
  { name: "Bo", age: 22 },
];
assert.deepEqual(pluck(people, "name"), ["Ada", "Bo"]);
assert.deepEqual(pluck(people, "age"), [36, 22]);

console.log("✅ 07 passed");
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 07/generics.ts
export function firstOrUndefined<T>(items: T[]): T | undefined {
  return items[0];
}

export function lastOrUndefined<T>(items: T[]): T | undefined {
  return items[items.length - 1];
}

export function pair<A, B>(a: A, b: B): [A, B] {
  return [a, b];
}

export function pluck<T, K extends keyof T>(items: T[], key: K): T[K][] {
  return items.map((item) => item[key]);
}
```

Hover over `pluck(people, "name")` in your editor — TypeScript infers the result as
`string[]`, and `pluck(people, "age")` as `number[]`, all from one generic
signature. Try `pluck(people, "nope")` and it won't compile, because `"nope"` isn't
a key of the object. That safety is the whole point.

</details>
