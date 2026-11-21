---
sidebar_position: 1.5
title: Terminology
---

# Terminology

## Terms

- **taintwire** is the whole technology: this library, which turns JavaScript into a graph and queries it.
- A **PookieGraph** is the graph taintwire generates from your code. It's an embedded [LadybugDB](https://ladybugdb.com/) database, saved as a single `.pg` file. See the [graph model](graph-model.md).
- A **PookieGraph Query** (pgq) is a `.pgq` file of queries you run on a PookieGraph. See [Query files (pgq)](pgq.md).
- **PookieGraph aliases** (pga) are short names for Cypher fragments that repeat across queries. They live in `.pgq` library files in the [taintwire-queries](https://github.com/js-recon/taintwire-queries) pack, not in a file type of their own. See [PookieGraph aliases (pga)](pga.md).

Why these names? No reason at all.

## File extensions

| Extension | Holds                                                                                                                 | Made by                                              | Read by                                                  |
| --------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- | -------------------------------------------------------- |
| `.pg`     | One PookieGraph, a LadybugDB database file                                                                            | `PookieGraph.open(path)`, `js-recon taintwire build` | `PookieGraph.open(path)`, `js-recon taintwire analyze`   |
| `.pgq`    | A TypeScript module of selectors and rules (a PookieGraph Query)                                                      | You, or a query pack such as taintwire-queries       | `loadPgq()`, `js-recon taintwire analyze` and `validate` |
| `.tw`     | A tar of one `.pg` per unit plus a `manifest.json`, so large builds load one unit at a time (see [memory](memory.md)) | `js-recon taintwire build -o app.tw`                 | `js-recon taintwire analyze -g app.tw`                   |

`.tw` is a [js-recon](https://js-recon.io) format: the taintwire package itself only reads and writes `.pg` and `.pgq` files.
