# Sales Tool Engineering Stack Instructions

Related notes:

- [[Architecture]]
- [[Lightweight CRM Spec]]
- [[Project Plan]]

---

## Default stack

The default stack for this product is:

- **C# / ASP.NET Core** for the web app, API, core business logic, background jobs, and product workflows
- **TypeScript** for the frontend
- **PostgreSQL** for the primary system-of-record database
- **Python** for AI workflows, extraction pipelines, embeddings, evaluations, and data-oriented model tasks
- **Rust** for highly performant serverless functions only when the workload clearly benefits from low latency, low memory usage, or stronger systems-level performance

## Core principle

This product should not try to force one language everywhere. It should use the best tool for each layer while keeping the number of languages under control.

The product core lives in **C#**. The UI lives in **TypeScript**. The source of truth lives in **PostgreSQL**. Intelligence-oriented workflows live in **Python**. Performance-critical infrastructure can live in **Rust** when it is earned.

## Usage guidance

1. Default to **C# + TypeScript + PostgreSQL** for almost all product work.
2. Add **Python** where AI or data workflows are materially easier, faster, or more capable there.
3. Add **Rust** only for hot-path or infrastructure-heavy workloads such as high-performance crawling, parsing, event processing, or latency-sensitive serverless functions.
4. Do not introduce extra languages without a clear operational or product advantage.

## Architecture rule

For an early-stage startup, simplicity matters more than technical novelty. The main application should stay easy to ship, easy to host, and easy to reason about.

That means:

- keep the main product in **C#**
- keep the frontend in **TypeScript**
- keep the database model centered on **PostgreSQL**
- isolate **Python** to AI and data boundaries
- isolate **Rust** to proven performance-critical components rather than general application code

## Short version

**The product lives in C#. The UI lives in TypeScript. Intelligence lives in Python. The source of truth lives in PostgreSQL. Performance-critical infrastructure can live in Rust.**
