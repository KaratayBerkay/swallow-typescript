# TypeScript Deep Mastery — Structured Learning Path

> **Goal:** Go from TypeScript basics to professional-grade advanced typing.
>
> **Stack:** Node.js · pnpm · TypeScript 6 · ESM · strict mode enabled
>
> **Project config:** `tsconfig.json` is already set to strict with `noUncheckedIndexedAccess`,
> `exactOptionalPropertyTypes`, and `verbatimModuleSyntax` — this will challenge you more than the average tutorial.
>
> **Rule:** Every phase has a challenge. Do not move on until you complete it.

```md
[x] completed
[ ] not completed
```

---

## Environment

```bash
node -v       # v22+
pnpm -v       # v9+
pnpm exec tsc --version
```

Run files with:

```bash
pnpm exec tsx src/your-file.ts
```

---

# PHASE 1 — Primitive Types & Type Annotations

## Covered

| Type | Example |
|------|---------|
| `string` | `let name: string = "Berkay"` |
| `number` | `let age: number = 25` |
| `boolean` | `let active: boolean = true` |
| `bigint` | `let big: bigint = 9007199254740993n` |
| `symbol` | `let id: symbol = Symbol("id")` |
| `null` | `let empty: null = null` |
| `undefined` | `let missing: undefined = undefined` |

## Key Concepts

**Type annotation vs inference:**

```ts
let x: number = 5;  // explicit annotation
let y = 5;          // inferred as number — prefer this when obvious
```

**`null` and `undefined` are NOT the same:**

```ts
let a: string | null = null;       // intentionally absent
let b: string | undefined = undefined; // not yet assigned
```

With `strictNullChecks` (enabled), you cannot assign `null` to a `string` variable directly.

## Checklist

- [ ] All seven primitives written and compiled
- [ ] Understand when to annotate vs let TS infer
- [ ] Hit a `null` type error on purpose, then fix it

## Challenge

Write a function `describe(value: string | number | boolean | null): string` that returns a different string for each possible type including `null`. No `any` allowed.

---

# PHASE 2 — Arrays, Tuples & Readonly

## Arrays

```ts
let temps: number[] = [20, 21, 25];
let names: Array<string> = ["Ali", "Berkay"];
let matrix: number[][] = [[1, 2], [3, 4]];
```

## Tuples

A tuple is a fixed-length array where each position has a known type.

```ts
type RGB = [number, number, number];
let red: RGB = [255, 0, 0];

type Entry = [id: number, label: string, active: boolean];
let entry: Entry = [1, "sensor-A", true];
```

**Labeled tuples** (above) improve readability in editor hints.

## Readonly arrays

```ts
const config: readonly string[] = ["prod", "staging"];
// config.push("dev"); // Error — readonly
```

## Rest in tuples

```ts
type AtLeastOne = [string, ...number[]];
let data: AtLeastOne = ["header", 1, 2, 3];
```

## Checklist

- [ ] Typed arrays and nested arrays
- [ ] Named tuples with at least 3 fields
- [ ] `readonly` array — trigger and handle the error
- [ ] Rest element in a tuple

## Challenge

Model an HTTP request log entry as a labeled tuple: `[timestamp: number, method: string, path: string, status: number, duration: number]`. Write a function that takes this tuple and returns a formatted log string.

---

# PHASE 3 — Objects, Type Aliases & Interfaces

## Inline object types

```ts
let sensor: { name: string; value: number } = { name: "TMP36", value: 24.5 };
```

## Type Aliases

```ts
type Sensor = {
  name: string;
  value: number;
  unit?: string;       // optional
  readonly id: number; // cannot be reassigned after creation
};
```

## Interfaces

```ts
interface Device {
  id: number;
  active: boolean;
  ping(): boolean;
}
```

## Type vs Interface — key differences

| Feature | `type` | `interface` |
|---------|--------|-------------|
| Can extend | via `&` intersection | via `extends` |
| Declaration merging | No | Yes |
| Can alias primitives/unions | Yes | No |
| Computed properties | Yes | No |

Use `interface` for object shapes that may be extended. Use `type` for unions, intersections, and complex computed types.

## Extending

