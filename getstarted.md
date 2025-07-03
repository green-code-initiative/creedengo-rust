# Getting Started with creedengo-rust

This guide will help you set up and use the creedengo-rust dylint library to improve the energy efficiency of your Rust projects.

## Prerequisites

Before you start, make sure you have:

1. **Rust nightly toolchain** installed
2. **cargo-dylint** and **dylint-link** tools

## Installation

### 1. Install Required Tools

```bash
# Install dylint tools
cargo install cargo-dylint dylint-link
```

### 2. Set up Rust Nightly Toolchain

```bash
# Install nightly toolchain
rustup toolchain install nightly

# Set nightly as default (optional)
rustup default nightly
```

## Using creedengo-rust in Your Project

### Method 1: Direct Usage with Path

Navigate to your Rust project directory and run:

```bash
cargo dylint --path path/to/creedengo-rust/creedengo-rust
```

### Method 2: Workspace Metadata Configuration

Add the following to your project's `Cargo.toml`:

```toml
[workspace.metadata.dylint]
libraries = [
    { path = "path/to/creedengo-rust/creedengo-rust" }
]
```

Then run:

```bash
cargo dylint --all
```

### Method 3: Git Repository Configuration

For a more permanent setup, add this to your `Cargo.toml`:

```toml
[workspace.metadata.dylint]
libraries = [
    { git = "https://github.com/green-code-initiative/creedengo-rust", pattern = "creedengo-rust" }
]
```

## VS Code Integration

To see dylint warnings directly in VS Code, add the following to your `.vscode/settings.json`:

```json
{
    "rust-analyzer.check.overrideCommand": [
        "cargo",
        "dylint",
        "--path",
        "path/to/creedengo-rust/creedengo-rust",
        "--",
        "--all-targets",
        "--message-format=json"
    ]
}
```

For workspace metadata configuration, use:

```json
{
    "rust-analyzer.check.overrideCommand": [
        "cargo",
        "dylint",
        "--all",
        "--",
        "--all-targets",
        "--message-format=json"
    ]
}
```

## Available Rules

### GCI2 - Avoid Multiple If-Else Statements

This rule detects chains of if-else statements that could impact energy consumption and suggests using pattern matching instead.

#### ❌ Bad Example:

```rust
fn get_status_message(code: u16) -> &'static str {
    if code == 200 {
        "OK"
    } else if code == 404 {
        "Not Found"
    } else if code == 500 {
        "Internal Server Error"
    } else if code == 403 {
        "Forbidden"
    } else {
        "Unknown"
    }
}
```

#### ✅ Good Example:

```rust
fn get_status_message(code: u16) -> &'static str {
    match code {
        200 => "OK",
        404 => "Not Found",
        500 => "Internal Server Error",
        403 => "Forbidden",
        _ => "Unknown",
    }
}
```

## Building the Project

To build the creedengo-rust library:

```bash
cd creedengo-rust
cargo build --release
```

## Testing

To run the tests:

```bash
cd creedengo-rust
cargo test
```

## Troubleshooting

### Issue: `cargo dylint` command not found

**Solution:** Make sure cargo-dylint is installed:
```bash
cargo install cargo-dylint
```

### Issue: Nightly toolchain required

**Solution:** Install and use the nightly toolchain:
```bash
rustup toolchain install nightly
rustup override set nightly
```

### Issue: VS Code not showing warnings

**Solution:** 
1. Ensure rust-analyzer extension is installed
2. Check that the path in `settings.json` is correct
3. Restart VS Code after configuration changes

## Next Steps

- Check the [dylint documentation](https://github.com/trailofbits/dylint) for advanced usage
- Contribute new rules following our [CONTRIBUTING.md](CONTRIBUTING.md) guide
- Report issues or suggest improvements in our repository

## Resources

- [Dylint GitHub Repository](https://github.com/trailofbits/dylint)
- [Creedengo Project](https://github.com/green-code-initiative/creedengo-rules-specifications)
- [Green Code Initiative](https://www.green-code-initiative.org/)