# neural-shared

> Shared utilities for Neural Garage code analysis tools

[![Crates.io](https://img.shields.io/crates/v/neural-shared.svg)](https://crates.io/crates/neural-shared)
[![License: MIT OR Apache-2.0](https://img.shields.io/badge/License-MIT%20OR%20Apache--2.0-blue.svg)](https://opensource.org/licenses/MIT)

## Overview

`neural-shared` provides common functionality for the Neural Garage suite of code analysis tools. It includes:

- **Parser Module** - Tree-sitter-based AST parsing for multiple languages
- **Scanner Module** - File system traversal with .gitignore support
- **Report Module** - Generic reporting framework for analysis results

## Features

### Parser Module

Language-agnostic parsing infrastructure:

```rust
use neural_shared::parser::{Language, PythonParser, Parser};

let parser = PythonParser::new()?;
let source = "def hello(): pass";
let parsed = parser.parse(source, Path::new("example.py"))?;

// Access definitions, usages, and entry points
for def in parsed.definitions {
    println!("Found: {} at {}:{}", def.name, def.location.line, def.location.column);
}
```

Supported languages:
- Python
- TypeScript
- JavaScript

### Scanner Module

Efficient file system scanning:

```rust
use neural_shared::Scanner;

let scanner = Scanner::new(Path::new("./src"));
let files = scanner.scan()?;  // Respects .gitignore
```

Features:
- .gitignore support via the `ignore` crate
- Parallel file scanning
- Language-specific file filtering

### Report Module

Generic reporting framework with `Finding` trait:

```rust
use neural_shared::report::{Finding, Reporter, JsonReporter};
use serde::Serialize;

#[derive(Serialize)]
struct MyFinding {
    // Your finding data
}

impl Finding for MyFinding {
    fn kind(&self) -> String { /* */ }
    fn name(&self) -> String { /* */ }
    // ... other methods
}

let reporter = JsonReporter;
let json = reporter.report(&findings)?;
```

Built-in reporters:
- `JsonReporter` - LLM-friendly JSON output
- `MarkdownReporter` - Human-readable markdown

## Part of Neural Garage 🧠🔧

This library is part of the [Neural Garage](https://github.com/neural-garage/tools) monorepo.

**Tools using neural-shared:**
- **[bury](https://crates.io/crates/bury)** - Dead code detector
- **complexity** *(coming soon)* - Code complexity analyzer

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

## Contributing

See the [main repository](https://github.com/neural-garage/tools) for contribution guidelines.