```ts
interface BaseDevice {
  id: number;
}

interface SmartDevice extends BaseDevice {
  firmware: string;
}

// Type intersection equivalent:
type SmartSensor = Sensor & { firmware: string };
```

## Index signatures

When keys are dynamic:

```ts
interface Registry {
  [deviceId: string]: Device;
}
```

## Checklist

- [ ] Type alias with optional and readonly fields
- [ ] Interface with a method signature
- [ ] `extends` on an interface
- [ ] `&` intersection on a type
- [ ] Index signature
- [ ] Trigger a type error by assigning an incompatible shape

## Challenge

Model a `Fleet` system: a `Vehicle` interface with `id`, `make`, `model`, `year`. A `ElectricVehicle` that extends `Vehicle` and adds `batteryKwh` and `rangeKm`. A `Fleet` type that maps `string` keys to `Vehicle`. Write a function `addVehicle(fleet: Fleet, v: Vehicle): Fleet`.

---

# PHASE 4 — Functions, Overloads & Signatures

## Typed functions

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

## Function types

```ts
type BinaryOp = (a: number, b: number) => number;

const multiply: BinaryOp = (a, b) => a * b;
```

## Optional, default, rest parameters

```ts
function greet(name: string, title?: string): string {
  return title ? `${title} ${name}` : name;
}

function power(base: number, exp: number = 2): number {
  return base ** exp;
}

function sum(...values: number[]): number {
  return values.reduce((acc, v) => acc + v, 0);
}
```

## Function overloads

Used when a function behaves differently based on argument types:

```ts
function parse(input: string): number;
function parse(input: number): string;
function parse(input: string | number): string | number {
  if (typeof input === "string") return parseInt(input, 10);
  return input.toString();
}
```

The overload signatures define the public contract. The implementation signature is internal.

## `never` return type

```ts
function panic(msg: string): never {
  throw new Error(msg);
}
```

`never` means the function never returns normally — it throws or loops forever.

## Checklist

- [ ] Function type alias used as a parameter
- [ ] Rest parameters
- [ ] Overloaded function with 2 signatures
- [ ] A function that returns `never`

## Challenge

Write a `format` function with overloads: `format(value: number, decimals: number): string`, `format(value: Date): string`, `format(value: boolean): string`. All three overload signatures, one implementation.

---

# PHASE 5 — Union Types, Intersection & Literal Types

## Union types

```ts
let id: string | number;

function printId(id: string | number): void {
  console.log(id.toString());
}
```

## Literal types

```ts
type Direction = "north" | "south" | "east" | "west";
type StatusCode = 200 | 201 | 400 | 401 | 404 | 500;
type Bit = 0 | 1;
```

## Template literal types

```ts
type EventName = `on${Capitalize<string>}`;
type CSSUnit = `${number}px` | `${number}em` | `${number}rem`;
type Endpoint = `/api/${string}`;
```

## Intersection types

```ts
type Timestamped = { createdAt: Date; updatedAt: Date };
type Named = { name: string };
type TimestampedEntity = Named & Timestamped;
```

## Checklist

- [ ] Union of 3+ types
- [ ] Literal union for a finite set of values
- [ ] Template literal type
- [ ] Intersection type combining two shapes

## Challenge

Define a `LogLevel` literal union: `"debug" | "info" | "warn" | "error" | "fatal"`. Create a `LogEntry` type using intersection: combine a `{ level: LogLevel; message: string }` with a `Timestamped` type. Write a `log(entry: LogEntry): string` function.

---

# PHASE 6 — Type Narrowing & Type Guards

## `typeof` narrowing

```ts
function process(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase(); // TS knows it's string here
  }
  return value * 2;
}
```

## `instanceof` narrowing

```ts
function handleError(err: unknown) {
  if (err instanceof Error) {
    console.log(err.message);
  }
}
```

## `in` operator narrowing

```ts
type Cat = { meow(): void };
type Dog = { bark(): void };

function makeSound(animal: Cat | Dog) {
  if ("meow" in animal) {
    animal.meow();
  } else {
    animal.bark();
  }
}
```

## Custom type guards

