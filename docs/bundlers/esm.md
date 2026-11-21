---
sidebar_position: 4
title: Vite, Rollup and esbuild
---

# Vite, Rollup and esbuild (ES-module chunks)

Vite (Rollup, or Rolldown from Vite 8), Rollup and esbuild with `--splitting` emit chunks that are native ES modules. They have no runtime registry: one chunk imports another by URL with real `import`/`export` statements. taintwire links those statements across the files of one [`addFiles()`](../api/reference.md#graphaddfiles) call. A call or value read through an import gets the `CALLS`, `ARGUMENT_TO`, `RETURNS_TO` and `FLOWS_TO` edges it would get if both modules were in one file.

## What you get

```js
// assets/source-a1b2.js
function u() { return window.name }
export { u as n };

// assets/page-c3d4.js
import { n as a } from "./source-a1b2.js";
document.getElementById("o").innerHTML = a();
```

Loaded together, `a()` gets a `CALLS` edge to `u`, and `window.name` reaches the `innerHTML` assignment through `RETURNS_TO`. Loaded one file at a time, `a` is an unknown import and the path stops there.

## What's linked

| Shape | Example |
| --- | --- |
| Named imports | `import {k as x} from "./c.js"` with `export {v as k}`, `export function`, `export const`, `export class` |
| Default imports | `import x from "./c.js"` with `export default <expr>` / `export default function` |
| Namespace imports | `import * as ns from "./c.js"`, then `ns.k` |
| Re-exports | `export {x as y} from "./c.js"`, `export * from "./c.js"` (resolved through any number of barrels) |
| `import()` | `import("./c.js").then(m => m.k)`, `(await import("./c.js")).k`, Vite's `__vitePreload(() => import("./c.js"), deps)` |
| Rolldown namespaces | ``import(`./c.js`).then(e => e.a)``, where `a` is the chunk's exported getter object `helper({k: () => v})` |

Specifiers resolve the way the browser resolves them from the chunk's own URL:
- Relative specifiers (`./x.js`, `../x.js`) resolve against the importing file.
- An absolute path (`/assets/x.js`) or a full URL matches the one loaded file whose path ends with it.
- A query string or hash is ignored.

Bare package names (`import "react"`) aren't in the graph and stay unresolved.

The graph gets `IMPORTS` edges (`ImportDeclaration → File`, with `module_id`) and `EXPORTS` edges (`File → value`, with `name`), like the [webpack](webpack.md) registry edges. [`moduleLinks()`](../api/reference.md#modulelinks) reports an ES-module file as defining itself and needing the files it imports, so [`planUnits()`](../api/reference.md#planunits) covers these builds too.

## Not linked yet

- `export * as ns from "./c.js"`, and namespace objects used through anything other than a direct `ns.k` read.
- Import maps (`<script type="importmap">`). Specifiers are resolved by path only.
- esbuild's CommonJS interop (`__toESM(require_x())`) inside ES-module chunks.
