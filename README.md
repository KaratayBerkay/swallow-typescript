# TypeScript Master Roadmap — Complete Learning Path

> Purpose:
> Learn TypeScript from beginner → advanced → professional ecosystem usage.
>
> Philosophy:
>
> - Learn gradually
> - Build constantly
> - Break code intentionally
> - Fix errors yourself
> - Focus on practical understanding
>
> Completion Rule:
>
> ```md
> [x] completed
> [ ] not completed
> ```

---

# PHASE 0 — ENVIRONMENT SETUP

## Install Node.js

Download:

https://nodejs.org

Check:

```bash
node -v
npm -v
```

Checklist:

- [ ] Node.js installed
- [ ] npm working

---

## Install TypeScript

```bash
pnpm add -D typescript
```

```bash
pnpm add -D @types/node
```

Check:

```bash
pnpm exec tsc --init
```

Checklist:

- [ ] TypeScript installed
- [ ] tsc command works

---

# PHASE 1 — FIRST TYPESCRIPT PROJECT

## Create Project

```bash
mkdir ts-learning
cd ts-learning
```

---

## Initialize npm

```bash
npm init -y
```

---

## Create tsconfig

```bash
tsc --init
```

Checklist:

- [ ] package.json created
- [ ] tsconfig.json created

---

# PHASE 2 — YOUR FIRST TS FILE

Create:

```text
index.ts
```

Code:

```ts
let username: string = "Mehmet";

console.log(username);
```

Compile:

```bash
tsc
```

Run:

```bash
node index.js
```

Checklist:

- [ ] First TS file created
- [ ] TS compiled successfully
- [ ] JS executed successfully

---

# PHASE 3 — BASIC TYPES

---

# String

```ts
let city: string = "Ankara";
```

---

# Number

```ts
let age: number = 60;
```

---

# Boolean

```ts
let retired: boolean = true;
```

---

# Arrays

```ts
let temperatures: number[] = [20, 21, 25];
```

---

# Objects

```ts
let board: {
  name: string;
  cores: number;
} = {
  name: "STM32",
  cores: 2,
};
```

Checklist:

- [ ] string
- [ ] number
- [ ] boolean
- [ ] arrays
- [ ] objects

---

# PHASE 4 — FUNCTIONS

---

# Typed Functions

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

---

# Void Functions

```ts
function logMessage(msg: string): void {
  console.log(msg);
}
```

---

# Optional Parameters

```ts
function greet(name: string, title?: string) {
  console.log(title ? `${title} ${name}` : name);
}
```

---

# Default Parameters

```ts
function power(value: number, exp: number = 2) {
  return value ** exp;
}
```

Checklist:

- [ ] typed functions
- [ ] return types
- [ ] void
- [ ] optional parameters
- [ ] default parameters

---

# PHASE 5 — TYPE INFERENCE

```ts
let temp = 35;
```

TypeScript infers:

```ts
number;
```

Checklist:

- [ ] understand inference

---

# PHASE 6 — IMPORTANT TYPES

---

# any

```ts
let value: any = 5;
```

---

# unknown

```ts
let value: unknown;
```

---

# union types

```ts
let id: string | number;
```

---

# literal types

```ts
let mode: "auto" | "manual";
```

---

# tuple types

```ts
let rgb: [number, number, number] = [255, 0, 0];
```

Checklist:

- [ ] any
- [ ] unknown
- [ ] unions
- [ ] literal types
- [ ] tuples

---

# PHASE 7 — OBJECT MODELING

---

# Type Alias

```ts
type Sensor = {
  name: string;
  value: number;
};
```

---

# Interface

```ts
interface Device {
  id: number;
  active: boolean;
}
```

---

# Optional Properties

```ts
interface User {
  name: string;
  age?: number;
}
```

---

# Readonly

```ts
interface Config {
  readonly id: number;
}
```

Checklist:

- [ ] type aliases
- [ ] interfaces
- [ ] optional properties
- [ ] readonly
- [ ] difference between type/interface

---

# PHASE 8 — ENUMS

```ts
enum Status {
  Idle,
  Running,
  Error,
}
```

Checklist:

- [ ] enums
- [ ] enum states
- [ ] enum usage

---

# PHASE 9 — TYPE NARROWING

```ts
function print(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

Checklist:

- [ ] typeof narrowing
- [ ] instanceof
- [ ] type guards

---

# PHASE 10 — ARRAYS & GENERICS

---

# Generic Function

```ts
function identity<T>(value: T): T {
  return value;
}
```

---

# Generic Interfaces

```ts
interface ApiResponse<T> {
  data: T;
  success: boolean;
}
```

Checklist:

- [ ] generics
- [ ] generic functions
- [ ] generic interfaces

---

# PHASE 11 — CLASSES

```ts
class Sensor {
  value: number;

  constructor(value: number) {
    this.value = value;
  }

  read(): number {
    return this.value;
  }
}
```

---

# Access Modifiers

```ts
class Device {
  private id: number;