```ts
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

The `value is string` predicate tells TypeScript to narrow to `string` in the branch where this returns `true`.

## Assertion functions

```ts
function assertDefined<T>(value: T | null | undefined): asserts value is T {
  if (value == null) throw new Error("Expected a value");
}
```

## Checklist

- [ ] `typeof` guard for 3+ types
- [ ] `instanceof` guard
- [ ] `in` operator guard
- [ ] Custom type guard with `is` predicate
- [ ] Assertion function

## Challenge

Write a `parseConfig(raw: unknown)` function that validates and narrows `unknown` to a `{ host: string; port: number; debug: boolean }` shape. Use `in` operator checks and custom type guards. Throw a descriptive error if validation fails.

---

# PHASE 7 — Enums

## Numeric enum

```ts
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right, // 3
}
```

## String enum

```ts
enum Status {
  Idle = "IDLE",
  Running = "RUNNING",
  Error = "ERROR",
}
```

Prefer string enums in practice — they produce readable output and don't have reverse-mapping surprises.

## Const enum

```ts
const enum HttpMethod {
  GET = "GET",
  POST = "POST",
  PUT = "PUT",
  DELETE = "DELETE",
}
```

`const enum` is inlined at compile time — no runtime object generated. Use when you don't need to iterate over enum values.

## Enum vs union literal — when to choose

| | `enum` | literal union |
|---|---|---|
| Runtime object | Yes | No |
| Iterable | Yes | No |
| IDE completion | Both | Both |
| Recommended for | State machines, flags | Most other cases |

## Checklist

- [ ] Numeric enum
- [ ] String enum
- [ ] `const enum`
- [ ] Used enum as a function parameter type

## Challenge

Build a `TrafficLight` state machine using a string enum for states (`Red`, `Yellow`, `Green`) and a function `next(state: TrafficLight): TrafficLight` that returns the correct next state in the cycle.

---

# PHASE 8 — Generics

## Generic functions

```ts
function identity<T>(value: T): T {
  return value;
}

function first<T>(arr: T[]): T | undefined {
  return arr[0];
}
```

## Multiple type parameters

```ts
function pair<A, B>(a: A, b: B): [A, B] {
  return [a, b];
}
```

## Generic interfaces

```ts
interface ApiResponse<T> {
  data: T;
  success: boolean;
  error?: string;
}

interface Repository<T, ID> {
  findById(id: ID): Promise<T | null>;
  save(entity: T): Promise<T>;
  delete(id: ID): Promise<void>;
}
```

## Generic type aliases

```ts
type Nullable<T> = T | null;
type Maybe<T> = T | null | undefined;
type Result<T, E = Error> = { ok: true; value: T } | { ok: false; error: E };
```

## Generic classes

```ts
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }
}
```

## Checklist

- [ ] Generic function with 1 type param
- [ ] Generic function with 2 type params
- [ ] Generic interface used with 2+ concrete types
- [ ] `Result<T, E>` type alias
- [ ] Generic class

## Challenge

Implement a generic `Cache<K, V>` class with `set(key: K, value: V): void`, `get(key: K): V | undefined`, `has(key: K): boolean`, and `invalidate(key: K): void`. Then write a `memoize<T extends (...args: unknown[]) => unknown>(fn: T): T` function (type the wrapper correctly).

---

# PHASE 9 — Generic Constraints (`extends`)

## Basic constraint

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

`K extends keyof T` means `K` must be a key of `T`. The return type `T[K]` is the exact type of that property — this is **indexed access typing**.

## Constrained to an interface

```ts
interface HasId {
  id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}
```

## Default type parameters

```ts
interface Pagination<T, Meta = Record<string, unknown>> {
  items: T[];
  total: number;
  meta: Meta;
}
```

## Conditional constraints

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
```

## Real-world example

```ts
function merge<T extends object, U extends object>(a: T, b: U): T & U {
  return { ...a, ...b };
}
```

## Checklist

- [ ] `K extends keyof T` with indexed access
- [ ] `T extends SomeInterface` constraint
- [ ] Default type parameter
- [ ] `T extends null | undefined ? never : T` pattern

## Challenge

Write a `pick<T, K extends keyof T>(obj: T, keys: K[]): Pick<T, K>` function from scratch without using `Pick` from the standard library in the implementation. Then write `omit<T, K extends keyof T>(obj: T, keys: K[]): Omit<T, K>`.

---

# PHASE 10 — Utility Types

