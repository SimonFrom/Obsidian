# 09 — Async Functions & Promises

**Difficulty:** ⭐⭐⭐⭐

## Concepts you'll practise

- Typing a Promise: `Promise<T>`
- `async` / `await`
- Running async work in sequence vs in parallel (`Promise.all`)
- Handling errors with `try` / `catch`

## Background

An `async` function always returns a `Promise`. Its return type is `Promise<T>`
where `T` is what it resolves to:

```ts
async function getGreeting(name: string): Promise<string> {
  return `Hello, ${name}!`; // wrapped in a Promise automatically
}

const message = await getGreeting("Simon"); // message is string
```

A helper to simulate a delay (no real network needed):

```ts
function delay(ms: number): Promise<void> {
  return new Promise((resolve) => setTimeout(resolve, ms));
}
```

Run things in **parallel** with `Promise.all`:

```ts
const [a, b] = await Promise.all([getGreeting("A"), getGreeting("B")]);
```

## Your task

Create `09/async.ts` and `09/main.ts`.

In **`09/async.ts`**, export:

1. `delay(ms: number): Promise<void>` — resolves after `ms` milliseconds
2. `fetchUser(id: number): Promise<{ id: number; name: string }>`
   - wait 50ms (use `delay`), then resolve with `{ id, name: "User " + id }`
   - if `id` is negative, `throw new Error("Invalid id")`
3. `fetchAllUsers(ids: number[]): Promise<{ id: number; name: string }[]>`
   - fetch every id **in parallel** with `Promise.all`
4. `safeFetchUser(id: number): Promise<{ id: number; name: string } | null>`
   - calls `fetchUser`, but returns `null` instead of throwing on error

In **`09/main.ts`**, write a top-level `async` `main()` that awaits these and logs
the results, then call `main()`.

## Test

`09/async.test.ts`:

```ts
import assert from "node:assert/strict";
import { fetchUser, fetchAllUsers, safeFetchUser } from "./async.js";

async function run() {
  const user = await fetchUser(1);
  assert.deepEqual(user, { id: 1, name: "User 1" });

  const users = await fetchAllUsers([1, 2, 3]);
  assert.equal(users.length, 3);
  assert.equal(users[2].name, "User 3");

  await assert.rejects(() => fetchUser(-1), /Invalid id/);

  assert.equal(await safeFetchUser(-1), null);
  assert.deepEqual(await safeFetchUser(5), { id: 5, name: "User 5" });

  console.log("✅ 09 passed");
}

run();
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 09/async.ts
export function delay(ms: number): Promise<void> {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

export interface User {
  id: number;
  name: string;
}

export async function fetchUser(id: number): Promise<User> {
  if (id < 0) {
    throw new Error("Invalid id");
  }
  await delay(50);
  return { id, name: `User ${id}` };
}

export async function fetchAllUsers(ids: number[]): Promise<User[]> {
  return Promise.all(ids.map((id) => fetchUser(id)));
}

export async function safeFetchUser(id: number): Promise<User | null> {
  try {
    return await fetchUser(id);
  } catch {
    return null;
  }
}
```

> **Why `Promise.all`?** Fetching three users sequentially with `await` in a loop
> would take 3 × 50ms = 150ms. In parallel it's ~50ms total. Same types, better
> performance.

</details>
