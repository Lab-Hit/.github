<p align="center">
  <a href="https://labhit.dev">
    <img src="https://raw.githubusercontent.com/Lab-Hit/.github/main/profile/banner.svg" alt="LabHit — The modular CI/CD engine" width="100%">
  </a>
</p>

<p align="center">
  <a href="https://labhit.dev"><img src="https://img.shields.io/badge/labhit.dev-4F46E5?style=for-the-badge&logoColor=white" alt="Website"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/Specification-181717?style=for-the-badge&logo=github&logoColor=white" alt="Spec"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><img src="https://img.shields.io/badge/Apache_2.0-D22128?style=for-the-badge&logo=apache&logoColor=white" alt="License"></a>
</p>

---

<table>
<tr>
<td align="center" width="33%">

### :gear: Modular

The engine ships with **zero built-in integrations**. Install `source/git` for checkout, `build/container` for builds, `scan/trivy` for security. Your pipeline has exactly what it needs — nothing more.

</td>
<td align="center" width="33%">

### :shield: Sandboxed

Every extension runs in a **WASM sandbox** with deny-by-default permissions. Scoped filesystem, network allowlists, capped memory and CPU. A bad plugin cannot touch the host.

</td>
<td align="center" width="33%">

### :package: Portable

**One binary, three modes.** Run on your laptop with embedded storage, scale to a standalone server, or deploy a full cluster. Same config, same extensions, same results.

</td>
</tr>
</table>

---

### Pipeline format

Stages declare **what** to run (`use` an extension or `run` a command), **when** to run (`after` dependencies), and **how** to run (inside a `sandbox`). The scheduler builds a DAG and runs independent stages in parallel.

```yaml
engine: "1"
pipeline:
  name: build-and-deploy

stages:
  fetch:
    use: source/git
    with:
      depth: 1

  test:
    after: [fetch]
    run: cargo test --workspace
    sandbox:
      image: rust:1.93-slim

  build:
    after: [test]
    use: build/container
    with:
      dockerfile: Dockerfile

  scan:
    after: [build]
    use: scan/trivy
    with:
      fail_on: [critical, high]

  deploy:
    after: [build, scan]
    use: deploy/kubernetes
    gate:
      approval: required
      policy: production-deploy
```

> Extensions use the `category/name` convention. The [specification](https://github.com/Lab-Hit/labhit-spec) defines the interface contract — WIT types, gRPC services, and the pipeline JSON Schema.

---

<p align="center">
  <a href="https://labhit.dev"><b>Website</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><b>Specification</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/.github/blob/main/SECURITY.md"><b>Security</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><b>License</b></a>
</p>
