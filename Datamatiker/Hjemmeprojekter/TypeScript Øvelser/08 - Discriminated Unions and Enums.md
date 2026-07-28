# 08 — Discriminated Unions & Enums

**Difficulty:** ⭐⭐⭐⭐

## Concepts you'll practise

- Enums for a fixed set of named values
- **Discriminated unions** — the idiomatic TS way to model "one of several shapes"
- Exhaustive `switch` with a `never` safety check
- Sharing these types across files

## Background

An **enum** names a fixed set of related values:

```ts
enum Status {
  Active = "ACTIVE",
  Paused = "PAUSED",
  Closed = "CLOSED",
}
```

A **discriminated union** is a union of object types that share a common literal
field (the *discriminant*, here `kind`). Checking that field narrows the type:

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number }
  | { kind: "square"; side: number };

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;   // TS knows radius exists here
    case "rectangle":
      return shape.width * shape.height;
    case "square":
      return shape.side ** 2;
    default:
      // if you add a new shape and forget a case, this line won't compile
      const _exhaustive: never = shape;
      return _exhaustive;
  }
}
```

The `never` trick is a compile-time guarantee that you've handled every case.

## Your task

Create `08/shapes.ts` and `08/main.ts`.

In **`08/shapes.ts`**, export:

- the `Shape` type (the discriminated union above)
- `area(shape: Shape): number` — with an exhaustive `switch` and the `never` check
- `describeShape(shape: Shape): string` — e.g. `"circle with area 12.57"` (area
  rounded to 2 decimals)
- `totalArea(shapes: Shape[]): number` — sum of all areas

In **`08/main.ts`**, build an array with all three kinds and log the total area.

## Test

`08/shapes.test.ts`:

```ts
import assert from "node:assert/strict";
import { area, totalArea, type Shape } from "./shapes.js";

const circle: Shape = { kind: "circle", radius: 2 };
const rect: Shape = { kind: "rectangle", width: 3, height: 4 };
const square: Shape = { kind: "square", side: 5 };

assert.ok(Math.abs(area(circle) - 12.566) < 0.01);
assert.equal(area(rect), 12);
assert.equal(area(square), 25);
assert.ok(Math.abs(totalArea([circle, rect, square]) - 49.566) < 0.01);

console.log("✅ 08 passed");
```

## Stretch goal

Add a `"triangle"` variant (`base`, `height`) to the `Shape` union. Notice how the
`never` check in `area` immediately flags that you haven't handled it yet — fix it
and the error disappears. That's the compiler doing your code review.

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 08/shapes.ts
export type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number }
  | { kind: "square"; side: number };

export function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "rectangle":
      return shape.width * shape.height;
    case "square":
      return shape.side ** 2;
    default: {
      const _exhaustive: never = shape;
      return _exhaustive;
    }
  }
}

export function describeShape(shape: Shape): string {
  return `${shape.kind} with area ${area(shape).toFixed(2)}`;
}

export function totalArea(shapes: Shape[]): number {
  return shapes.reduce((total, shape) => total + area(shape), 0);
}
```

</details>
