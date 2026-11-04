# TypeScript Øvelser — Start Here

A hands-on path to level up your TypeScript, starting gentle and getting harder.
A running theme throughout: **splitting functions across files** with `export` /
`import`, because that's a core skill for real projects.

Work through the exercises in order. Each one builds on the last.

## The path

| # | Exercise | You'll practise |
|---|----------|-----------------|
| 01 | Basic types & your first typed function | type annotations, return types, `tsc`/`tsx` |
| 02 | Functions across files (import & export) | ES modules, named vs default exports |
| 03 | Arrays & objects | typing arrays, object shapes, inline types |
| 04 | Interfaces & type aliases | modelling data, sharing types across files |
| 05 | Union types, optional & default params | `|`, `?`, default values, narrowing |
| 06 | Higher-order functions | functions as arguments, `map`/`filter`/`reduce` |
| 07 | Generics | reusable typed functions, `<T>` |
| 08 | Discriminated unions & enums | modelling states, exhaustive `switch` |
| 09 | Async functions & Promises | `async`/`await`, typing async results |
| 10 | Capstone project | everything together, multi-file, barrel exports |

## One-time setup

You said you'll create the code files yourself — here's the quickest modern way
to run them. From a terminal in the `TypeScript Øvelser` folder:

```bash
# 1. Create a project (once)
npm init -y

# 2. Install TypeScript + tsx (runs .ts files directly, no compile step)
npm install -D typescript tsx

# 3. Create a tsconfig
npx tsc --init
```

In the generated `tsconfig.json`, make sure these are set so modules work cleanly:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

> **Tip:** `"strict": true` is where the real learning happens — it forces you to
> handle types properly. Keep it on.

## How to run an exercise

Each exercise tells you which files to create. To run one:

```bash
npx tsx 01/main.ts
```

## How the tests work

Every exercise includes a **test file** you can copy into that exercise's folder
(e.g. `01/main.test.ts`). Tests use Node's built-in assertion library — no extra
dependencies. Run a test like this:

```bash
npx tsx 01/main.test.ts
```

If it prints nothing and exits cleanly, everything passed. If an assertion fails,
you'll get an `AssertionError` telling you exactly what didn't match.

Each exercise also has a **Solution** section at the bottom, hidden inside a
collapsible block. Try hard first, then peek.

## Suggested folder layout

```
TypeScript Øvelser/
├── package.json
├── tsconfig.json
├── 01/
│   ├── main.ts
│   └── main.test.ts
├── 02/
│   ├── mathUtils.ts
│   ├── main.ts
│   └── main.test.ts
└── ...
```

Ready? Open **01 - Basic Types.md**.
