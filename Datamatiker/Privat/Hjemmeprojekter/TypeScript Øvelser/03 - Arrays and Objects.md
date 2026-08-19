# 03 — Arrays & Objects

**Difficulty:** ⭐⭐

## Concepts you'll practise

- Typing arrays (`number[]`, `string[]`)
- Typing object shapes inline (`{ name: string; age: number }`)
- Functions that take and return objects
- Keeping the data-processing functions in their own file

## Background

Array types are written with `[]`:

```ts
const scores: number[] = [10, 8, 9];
const names: string[] = ["Ada", "Linus"];
```

An object's shape can be described inline:

```ts
function describe(person: { name: string; age: number }): string {
  return `${person.name} is ${person.age}`;
}
```

## Your task

Create `03/stats.ts` (utility module) and `03/main.ts` (uses it).

In **`03/stats.ts`**, export these named functions:

1. `sum(numbers: number[]): number` — total of all numbers.
2. `average(numbers: number[]): number` — the mean. Return `0` for an empty array.
3. `max(numbers: number[]): number` — the largest number.
4. `longestName(people: { name: string; age: number }[]): string` — returns the
   `name` of the person whose name has the most characters.

In **`03/main.ts`**, import them and try them out:

```ts
import { sum, average, longestName } from "./stats.js";

const people = [
  { name: "Ada", age: 36 },
  { name: "Grace", age: 41 },
  { name: "Bo", age: 22 },
];

console.log(sum([1, 2, 3, 4]));       // 10
console.log(average([2, 4, 6]));      // 4
console.log(longestName(people));     // "Grace"
```

## Test

`03/stats.test.ts`:

```ts
import assert from "node:assert/strict";
import { sum, average, max, longestName } from "./stats.js";

assert.equal(sum([1, 2, 3, 4]), 10);
assert.equal(average([2, 4, 6]), 4);
assert.equal(average([]), 0);
assert.equal(max([3, 9, 2, 7]), 9);
assert.equal(
  longestName([
    { name: "Ada", age: 36 },
    { name: "Grace", age: 41 },
    { name: "Bo", age: 22 },
  ]),
  "Grace",
);

console.log("✅ 03 passed");
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 03/stats.ts
type Person = { name: string; age: number };

export function sum(numbers: number[]): number {
  return numbers.reduce((total, n) => total + n, 0);
}

export function average(numbers: number[]): number {
  if (numbers.length === 0) return 0;
  return sum(numbers) / numbers.length;
}

export function max(numbers: number[]): number {
  return Math.max(...numbers);
}

export function longestName(people: Person[]): string {
  let longest = people[0].name;
  for (const person of people) {
    if (person.name.length > longest.length) {
      longest = person.name;
    }
  }
  return longest;
}
```

Notice `average` reuses `sum` from the same file — small functions composing into
bigger ones. `type Person` is defined once and reused; the next exercise moves
that kind of type into its own file.

</details>
