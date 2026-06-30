This prompt is to gnerate 

```
**Role:** Expert Rust and C# Systems Engineer.

**Objective:** Create a C# wrapper for the high-performance `spider` Rust crate (github.com) to enable fast web crawling within the .NET ecosystem.

**Task Requirements:**

1. **Rust Layer (The Bridge):**
    - Create a Rust library (`cdylib`) that uses the `spider` crate.
    - Expose a `extern "C"` function called `crawl_site` that takes a URL string and a callback function pointer.
    - The Rust code should initialize the `Website` struct, enable `with_markdown(true)`, and run the crawl asynchronously.
    - As each page is found, it should pass the URL and the Markdown content back to C# via the callback.
2. **C# Layer (The Client):**
    - Create a .NET 10 class called `SpiderClient`.
    - Use `[DllImport]` to link to the compiled Rust library.
    - Define a delegate and an event (or an `IAsyncEnumerable`) so the user can process crawled pages in real-time.
    - Ensure proper string memory handling (converting Rust's `*const c_char` to C# strings safely).
3. **Tooling:**
    - Provide a `Cargo.toml` with the necessary dependencies (`spider`, `tokio`).
    - Provide a basic `Program.cs` example showing how to start a crawl and print the results to the console.

**Note:** Prioritize performance and safety. Ensure the Rust `tokio` runtime is properly handled within the FFI boundary so it doesn't block the C# main thread.

---

```