All built-in, no need to import. Understand each by reimplementing it yourself.

| Utility | What it does |
|---------|-------------|
| `Partial<T>` | All properties optional |
| `Required<T>` | All properties required |
| `Readonly<T>` | All properties readonly |
| `Pick<T, K>` | Keep only keys `K` |
| `Omit<T, K>` | Remove keys `K` |
| `Record<K, V>` | Object type with keys `K` and values `V` |
| `Exclude<T, U>` | Remove from union `T` any types in `U` |
| `Extract<T, U>` | Keep from union `T` only types in `U` |
| `NonNullable<T>` | Remove `null` and `undefined` from `T` |
| `ReturnType<F>` | Infer return type of function `F` |
| `Parameters<F>` | Infer parameter types of function `F` |
| `InstanceType<C>` | Infer instance type of constructor `C` |
| `Awaited<T>` | Unwrap a `Promise<T>` to `T` |

## Examples

```ts
type User = { id: number; name: string; email: string };

type UserPreview = Pick<User, "id" | "name">;
type CreateUser = Omit<User, "id">;
type PatchUser = Partial<CreateUser>;

type StringOrNumber = string | number | boolean;
type OnlyStrNum = Exclude<StringOrNumber, boolean>; // string | number

async function fetchUser(): Promise<User> { /* ... */ return {} as User; }
type FetchResult = Awaited<ReturnType<typeof fetchUser>>; // User
```

## Checklist

- [ ] `Partial`, `Required`, `Readonly` applied to same type
- [ ] `Pick` and `Omit` on a complex type
- [ ] `Record` to build a lookup map
- [ ] `Exclude` and `Extract` on a union
- [ ] `ReturnType` and `Parameters` extracted from a function
- [ ] `Awaited` to unwrap a promise type

## Challenge

Given:

```ts
type Config = {
  host: string;
  port: number;
  debug: boolean;
  secret: string;
  apiKey: string;
};
```

Create: `PublicConfig` (omit `secret` and `apiKey`), `OptionalConfig` (all optional), `ReadonlyConfig` (all readonly), and a `ConfigMap` type that maps `string` keys to `Config`. All without `any`.

---

# PHASE 11 — Mapped Types

Mapped types iterate over keys and transform them.

## Basic mapped type

```ts
type Optional<T> = {
  [K in keyof T]?: T[K];
};
```

## Modifiers: `+`, `-`

```ts
// Remove readonly from all props
type Mutable<T> = {
  -readonly [K in keyof T]: T[K];
};

// Remove optional from all props
type Concrete<T> = {
  [K in keyof T]-?: T[K];
};
```

## Key remapping with `as`

```ts
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type UserGetters = Getters<{ name: string; age: number }>;
// { getName: () => string; getAge: () => number }
```

## Filtering keys with `never`

```ts
type OnlyStrings<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K];
};
```

## Checklist

- [ ] Implement `Partial<T>` and `Required<T>` manually using mapped types
- [ ] Implement `Readonly<T>` and `Mutable<T>` manually
- [ ] Use `as` clause to remap keys
- [ ] Filter keys using `never` in a mapped type

## Challenge

Implement `Setters<T>` — a mapped type that produces `{ setName: (v: string) => void; setAge: (v: number) => void }` from `{ name: string; age: number }`. Then combine it with `Getters<T>` into an `Accessors<T>` type.

---

# PHASE 12 — Conditional Types

```ts
type IsArray<T> = T extends unknown[] ? true : false;
type IsString<T> = T extends string ? true : false;
```

## Distributive conditional types

When `T` is a union, conditional types distribute over it:

```ts
type ToArray<T> = T extends unknown ? T[] : never;
type Result = ToArray<string | number>; // string[] | number[]
```

## `infer` — extracting types

```ts
type UnpackArray<T> = T extends (infer Item)[] ? Item : T;
type UnpackPromise<T> = T extends Promise<infer R> ? R : T;

type ReturnType<F> = F extends (...args: unknown[]) => infer R ? R : never;
type FirstParam<F> = F extends (first: infer P, ...rest: unknown[]) => unknown ? P : never;
```

`infer` introduces a new type variable in the `extends` clause that TS fills in.

## Recursive conditional types

