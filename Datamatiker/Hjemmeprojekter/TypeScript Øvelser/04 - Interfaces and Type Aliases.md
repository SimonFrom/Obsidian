# 04 — Interfaces & Type Aliases

**Difficulty:** ⭐⭐

## Concepts you'll practise

- Defining a reusable data shape with `interface` and `type`
- **Sharing a type across files** by exporting it
- Functions in one file operating on a type defined in another

## Background

Instead of repeating `{ name: string; age: number }` everywhere, name it once:

```ts
interface Person {
  name: string;
  age: number;
}
// or, equivalently for object shapes:
type Person = {
  name: string;
  age: number;
};
```

Both can be exported and imported like functions:

```ts
// types.ts
export interface Book {
  title: string;
  author: string;
  year: number;
  pages: number;
}
```

```ts
// library.ts
import type { Book } from "./types.js";
```

> `import type { ... }` tells TypeScript you're only importing a type (it disappears
> at runtime). Plain `import { ... }` also works — `import type` is just clearer.

## Your task

Build a tiny "library" across three files.

**`04/types.ts`** — export an interface `Book`:

```ts
export interface Book {
  title: string;
  author: string;
  year: number;
  pages: number;
}
```

**`04/library.ts`** — import `Book` and export these functions:

1. `describeBook(book: Book): string` — returns
   `"<title> by <author> (<year>)"`.
2. `isLongBook(book: Book): boolean` — `true` if `pages > 400`.
3. `booksByAuthor(books: Book[], author: string): Book[]` — filters to that author.
4. `totalPages(books: Book[]): number` — sums the pages of all books.

**`04/main.ts`** — import both the type and the functions, make a couple of books,
and log some results.

## Test

`04/library.test.ts`:

```ts
import assert from "node:assert/strict";
import type { Book } from "./types.js";
import { describeBook, isLongBook, booksByAuthor, totalPages } from "./library.js";

const books: Book[] = [
  { title: "Clean Code", author: "Martin", year: 2008, pages: 464 },
  { title: "The Pragmatic Programmer", author: "Hunt", year: 1999, pages: 352 },
  { title: "Refactoring", author: "Fowler", year: 1999, pages: 448 },
];

assert.equal(describeBook(books[0]), "Clean Code by Martin (2008)");
assert.equal(isLongBook(books[0]), true);
assert.equal(isLongBook(books[1]), false);
assert.equal(booksByAuthor(books, "Fowler").length, 1);
assert.equal(totalPages(books), 1264);

console.log("✅ 04 passed");
```

## Interface vs type — which?

For plain object shapes they're nearly interchangeable. A common rule of thumb:
use `interface` for object/class shapes you might extend, and `type` when you need
unions or aliases (you'll hit those in exercise 05 and 08). Pick one style and stay
consistent.

## Solution

<details>
<summary>Click to reveal</summary>

```ts
// 04/types.ts
export interface Book {
  title: string;
  author: string;
  year: number;
  pages: number;
}
```

```ts
// 04/library.ts
import type { Book } from "./types.js";

export function describeBook(book: Book): string {
  return `${book.title} by ${book.author} (${book.year})`;
}

export function isLongBook(book: Book): boolean {
  return book.pages > 400;
}

export function booksByAuthor(books: Book[], author: string): Book[] {
  return books.filter((book) => book.author === author);
}

export function totalPages(books: Book[]): number {
  return books.reduce((total, book) => total + book.pages, 0);
}
```

```ts
// 04/main.ts
import type { Book } from "./types.js";
import { describeBook, totalPages } from "./library.js";

const books: Book[] = [
  { title: "Clean Code", author: "Martin", year: 2008, pages: 464 },
  { title: "Refactoring", author: "Fowler", year: 1999, pages: 448 },
];

console.log(describeBook(books[0]));
console.log(`Total pages: ${totalPages(books)}`);
```

</details>
