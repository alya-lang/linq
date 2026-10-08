# linq

[![CI](https://github.com/alya-lang/linq/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/linq/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/linq?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Flinq%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Flinq%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Fluent LINQ and functional collection query pipeline for Alya: filter, map, sort, group, aggregate and more

---

## 🌟 Features

- ⚡ **Lightweight & High Performance**: Clean native collection pipelines without heap overhead or intermediate bloat
- 🔄 **Fluent Pipeline (`query`)**: Chain operations cleanly with `query(arr).where(...).map(...).to_array()`
- 🔍 **Filtering & Slicing**: `where`, `filter`, `take`, `take_while`, `skip`, `skip_while`, `distinct`, `distinct_by`
- 🗺️ **Transformations**: `map`, `flat_map`, `chunk`, `window`, `zip`, `concat`, `reverse`
- 📊 **Sorting & Grouping**: `order_by`, `order_by_desc`, and key-based `group_by` returning bucket maps
- 📈 **Aggregations & Reductions**: `sum`, `min`, `max`, `average`, `count`, `aggregate` (fold/reduce), and `for_each`
- 🎯 **Search & Quantifiers**: `any`, `all`, `first`, `last`, `find`, `contains`
- 🧪 **Enterprise Test Suite**: 100% test coverage with automated unit tests and benchmark throughput measurements

---

## 📁 Project Architecture

```
linq/
├── .alyalint               # Linter configuration (rules, exclusions, severity overrides)
├── .editorconfig           # Uniform formatting rules across IDEs and editors
├── .gitignore              # Ecosystem standard ignore filters
├── .vscode/                # VS Code workspace settings, DAP launch configurations & tasks
├── alya.toml               # Package manifest with dependencies and metadata
├── src/
│   ├── lib.alya            # Public API facade and constructor exports
│   ├── types.alya          # Core Enumerable struct definition
│   └── pipeline.alya       # Fluent query chain transformation & terminal methods
├── examples/
│   └── demo.alya           # Comprehensive runnable walkthrough of all query operations
├── tests/
│   └── test_basic.alya     # Automated test suite with 60+ test assertions
└── benches/
    └── bench_basic.alya    # Micro-benchmarks measuring pipeline throughput
```

> [!NOTE]
> **Visibility & Modularity:** Symbols annotated with `pub` (`pub function`, `pub struct`) are exported to external callers. Internal pipeline transformations remain clean and encapsulated.

---

## 📦 Installation

Add `linq` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
linq = { git = "https://github.com/alya-lang/linq", branch = "main" }
```

Or install it directly using the Alya package CLI:

```bash
alya add linq --git https://github.com/alya-lang/linq --branch main
alya install
```

---

## 🚀 Quick Start

```alya
import "linq" as linq

function is_even(n)
    return n % 2 == 0
end

function square(n)
    return n * n
end

function main()
    # 1. Fluent filter, map, and materialization
    let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    let result = linq::query(numbers)
        .where(is_even)
        .map(square)
        .to_array()

    say f"Even squares: {result}"

    # 2. Sequence generation and aggregation
    let total = linq::range(1, 100).sum()
    say f"Sum 1..100: {total}"

    # 3. Pagination with skip and take
    let page = linq::query(numbers).skip(3).take(4).to_array()
    say f"Page 2 items: {page}"
end

main()
```

---

## 📖 API Reference

| Symbol | Visibility | Description |
|---|---|---|
| `query(source)` | `pub function` | Wraps an array into an `Enumerable` query pipeline. |
| `from_array(source)` | `pub function` | Alias for `query(source)`. |
| `range(start, count)` | `pub function` | Generates a sequential integer range `[start .. start + count - 1]`. |
| `replicate(item, count)` | `pub function` | Generates an `Enumerable` sequence containing `item` repeated `count` times. |
| `Enumerable.where(predicate)` | `pub method` | Filters elements matching the boolean predicate function. |
| `Enumerable.filter(predicate)` | `pub method` | Alias for `where(predicate)`. |
| `Enumerable.take(count)` | `pub method` | Returns the first `count` elements of the sequence. |
| `Enumerable.take_while(predicate)` | `pub method` | Takes elements until the predicate returns false. |
| `Enumerable.skip(count)` | `pub method` | Bypasses the first `count` elements and returns the remainder. |
| `Enumerable.skip_while(predicate)` | `pub method` | Bypasses elements while the predicate returns true. |
| `Enumerable.distinct()` | `pub method` | Filters duplicate elements preserving first-seen order. |
| `Enumerable.distinct_by(key_fn)` | `pub method` | Filters duplicate elements based on a key extraction function. |
| `Enumerable.map(transform)` | `pub method` | Projects each element into a new form using the transform function. |
| `Enumerable.flat_map(transform)` | `pub method` | Maps each element to an array and flattens the resulting sequences. |
| `Enumerable.chunk(size)` | `pub method` | Splits elements into chunks of fixed `size`. |
| `Enumerable.window(size)` | `pub method` | Emits sliding windows of length `size` across elements. |
| `Enumerable.zip(other, selector)` | `pub method` | Merges two sequences element-wise with a selector combiner. |
| `Enumerable.concat(other)` | `pub method` | Concatenates another array or Enumerable sequence. |
| `Enumerable.reverse()` | `pub method` | Inverts the order of elements in the pipeline. |
| `Enumerable.order_by(key_fn)` | `pub method` | Sorts elements in ascending order using key extractor (or direct comparison). |
| `Enumerable.order_by_desc(key_fn)` | `pub method` | Sorts elements in descending order using key extractor. |
| `Enumerable.group_by(key_fn)` | `pub method` | Groups elements by key into a map of `{ key: [elements] }`. |
| `Enumerable.to_array()` | `pub method` | Materializes the query pipeline into an array. |
| `Enumerable.count()` | `pub method` | Returns the number of elements in the sequence. |
| `Enumerable.any(predicate)` | `pub method` | Returns true if any element satisfies the predicate (or if non-empty when predicate is null). |
| `Enumerable.all(predicate)` | `pub method` | Returns true if all elements satisfy the predicate. |
| `Enumerable.first()` | `pub method` | Returns the first element or null if empty. |
| `Enumerable.last()` | `pub method` | Returns the last element or null if empty. |
| `Enumerable.find(predicate)` | `pub method` | Returns the first element satisfying the predicate, or null. |
| `Enumerable.contains(target)` | `pub method` | Returns true if target value exists in the sequence. |
| `Enumerable.sum()` | `pub method` | Computes the arithmetic sum of numeric elements. |
| `Enumerable.min()` | `pub method` | Returns the minimum numeric element in the sequence. |
| `Enumerable.max()` | `pub method` | Returns the maximum numeric element in the sequence. |
| `Enumerable.average()` | `pub method` | Returns the arithmetic mean of numeric elements. |
| `Enumerable.aggregate(initial, accumulator)` | `pub method` | Applies an accumulator reduction function over elements. |
| `Enumerable.for_each(action)` | `pub method` | Invokes an action callback for each element. |

---

## 🧪 Running Tests & Benchmarks

Run the automated test suite using `alya test`:

```bash
alya test
```

Generate static API documentation:

```bash
alya doc . -o docs --markdown
```

Run the benchmark suite:

```bash
alya run benches/bench_basic.alya
```

Run the example demo:

```bash
alya run examples/demo.alya
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

### 💻 Developer Tooling & VS Code Integration

This package comes preconfigured with recommended workspace settings and tasks for **Visual Studio Code**:
- **LSP & Formatting**: Auto-formatting on save and real-time Language Server diagnostics via `alya-lang.vscode-alya`.
- **DAP Debugging**: Launch configurations in `.vscode/launch.json` ready for interactive step-debugging via `F5`.
- **Predefined Tasks**: Press `Ctrl+Shift+B` or run tasks (`Test`, `Lint`, `Format`, `Build Docs`) directly from the Command Palette.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.