```ts
type DeepReadonly<T> = T extends object
  ? { readonly [K in keyof T]: DeepReadonly<T[K]> }
  : T;

type Flatten<T> = T extends Array<infer Item> ? Flatten<Item> : T;
```

## Checklist

- [ ] Simple conditional type
- [ ] Distributive conditional type over a union
- [ ] `infer` to extract array element type
- [ ] `infer` to extract promise return type
- [ ] Recursive conditional type

## Challenge

Implement `DeepPartial<T>` — like `Partial<T>` but recursively makes nested objects partial too. Test it with a deeply nested config object.

---

# PHASE 13 — Discriminated Unions & Exhaustiveness

Discriminated unions use a shared literal field to discriminate between variants.

## Pattern

```ts
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "rectangle"; width: number; height: number }
  | { kind: "triangle"; base: number; height: number };
```

## Exhaustive switch

```ts
function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "rectangle":
      return shape.width * shape.height;
    case "triangle":
      return (shape.base * shape.height) / 2;
    default:
      // This line makes the check exhaustive:
      const _exhaustive: never = shape;
      throw new Error(`Unhandled shape: ${JSON.stringify(_exhaustive)}`);
  }
}
```

If you add a new variant to `Shape` and forget to handle it in the `switch`, TS will error on the `_exhaustive: never` line.

## Result type pattern

```ts
type Ok<T> = { ok: true; value: T };
type Err<E> = { ok: false; error: E };
type Result<T, E = Error> = Ok<T> | Err<E>;

function divide(a: number, b: number): Result<number, string> {
  if (b === 0) return { ok: false, error: "Division by zero" };
  return { ok: true, value: a / b };
}
```

## Checklist

- [ ] Discriminated union with 3+ variants
- [ ] Exhaustive switch with `never` check
- [ ] `Result<T, E>` type used in real code
- [ ] Add a new variant and watch the compiler force you to handle it

## Challenge

Model an async task state machine with these states: `{ status: "idle" }`, `{ status: "pending"; startedAt: number }`, `{ status: "success"; data: unknown; duration: number }`, `{ status: "error"; error: string; retries: number }`. Write a `transition(state: TaskState, event: TaskEvent): TaskState` function where `TaskEvent` is also a discriminated union.

---

# PHASE 14 — Classes in Depth

## Access modifiers

```ts
class Sensor {
  public name: string;
  private _value: number;
  protected unit: string;

  constructor(name: string, value: number, unit: string) {
    this.name = name;
    this._value = value;
    this.unit = unit;
  }

  get value(): number {
    return this._value;
  }

  set value(v: number) {
    if (v < 0) throw new RangeError("Value cannot be negative");
    this._value = v;
  }
}
```

**Shorthand constructor params:**

```ts
class Point {
  constructor(
    public readonly x: number,
    public readonly y: number,
  ) {}
}
```

## Abstract classes

```ts
abstract class Transport {
  abstract send(data: Uint8Array): Promise<void>;
  abstract close(): void;

  protected log(msg: string): void {
    console.log(`[${this.constructor.name}] ${msg}`);
  }
}

class UartTransport extends Transport {
  async send(data: Uint8Array): Promise<void> {
    this.log(`sending ${data.length} bytes`);
  }

  close(): void {
    this.log("closed");
  }
}
```

## Implementing interfaces

```ts
interface Serializable {
  serialize(): string;
  deserialize(data: string): void;
}

class Config implements Serializable {
  private data: Record<string, unknown> = {};

  serialize(): string {
    return JSON.stringify(this.data);
  }

  deserialize(raw: string): void {
    this.data = JSON.parse(raw) as Record<string, unknown>;
  }
}
```

## Static members

```ts
class IdGenerator {
  private static counter = 0;

  static next(): number {
    return ++IdGenerator.counter;
  }

  static reset(): void {
    IdGenerator.counter = 0;
  }
}
```

## Generic classes

```ts
class TypedEventEmitter<EventMap extends Record<string, unknown>> {
  private listeners = new Map<keyof EventMap, Array<(data: unknown) => void>>();

  on<K extends keyof EventMap>(event: K, handler: (data: EventMap[K]) => void): void {
    const existing = this.listeners.get(event) ?? [];
    this.listeners.set(event, [...existing, handler as (data: unknown) => void]);
  }

  emit<K extends keyof EventMap>(event: K, data: EventMap[K]): void {
    this.listeners.get(event)?.forEach(fn => fn(data));
  }
}
```

