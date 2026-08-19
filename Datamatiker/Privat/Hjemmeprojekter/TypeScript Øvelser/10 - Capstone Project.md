# 10 — Capstone: A Mini Task Tracker

**Difficulty:** ⭐⭐⭐⭐⭐

Time to combine everything: types across files, generics, unions, higher-order
functions, and a **barrel file** that re-exports a whole module folder. You'll
build a small in-memory task tracker spread across several files.

## Concepts you'll practise

- Designing a multi-file module structure
- **Barrel exports** — one `index.ts` that re-exports everything
- Discriminated unions + enums for task state
- Generic + higher-order helpers working on real domain data
- Pulling it all together in a `main.ts`

## File structure to build

```
10/
├── types.ts        # the data model (interfaces, enums, unions)
├── taskStore.ts    # functions that create/update/query tasks
├── reporting.ts    # functions that summarise tasks
├── index.ts        # barrel: re-exports from the three files above
├── main.ts         # uses everything via the barrel
└── capstone.test.ts
```

## Step 1 — `10/types.ts`

Export:

```ts
export enum Priority {
  Low = "LOW",
  Medium = "MEDIUM",
  High = "HIGH",
}

export type TaskStatus = "todo" | "in-progress" | "done";

export interface Task {
  id: number;
  title: string;
  priority: Priority;
  status: TaskStatus;
}
```

## Step 2 — `10/taskStore.ts`

Import the types and export these functions. Treat tasks **immutably** — never
mutate an existing task or array; always return a new one.

1. `createTask(id: number, title: string, priority: Priority): Task`
   - new tasks start with status `"todo"`
2. `updateStatus(task: Task, status: TaskStatus): Task`
   - returns a **copy** of the task with the new status (use `{ ...task, status }`)
3. `filterByStatus(tasks: Task[], status: TaskStatus): Task[]`
4. `sortByPriority(tasks: Task[]): Task[]`
   - High first, then Medium, then Low. Don't mutate the input.

## Step 3 — `10/reporting.ts`

Import the types and export:

1. `countByStatus(tasks: Task[]): Record<TaskStatus, number>`
   - e.g. `{ "todo": 2, "in-progress": 1, "done": 3 }`
2. `completionRate(tasks: Task[]): number`
   - fraction of tasks that are `"done"`, `0`–`1`. Return `0` for no tasks.
3. `summarise(tasks: Task[]): string`
   - e.g. `"6 tasks, 50% done"` (round the percentage to a whole number)

## Step 4 — `10/index.ts` (the barrel)

Re-export everything so consumers import from one place:

```ts
export * from "./types.js";
export * from "./taskStore.js";
export * from "./reporting.js";
```

## Step 5 — `10/main.ts`

Now import from the **barrel only** and build a little scenario:

```ts
import {
  Priority,
  createTask,
  updateStatus,
  sortByPriority,
  summarise,
} from "./index.js";

let tasks = [
  createTask(1, "Write TS exercises", Priority.High),
  createTask(2, "Review PR", Priority.Medium),
  createTask(3, "Water plants", Priority.Low),
];

tasks = tasks.map((t) => (t.id === 1 ? updateStatus(t, "done") : t));

for (const t of sortByPriority(tasks)) {
  console.log(`[${t.priority}] ${t.title} — ${t.status}`);
}
console.log(summarise(tasks));
```

Run: `npx tsx 10/main.ts`

## Test

`10/capstone.test.ts`:

```ts
import assert from "node:assert/strict";
import {
  Priority,
  createTask,
  updateStatus,
  filterByStatus,
  sortByPriority,
  countByStatus,
  completionRate,
  summarise,
} from "./index.js";

const t1 = createTask(1, "A", Priority.Low);
assert.equal(t1.status, "todo");

// immutability: updateStatus must not change the original
const t1done = updateStatus(t1, "done");
assert.equal(t1done.status, "done");
assert.equal(t1.status, "todo");

const tasks = [
  updateStatus(createTask(1, "A", Priority.High), "done"),
  createTask(2, "B", Priority.Low),
  createTask(3, "C", Priority.Medium),
  updateStatus(createTask(4, "D", Priority.High), "done"),
];

assert.equal(filterByStatus(tasks, "done").length, 2);
assert.deepEqual(sortByPriority(tasks).map((t) => t.priority), [
  Priority.High,
  Priority.High,
  Priority.Medium,
  Priority.Low,
]);
assert.deepEqual(countByStatus(tasks), { "todo": 2, "in-progress": 0, "done": 2 });
assert.equal(completionRate(tasks), 0.5);
assert.equal(summarise(tasks), "4 tasks, 50% done");

console.log("✅ 10 passed — capstone complete! 🎉");
```

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 10/types.ts
export enum Priority {
  Low = "LOW",
  Medium = "MEDIUM",
  High = "HIGH",
}

export type TaskStatus = "todo" | "in-progress" | "done";

export interface Task {
  id: number;
  title: string;
  priority: Priority;
  status: TaskStatus;
}
```

```ts
// 10/taskStore.ts
import type { Task, TaskStatus } from "./types.js";
import { Priority } from "./types.js";

export function createTask(
  id: number,
  title: string,
  priority: Priority,
): Task {
  return { id, title, priority, status: "todo" };
}

export function updateStatus(task: Task, status: TaskStatus): Task {
  return { ...task, status };
}

export function filterByStatus(tasks: Task[], status: TaskStatus): Task[] {
  return tasks.filter((task) => task.status === status);
}

export function sortByPriority(tasks: Task[]): Task[] {
  const rank: Record<Priority, number> = {
    [Priority.High]: 0,
    [Priority.Medium]: 1,
    [Priority.Low]: 2,
  };
  return [...tasks].sort((a, b) => rank[a.priority] - rank[b.priority]);
}
```

```ts
// 10/reporting.ts
import type { Task, TaskStatus } from "./types.js";

export function countByStatus(tasks: Task[]): Record<TaskStatus, number> {
  const counts: Record<TaskStatus, number> = {
    "todo": 0,
    "in-progress": 0,
    "done": 0,
  };
  for (const task of tasks) {
    counts[task.status]++;
  }
  return counts;
}

export function completionRate(tasks: Task[]): number {
  if (tasks.length === 0) return 0;
  const done = tasks.filter((t) => t.status === "done").length;
  return done / tasks.length;
}

export function summarise(tasks: Task[]): string {
  const percent = Math.round(completionRate(tasks) * 100);
  return `${tasks.length} tasks, ${percent}% done`;
}
```

```ts
// 10/index.ts
export * from "./types.js";
export * from "./taskStore.js";
export * from "./reporting.js";
```

</details>

## Where to go next

You've now touched every core piece of everyday TypeScript. Good directions from
here: classes and access modifiers, utility types (`Partial`, `Pick`, `Omit`,
`Readonly`), `unknown` vs `any`, and wiring this up with a real test runner like
**Vitest** instead of hand-rolled asserts. Tell me which and I'll add a new set of
exercises to this same folder.