  constructor(id: number) {
    this.id = id;
  }
}
```

Checklist:

- [ ] classes
- [ ] constructors
- [ ] methods
- [ ] private/public/protected

---

# PHASE 12 — MODULES

---

# Export

```ts
export function add(a: number, b: number) {
  return a + b;
}
```

---

# Import

```ts
import { add } from "./math";
```

Checklist:

- [ ] exports
- [ ] imports
- [ ] module structure

---

# PHASE 13 — ASYNC TYPESCRIPT

---

# Promise

```ts
async function fetchData(): Promise<string> {
  return "data";
}
```

---

# Await

```ts
async function main() {
  const result = await fetchData();

  console.log(result);
}
```

Checklist:

- [ ] Promise
- [ ] async
- [ ] await

---

# PHASE 14 — ERROR HANDLING

```ts
try {
  throw new Error("Oops");
} catch (err) {
  console.log(err);
}
```

Checklist:

- [ ] try/catch
- [ ] Error objects
- [ ] custom errors

---

# PHASE 15 — ADVANCED TYPES

---

# keyof

```ts
type Keys = keyof User;
```

---

# typeof

```ts
type UserType = typeof user;
```

---

# Utility Types

```ts
Partial<T>;
Required<T>;
Readonly<T>;
Pick<T>;
Omit<T>;
Record<K, T>;
```

Checklist:

- [ ] keyof
- [ ] typeof
- [ ] utility types

---

# PHASE 16 — CONDITIONAL TYPES

```ts
type IsString<T> = T extends string ? true : false;
```

Checklist:

- [ ] conditional types
- [ ] extends keyword

---

# PHASE 17 — MAPPED TYPES

```ts
type Flags<T> = {
  [K in keyof T]: boolean;
};
```

Checklist:

- [ ] mapped types

---

# PHASE 18 — DISCRIMINATED UNIONS

```ts
type State =
  | { type: "loading" }
  | { type: "success"; data: string }
  | { type: "error"; error: string };
```

Checklist:

- [ ] discriminated unions
- [ ] state patterns

---

# PHASE 19 — DECORATORS

```ts
@Controller()
class UserController {}
```

Checklist:

- [ ] decorators
- [ ] metadata basics

---

# PHASE 20 — DECLARATION FILES

```text
index.d.ts
```

Checklist:

- [ ] declaration files
- [ ] external typings

---

# PHASE 21 — STRICT MODE

tsconfig:

```json
{
  "strict": true
}
```

Checklist:

- [ ] strict mode
- [ ] noImplicitAny
- [ ] strictNullChecks

---

# PHASE 22 — NODE.JS + TYPESCRIPT

Learn:

- file system
- path
- CLI tools
- APIs
- Express

Checklist:

- [ ] fs
- [ ] path
- [ ] Express
- [ ] REST APIs

---

# PHASE 23 — FRONTEND TYPESCRIPT

Learn:

- React
- props typing
- hooks typing
- refs
- context

Checklist:

- [ ] React TS
- [ ] props typing
- [ ] hooks typing

---

# PHASE 24 — TESTING

Learn:

- Jest
- Vitest
- unit testing
- mocks

Checklist:

- [ ] unit tests
- [ ] mocks
- [ ] assertions

---

# PHASE 25 — TOOLING

Learn:

- ESLint
- Prettier
- Vite
- tsup
- esbuild

Checklist:

- [ ] ESLint
- [ ] Prettier
- [ ] Vite

---

# PHASE 26 — EMBEDDED FRIENDLY TYPESCRIPT

---

# Binary Data

```ts
Uint8Array;
ArrayBuffer;
DataView;
```

---

# Serial Communication

- serialport library

---

# Protocol Parsing

- UART
- CAN
- MODBUS

Checklist:

- [ ] Uint8Array
- [ ] ArrayBuffer
- [ ] serial communication
- [ ] binary parsing

---

# PHASE 27 — REAL PROJECTS

## Beginner

- [ ] calculator
- [ ] todo app
- [ ] stopwatch
- [ ] converter

---

## Intermediate

- [ ] REST API app
- [ ] websocket dashboard
- [ ] CLI app

---

## Embedded Projects

- [ ] UART parser
- [ ] CAN decoder
- [ ] telemetry dashboard
- [ ] firmware config validator
- [ ] sensor monitor

---

# DAILY STUDY METHOD

Every day:

## Step 1

Read small section.

## Step 2

Write code manually.

## Step 3

Break code intentionally.

## Step 4

Fix errors.

Compiler errors are part of learning.

TypeScript sometimes feels like:
“A highly educated robot aggressively questioning your life choices.”

That means it is working.

---

# OFFICIAL RESOURCES

## Handbook

https://www.typescriptlang.org/docs/handbook/intro.html

---

## Playground

https://www.typescriptlang.org/play

---

## TS for JS Programmers

https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html

---

# FINAL GOAL

Eventually you should be able to:

- build APIs
- build tooling
- build dashboards
- create typed libraries
- parse binary protocols
- use React + TS
- use Node + TS
- understand advanced typing
- structure large applications

---

# FINAL NOTE

Do not rush advanced TypeScript.

Even experienced developers occasionally stare at advanced generic types like archaeologists trying to decode cursed ancient symbols.

That is normal.