## Checklist

- [ ] Class with `public`, `private`, `protected`, `readonly`
- [ ] Getters and setters with validation
- [ ] Constructor shorthand
- [ ] Abstract class with abstract methods
- [ ] Class that `implements` an interface
- [ ] Static factory method
- [ ] Generic class

## Challenge

Build a generic `EventBus<EventMap>` using the `TypedEventEmitter` pattern above. Then create a `DeviceEventMap` interface and wire up at least 3 typed events. The event bus should also support `off(event, handler)` and `once(event, handler)`.

---

# PHASE 15 — Modules & Declaration Files

## ESM modules (this project uses `"type": "module"`)

```ts
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export type BinaryOp = (a: number, b: number) => number;
```

```ts
// main.ts
import { add } from "./math.js"; // Note: .js extension required in ESM
import type { BinaryOp } from "./math.js";
```

`import type` — enforced by `verbatimModuleSyntax` in this project. Type-only imports must use `import type`.

## Re-exporting

```ts
// index.ts
export { add, subtract } from "./math.js";
export type { BinaryOp } from "./math.js";
export * from "./utils.js";
```

## Declaration files (`.d.ts`)

Used to type a JavaScript library that has no types:

```ts
// legacy-lib.d.ts
declare module "legacy-lib" {
  export function init(config: { url: string }): void;
  export function destroy(): void;
}
```

## Ambient declarations

```ts
declare global {
  interface Window {
    analytics: {
      track(event: string, data?: Record<string, unknown>): void;
    };
  }
}
```

## Checklist

- [ ] Multi-file project with imports/exports
- [ ] `import type` used for type-only imports
- [ ] Barrel `index.ts` re-exporting from multiple files
- [ ] A `.d.ts` declaration file for a mock JS library
- [ ] `declare global` augmentation

## Challenge

Structure your project with: `src/types/index.ts` (all shared types), `src/utils/` (utility functions), `src/core/` (business logic). Create a barrel for each folder. Import only through the barrel in `main.ts`.

---

# PHASE 16 — Async TypeScript

## Promise typing

```ts
async function fetchUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  return response.json() as Promise<User>;
}
```

## Error handling with Result

```ts
async function safe<T>(promise: Promise<T>): Promise<Result<T, Error>> {
  try {
    return { ok: true, value: await promise };
  } catch (err) {
    return { ok: false, error: err instanceof Error ? err : new Error(String(err)) };
  }
}
```

## Promise combinators

```ts
// All must succeed
const [user, posts] = await Promise.all([fetchUser(1), fetchPosts(1)]);

// First to settle wins
const result = await Promise.race([fetchWithTimeout(url, 5000), timeout(5000)]);

// All settle (no throw)
const results = await Promise.allSettled([fetch("/a"), fetch("/b")]);
```

## Generator-based async patterns

```ts
async function* paginate<T>(
  fetcher: (page: number) => Promise<T[]>,
): AsyncGenerator<T> {
  let page = 0;
  while (true) {
    const items = await fetcher(page++);
    if (items.length === 0) break;
    yield* items;
  }
}
```

## Checklist

- [ ] `async` function with explicit `Promise<T>` return type
- [ ] `safe()` wrapper using `Result<T, E>`
- [ ] `Promise.all` with typed destructuring
- [ ] `Promise.allSettled` with result handling
- [ ] Async generator function

## Challenge

Write a `retry<T>(fn: () => Promise<T>, attempts: number, delay: number): Promise<T>` function that retries `fn` up to `attempts` times with `delay` ms between attempts. Type everything precisely, no `any`.

---

# PHASE 17 — `keyof`, `typeof` & Template Literal Types

## `keyof`

```ts
type UserKeys = keyof User; // "id" | "name" | "email"

function pluck<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

## `typeof`

```ts
const defaultConfig = { host: "localhost", port: 3000, debug: false };
type Config = typeof defaultConfig;
// { host: string; port: number; debug: boolean }

