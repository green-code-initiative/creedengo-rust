# Getting Started with creedengo-rust

This guide will help you set up and use the creedengo-rust dylint library to improve the energy efficiency of your Rust projects.

## Prerequisites

Before you start, make sure you have:

1. **Rust nightly toolchain** installed
2. **cargo-dylint** and **dylint-link** tools

## Installation

### 0. Install RUST and Cargo

Please check installation instructions for [Rust](https://www.rust-lang.org/tools/install) if you haven't installed Rust yet.

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
cargo dylint --path path/to/creedengo-rust-project

# example (Windows PowerShell/Command Prompt) :
cd .\creedengo-rust-test
cargo dylint --path ../creedengo-rust
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

#### TODO DDC - check with an example NOT OK

TODO DDC : check this option, because not ok for me
- in creedengo-rust-test/Cargo.toml, adding the following lines:
```toml
[workspace.metadata.dylint]
libraries = [
    { path = "../creedengo-rust" }
]
```
- next run the command:
```bash
cargo dylint --all
```
but error:
```
Warning: No libraries were found.
```

### Method 3: Git Repository Configuration

For a more permanent setup, add this to your `Cargo.toml`:

```toml
[workspace.metadata.dylint]
libraries = [
    { git = "https://github.com/green-code-initiative/creedengo-rust", pattern = "creedengo-rust" }
]
```

#### TODO DDC - check with an example
- where to add these lines
- how to check if it works ?

## VS Code Integration

After installing rust-analyzer extension in VSCode, to see dylint warnings directly in VS Code, add the following to your `.vscode/settings.json`:

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

#### TODO DDC - check with an example
- where to add these two blocks ?
- error when in creedengo-rust base repository because several Cargo.tom files found
  - maybe launch VS Code only inside a subdirectory like creedengo-rust-test ? and add .vscode/settings.json in this subdirectory ?

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

#### TODO DDC - check the relevance of this rule for RUST
- how did you test that this rule is ok for RUST ?
- maybe this rule is not relevant for RUST ? I think we have to measure the relevance for RUST with some frameworks / tools like Code Carbon for python language

## Building the Project

To build the creedengo-rust library:

```bash
cd creedengo-rust
cargo build --release
```

### TODO DDC - question
- why do we need to build the project ? is it necessary for dylint ?
- what is the production of this command ? where is the output ?

## Testing

To run the tests:

```bash
cd creedengo-rust
cargo test
```

### TODO DDC - question
- how does test command detect the tests in "ui" directory ?
- "ui" directory is the standard directory for dylint tests ?
- I don't understand how the tests are run, because I don't see any test function in the "ui" directory
- I see a test bloc in src/lib.rs file : is it the standard way to test dylint rules ? It seems strange to me to add test code in the same file as the rules implementation.

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

### TODO DDC - refactoring this repository
- the repository inside green-code-initiative organization in github is called "creedengo-rust"
- but there are 3 current sub-directories:
  - creedengo-rust => this one contains the rule implementation, and must be renamed with another name
  - creedengo-rust-test => this one is a test project to test the rules in real environment
  - creedengo-rust-sonar => this one is for SonarQube integration
    - do you check it on a sonarqube server ?
- do we have to commit Cargo.lock files ? for me, no, because there are autogenerated files, and we don't need to commit autogenerated files. are you ok to add these files in .gitignore ?
- maybe add a section to explain sonarqube integration adn the content of creedengo-rust-sonar directory ?
  - the "restart_sonar.ps1" seems to be a script file for windows, but I'm on MacOS :p ... maybe can we write a bash script for unix / MacOS users also ?