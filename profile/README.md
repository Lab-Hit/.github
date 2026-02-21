# LabHit

**The modular CI/CD engine.**

A minimal core handles pipeline orchestration. All features -- source checkout, container builds, deployments, security scans, notifications -- are delivered through sandboxed WebAssembly extensions.

## Pipeline Example

```yaml
engine: "1"
pipeline:
  name: build-and-deploy

stages:
  fetch:
    use: source/git
  test:
    after: [fetch]
    run: cargo test --workspace
    sandbox:
      image: rust:1.93-slim
  build:
    after: [test]
    use: build/container
  deploy:
    after: [build]
    use: deploy/kubernetes
    gate:
      approval: required
```

## Links

- [labhit.dev](https://labhit.dev)
- [Specification](https://github.com/Lab-Hit/labhit-spec)

## License

Apache 2.0