const routes = ["home", "about", "contact"] as const;
type Route = (typeof routes)[number]; // "home" | "about" | "contact"
```

## `as const`

```ts
const STATUS = {
  OK: 200,
  NOT_FOUND: 404,
  ERROR: 500,
} as const;

type StatusCode = (typeof STATUS)[keyof typeof STATUS]; // 200 | 404 | 500
```

## Template literal types (advanced)

```ts
type EventName<T extends string> = `${T}Changed` | `${T}Added` | `${T}Removed`;
type UserEvents = EventName<"user">; // "userChanged" | "userAdded" | "userRemoved"

type PropPath<T, K extends keyof T = keyof T> =
  K extends string
    ? T[K] extends Record<string, unknown>
      ? `${K}.${PropPath<T[K]>}` | K
      : K
    : never;
```

## Checklist

- [ ] `keyof` used in a generic function
- [ ] `typeof` to derive a type from a value
- [ ] `as const` to narrow to literal types
- [ ] `(typeof arr)[number]` to get element type
- [ ] Template literal type with a generic parameter

## Challenge

Create an `EventEmitter<T>` where `T` is an object mapping event names to their payload types. Use template literal types so that calling `.on("userChanged", handler)` is typed, and `handler` receives the correct payload type from `T["userChanged"]`.

---

# PHASE 18 — Decorators

Decorators are experimental but widely used in frameworks like NestJS, TypeORM, and Angular.

Enable in tsconfig: `"experimentalDecorators": true` (or use TC39 stage-3 decorators in TS 5+).

## Class decorator

```ts
function Singleton<T extends { new (...args: unknown[]): unknown }>(ctor: T) {
  let instance: InstanceType<T>;
  return class extends ctor {
    constructor(...args: unknown[]) {
      if (instance) return instance;
      super(...args);
      instance = this as InstanceType<T>;
    }
  };
}
```

## Method decorator

```ts
function Log(target: object, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value as (...args: unknown[]) => unknown;
  descriptor.value = function (...args: unknown[]) {
    console.log(`Calling ${key} with`, args);
    return original.apply(this, args);
  };
  return descriptor;
}
```

## Parameter decorator (metadata)

```ts
function Validate(target: object, method: string, index: number) {
  // Record which parameter index requires validation
  const meta: number[] = Reflect.getMetadata("validate", target, method) ?? [];
  meta.push(index);
  Reflect.defineMetadata("validate", meta, target, method);
}
```

## Checklist

- [ ] Class decorator
- [ ] Method decorator that wraps behavior
- [ ] Property decorator
- [ ] Decorator factory (a function that returns a decorator)

## Challenge

Build a `@Memoize()` method decorator that caches the result of a method based on its arguments. The cache should be per-instance. Properly type the decorator so it only applies to methods.

---

# PHASE 19 — Strict Mode & Compiler Flags Deep Dive

This project already has a strict config. Understand what each flag does.

| Flag | What it enforces |
|------|-----------------|
| `strict` | Enables all strict checks below |
| `strictNullChecks` | `null` and `undefined` are not subtypes of every type |
| `noImplicitAny` | Error when type is implicitly `any` |
| `strictFunctionTypes` | Function parameter types checked contravariantly |
| `noUncheckedIndexedAccess` | Array/object index access returns `T \| undefined` |
| `exactOptionalPropertyTypes` | `{ x?: string }` means `x` can be `string` or absent, not `undefined` |
| `noImplicitOverride` | Must use `override` keyword when overriding |

## `noUncheckedIndexedAccess` in practice

```ts
const arr = [1, 2, 3];
const first = arr[0]; // type: number | undefined — must check before use
if (first !== undefined) {
  console.log(first * 2); // safe
}
```

## `exactOptionalPropertyTypes` in practice

```ts
interface Config {
  debug?: boolean;
}

