<p align="center">
  <img src="https://raw.githubusercontent.com/Lab-Hit/.github/main/profile/labhit-logo-512.png" height="120" alt="LabHit">
</p>

<h3 align="center">The modular CI/CD engine.</h3>

<p align="center">
  <a href="https://labhit.dev"><img src="https://img.shields.io/badge/labhit.dev-4F46E5?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0id2hpdGUiPjxwYXRoIGQ9Ik0xMiAyQzYuNDggMiAyIDYuNDggMiAxMnM0LjQ4IDEwIDEwIDEwIDEwLTQuNDggMTAtMTBTMTcuNTIgMiAxMiAyem0tMSAxNy45M2MtMy45NS0uNDktNy0zLjg1LTctNy45MyAwLS42Mi4wOC0xLjIxLjIxLTEuNzlMOSAxNXY1Yy4wNS41NSAxLjA1IDEgMiAxdjIuOTN6TTEyIDIwYy0uNTUgMC0xLjA5LS4wNS0xLjYxLS4xNVYxOGMwLTEuMS45LTIgMi0yaDF2LTMuMjFsNS45MS01LjkxYy4yOC44Ni40NCAxLjc4LjQ0IDIuNzMgMCA0LjQyLTMuNTggOC0gOCAxMHoiLz48L3N2Zz4=&logoColor=white" alt="Website"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/Specification-181717?style=flat-square&logo=github&logoColor=white" alt="Spec"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><img src="https://img.shields.io/badge/Apache_2.0-D22128?style=flat-square&logo=apache&logoColor=white" alt="License"></a>
</p>

---

A minimal core handles pipeline orchestration. All features -- source checkout, container builds, deployments, security scans, notifications -- are delivered through sandboxed WebAssembly extensions.

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

---

<p align="center">
  <a href="https://labhit.dev">Website</a>
  &nbsp;&middot;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec">Specification</a>
  &nbsp;&middot;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE">License</a>
</p>
