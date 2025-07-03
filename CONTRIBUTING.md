# Contributing to creedengo-rust

Thank you for your interest in contributing to creedengo-rust! This document provides guidelines for adding new dylint rules and contributing to the project.

Please also read the common [CONTRIBUTING.md](https://github.com/green-code-initiative/creedengo-common/blob/main/doc/CONTRIBUTING.md) in `creedengo-common` repository for general Creedengo contribution guidelines.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Setting Up Development Environment](#setting-up-development-environment)
- [Adding a New Dylint Rule](#adding-a-new-dylint-rule)
- [Testing Your Rule](#testing-your-rule)
- [Documentation](#documentation)
- [Submitting Your Contribution](#submitting-your-contribution)
- [Resources](#resources)

## Prerequisites

Before contributing, make sure you have:

1. **Rust nightly toolchain** - Required for dylint development
2. **cargo-dylint and dylint-link** - Essential dylint tools
3. **Git** - For version control
4. **VS Code** (recommended) - With rust-analyzer extension

## Setting Up Development Environment

1. **Clone the repository:**
   ```bash
   git clone https://github.com/green-code-initiative/creedengo-rust.git
   cd creedengo-rust
   ```

2. **Install required tools:**
   ```bash
   cargo install cargo-dylint dylint-link
   rustup toolchain install nightly
   ```

3. **Build the project:**
   ```bash
   cd creedengo-rust
   cargo build
   ```

4. **Run tests to ensure everything works:**
   ```bash
   cargo test
   ```

## Adding a New Dylint Rule

### Step 1: Understanding Dylint Architecture

Dylint rules are implemented as Rust lints that analyze code for patterns. Each rule:
- Implements the `LateLintPass` or `EarlyLintPass` trait
- Defines what code patterns to detect
- Provides helpful error messages and suggestions

For detailed dylint documentation, visit: [Dylint GitHub Repository](https://github.com/trailofbits/dylint)

### Step 2: Define Your Rule

1. **Choose a rule identifier** following the GCI naming convention (e.g., GCI3, GCI4)

2. **Add your rule to `src/lib.rs`:**

```rust
dylint_linting::declare_late_lint! {
    /// ### What it does
    /// Brief description of what the rule detects
    ///
    /// ### Why is this bad?
    /// Explanation of the environmental impact
    ///
    /// ### Example
    /// ```rust
    /// // Bad example
    /// let inefficient_code = do_something();
    /// ```
    ///
    /// Use instead:
    /// ```rust
    /// // Good example
    /// let efficient_code = do_something_better();
    /// ```
    pub YOUR_RULE_NAME,
    Warn,
    "brief description for lint message"
}
```

3. **Implement the lint logic:**

```rust
impl<'tcx> LateLintPass<'tcx> for YourRuleName {
    fn check_expr(&mut self, cx: &rustc_lint::LateContext<'tcx>, expr: &'tcx Expr<'tcx>) {
        // Your detection logic here
        if condition_detected {
            span_lint_and_help(
                cx,
                YOUR_RULE_NAME,
                expr.span,
                "description of the problem",
                None,
                "suggestion for improvement",
            );
        }
    }
}
```

### Step 3: Create Test Cases

1. **Create a test file** in the `ui/` directory (e.g., `ui/your_rule.rs`):

```rust
// Examples that should trigger the lint
fn bad_example() {
    // Code that should be flagged
}

// Examples that should NOT trigger the lint
fn good_example() {
    // Code that follows best practices
}

fn main() {
    bad_example();
    good_example();
}
```

2. **Run tests to generate expected output:**
   ```bash
   cargo test
   ```

3. **Create the `.stderr` file** with expected warnings (this will be generated automatically during testing)

### Step 4: Common Lint Patterns

Here are some useful patterns for implementing dylint rules:

#### Detecting Function Calls
```rust
fn check_expr(&mut self, cx: &LateContext<'tcx>, expr: &'tcx Expr<'tcx>) {
    if let ExprKind::Call(func, _args) = expr.kind {
        // Check function calls
    }
}
```

#### Detecting Method Calls
```rust
fn check_expr(&mut self, cx: &LateContext<'tcx>, expr: &'tcx Expr<'tcx>) {
    if let ExprKind::MethodCall(method_name, receiver, args, _) = expr.kind {
        // Check method calls
    }
}
```

#### Detecting Control Flow
```rust
fn check_expr(&mut self, cx: &LateContext<'tcx>, expr: &'tcx Expr<'tcx>) {
    match expr.kind {
        ExprKind::If(_, _, _) => { /* Handle if statements */ }
        ExprKind::Loop(_, _, _, _) => { /* Handle loops */ }
        ExprKind::Match(_, _, _) => { /* Handle match statements */ }
        _ => {}
    }
}
```

## Testing Your Rule

### Unit Testing

```bash
cd creedengo-rust
cargo test
```

### Integration Testing

Test your rule on real projects:

```bash
cd ../creedengo-rust-test
cargo dylint --path ../creedengo-rust
```

### Manual Testing

Create test scenarios in the `creedengo-rust-test` project to verify your rule works correctly.

## Documentation

### Rule Documentation

Each rule must include:

1. **Clear description** of what it detects
2. **Explanation** of why the pattern is problematic for energy consumption
3. **Code examples** showing bad and good patterns
4. **References** to relevant research or benchmarks if available

### Update README

Add your rule to the rules table in `readme.md`:

```markdown
|[GCI3](link-to-spec)|Your rule description|⚠️|✅|
```

## Submitting Your Contribution

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/gci-your-rule-name
   ```

2. **Make your changes** following the guidelines above

3. **Test thoroughly:**
   ```bash
   cargo test
   cargo dylint --path ../creedengo-rust
   ```

4. **Commit your changes:**
   ```bash
   git add .
   git commit -m "feat: add GCI3 rule for detecting inefficient pattern"
   ```

5. **Push and create a Pull Request:**
   ```bash
   git push origin feature/gci-your-rule-name
   ```

## Code Style Guidelines

- Follow Rust conventions and use `cargo fmt`
- Add comprehensive documentation for public items
- Include inline comments for complex logic
- Use descriptive variable and function names
- Keep functions focused and concise

## Resources

### Dylint Resources
- [Dylint GitHub Repository](https://github.com/trailofbits/dylint)
- [Dylint Documentation](https://github.com/trailofbits/dylint#readme)
- [Writing Dylint Rules Guide](https://github.com/trailofbits/dylint/blob/master/docs/writing_lints.md)

### Rust Compiler Resources
- [Rustc Dev Guide](https://rustc-dev-guide.rust-lang.org/)
- [HIR Documentation](https://rustc-dev-guide.rust-lang.org/hir.html)
- [Writing Clippy Lints](https://github.com/rust-lang/rust-clippy/blob/master/book/src/development/adding_lints.md)

### Creedengo Resources
- [Creedengo Rules Specifications](https://github.com/green-code-initiative/creedengo-rules-specifications)
- [Green Code Initiative](https://www.green-code-initiative.org/)

## Getting Help

If you need help:

1. Check the [dylint documentation](https://github.com/trailofbits/dylint)
2. Look at existing rules in this repository for examples
3. Open an issue in this repository for project-specific questions
4. Join the Creedengo community discussions

Thank you for contributing to a more sustainable software ecosystem! 🌱