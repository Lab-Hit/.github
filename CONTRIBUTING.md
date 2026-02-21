# Contributing to LabHit

Contributions are welcome across all LabHit repositories.

## Where to Contribute

| Repository | What to contribute |
|-----------|-------------------|
| [labhit-spec](https://github.com/Lab-Hit/labhit-spec) | Interface proposals, schema changes |

## How to Contribute

1. Fork the repository
2. Create a branch from `main`
3. Make your changes
4. Write tests for new functionality
5. Run the test suite
6. Submit a pull request

## Development Setup

### Engine (Rust)

```bash
# Prerequisites: Rust 1.75+, Docker 24+
rustup target add wasm32-wasip1
cd engine
cargo build --workspace
cargo test --workspace
cargo clippy --workspace
```

### Dashboard (React)

```bash
# Prerequisites: Node.js 20+
cd dashboard
npm install
npm run dev
```

## Commit Messages

Use clear, descriptive commit messages:

```
fix: prevent path traversal in filesystem capability check
feat: add network allowlist wildcard validation
docs: update CLI reference with new extension subcommands
test: add scheduler cycle detection edge cases
```

## Code Style

- **Rust:** `cargo fmt` and `cargo clippy` with zero warnings
- **TypeScript:** Prettier defaults
- **Documentation:** Direct, technical prose

## License

By contributing, you agree that your contributions are licensed under Apache 2.0.
