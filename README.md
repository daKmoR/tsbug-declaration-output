# Different Declaration output for same code from js and ts file

This is a test project to demonstrate a bug in the TypeScript compiler when using JavaScript.
See https://github.com/microsoft/TypeScript/issues/51622

## Outline

When creating types from a `ts` source file it keeps the import pathes as is. (which is expected)

👉 Source: [src/ElementA.ts](src/ElementA.ts)

```js
import { LitElement } from "lit";
export class ElementA extends LitElement {}
```

👉 Output: [dist-types/src/ElementA.d.ts](dist-types/src/ElementA.d.ts)

```js
import { LitElement } from "lit";
export declare class ElementA extends LitElement {}
```

When creating types from "the exact same file" but as a `js` it changes the import pathes by "resolving" them.

👉 Source: [src/ElementB.js](src/ElementB.js)

```js
import { LitElement } from "lit";
export class ElementB extends LitElement {}
```

### Expected Behavior:

```js
export class ElementB extends LitElement {}
import { LitElement } from "lit-element/lit-element.js";
```

### Actual Behavior:

👉 Output: [dist-types/src/ElementB.d.ts](dist-types/src/ElementB.d.ts)

```js
export class ElementB extends LitElement {}
import { LitElement } from "lit";
```

## Consequences

Because of the miss match between the "location" of `LitElement` TS believes those are different LitElement types.

## Steps to reproduce

1. Clone this repo
2. Run `npm install`
3. Run `npm run types`
4. Inspect the output in `dist-types`

## Environment

- Node 18
- TypeScript 4.9.3