const a: Config = {};           // ok — debug absent
const b: Config = { debug: true }; // ok
// const c: Config = { debug: undefined }; // Error — exactOptionalPropertyTypes
```

## Checklist

- [ ] Understand all 7 flags in the table
- [ ] Write code that would fail with `noUncheckedIndexedAccess` and fix it
- [ ] Write code that would fail with `exactOptionalPropertyTypes` and fix it
- [ ] Write a class with `override` keyword

---

# PHASE 20 — Real Project: Typed Event-Driven Architecture

Combine everything from all previous phases into one system.

## Project spec

Build a minimal in-memory message bus for a telemetry system.

**Requirements:**

- `EventBus<EventMap>` — fully typed, generic
- `Device` hierarchy — abstract base, concrete implementations
- At least 3 discriminated union state types
- Generic `Repository<T, ID>` with in-memory implementation
- A `Result<T, E>` return type throughout — no raw `throw` from public APIs
- `Partial` / `Readonly` / `Pick` used at least once each
- All `async` functions return `Promise<Result<T, E>>`
- No `any` anywhere

**Suggested structure:**

```
src/
  types/
    index.ts        ← shared types, interfaces, enums
  core/
    event-bus.ts    ← generic EventBus<EventMap>
    repository.ts   ← generic Repository<T, ID>
    result.ts       ← Result<T, E> helpers
  devices/
    base.ts         ← abstract Device
    sensor.ts       ← Sensor extends Device
    controller.ts   ← Controller extends Device
  telemetry/
    collector.ts    ← collects readings via EventBus
    store.ts        ← uses Repository to persist
  main.ts
```

## Checklist

- [ ] Compiles with zero errors under the current strict tsconfig
- [ ] No `any` — verified with `grep -r 'any' src/`
- [ ] Discriminated unions with exhaustive handling
- [ ] Generic repository with 2+ concrete uses
- [ ] Async throughout with `Result<T, E>`

---

# PHASE 21 — Advanced Patterns Reference

Patterns you will encounter in real codebases. Study once you finish Phase 20.

## Builder pattern

```ts
class QueryBuilder<T> {
  private conditions: string[] = [];

  where(condition: string): this {
    this.conditions.push(condition);
    return this;
  }

  build(): string {
    return `SELECT * FROM table WHERE ${this.conditions.join(" AND ")}`;
  }
}
```

## Fluent API with method chaining

Return `this` for chainable methods. With generics, `this` preserves the subclass type.

## Branded types (nominal typing)

TypeScript uses structural typing. Branded types prevent mixing up values of the same underlying type:

```ts
type UserId = number & { readonly _brand: "UserId" };
type OrderId = number & { readonly _brand: "OrderId" };

function createUserId(n: number): UserId {
  return n as UserId;
}

function fetchUser(id: UserId): Promise<User> { /* ... */ }

const orderId = createOrderId(42);
// fetchUser(orderId); // Error — OrderId is not UserId
```

## Opaque types with `unique symbol`

```ts
declare const _brand: unique symbol;
type Branded<T, Brand> = T & { readonly [_brand]: Brand };
```

## Variance and `in`/`out` modifiers (TS 4.7+)

```ts
interface Producer<out T> {
  produce(): T;
}

interface Consumer<in T> {
  consume(value: T): void;
}
```

## Checklist

- [ ] Builder pattern with `this` return type
- [ ] Branded type for at least 2 domain IDs
- [ ] Understand why structural typing causes branded types to be needed

---

# DAILY PRACTICE METHOD

1. **Read** — one phase section.
2. **Type** — write every example by hand, no copy-paste.
3. **Break** — introduce a type error intentionally, read the error message fully.
4. **Fix** — resolve the error without looking at the original.
5. **Challenge** — complete the phase challenge before moving on.

> TypeScript errors are compiler feedback, not failures.
> A type error means TypeScript caught a mistake before it reached production.
> That is the entire point.

---

# RESOURCES

| Resource | URL |
|----------|-----|
| Official Handbook | https://www.typescriptlang.org/docs/handbook/intro.html |
| TS Playground | https://www.typescriptlang.org/play |
| Type Challenges | https://github.com/type-challenges/type-challenges |
| Matt Pocock's Total TypeScript | https://www.totaltypescript.com |
| Effective TypeScript (book) | by Dan Vanderkam |

---

# FINAL GOAL

When you finish this roadmap you will be able to:

- Design complex type systems from scratch
- Use generics with constraints across classes, functions, and interfaces
- Build discriminated union state machines with exhaustive handling
- Write zero-`any` TypeScript in production codebases
- Read and understand library `.d.ts` files
- Type async code with precise `Result<T, E>` error handling
- Parse and type binary protocols and hardware interfaces
