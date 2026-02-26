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
    after: [test]
    use: scan/trivy

  deploy:
    after: [build, scan]
    use: deploy/kubernetes
    gate:
      approval: required
```

> Extensions use the `category/name` convention. The [specification](https://github.com/Lab-Hit/labhit-spec) defines the interface contract — WIT types, gRPC services, and the pipeline JSON Schema.

---

<h3 align="center">Progress</h3>

<p align="center"><b>Built in the open. Shipped when ready.</b></p>

<br>

<table width="100%">
<thead>
<tr>
<th align="center" width="33%">:white_check_mark: &nbsp; Shipped &nbsp; <code>13</code></th>
<th align="center" width="34%">:hammer_and_wrench: &nbsp; Building &nbsp; <code>4</code></th>
<th align="center" width="33%">:compass: &nbsp; Next &nbsp; <code>7</code></th>
</tr>
</thead>
<tr>
<td valign="top">

<p align="center">
<sub><b>PUBLISHED</b></sub> <br><br>
<a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/✓_Pipeline_Specification-2ea043?style=flat-square" alt="Pipeline Spec"></a> <br>
<a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/✓_Extension_Interface_(WIT)-2ea043?style=flat-square" alt="WIT Interface"></a> <br>
<img src="https://img.shields.io/badge/✓_Pipeline_Schema_%26_Validation-2ea043?style=flat-square" alt="Pipeline Schema"> <br>
<a href="https://github.com/Lab-Hit/.github/blob/main/SECURITY.md"><img src="https://img.shields.io/badge/✓_Security_Disclosure_Policy-2ea043?style=flat-square" alt="Security Policy"></a> <br><br>
<sub><b>COMPLETED</b></sub> <br><br>
<img src="https://img.shields.io/badge/✓_Extension_Naming_Convention-2ea043?style=flat-square" alt="Extension Naming"> <br>
<img src="https://img.shields.io/badge/✓_DAG_Pipeline_Scheduler-2ea043?style=flat-square" alt="DAG Scheduler"> <br>
<img src="https://img.shields.io/badge/✓_WASM_Sandbox_Model-2ea043?style=flat-square" alt="WASM Sandbox"> <br>
<img src="https://img.shields.io/badge/✓_CLI_Interface_Design-2ea043?style=flat-square" alt="CLI Design"> <br>
<img src="https://img.shields.io/badge/✓_Pipeline_Execution_Engine-2ea043?style=flat-square" alt="Execution Engine"> <br>
<img src="https://img.shields.io/badge/✓_Container_Execution_Backend-2ea043?style=flat-square" alt="Container Backend"> <br>
<img src="https://img.shields.io/badge/✓_Event_Bus_Integration-2ea043?style=flat-square" alt="Event Bus"> <br>
<img src="https://img.shields.io/badge/✓_Policy_Engine-2ea043?style=flat-square" alt="Policy Engine"> <br>
<img src="https://img.shields.io/badge/✓_WASM_Plugin_Loading-2ea043?style=flat-square" alt="WASM Plugin Loading">
</p>

</td>
<td valign="top">

<p align="center">
<sub><b>IN PROGRESS</b></sub> <br><br>
<img src="https://img.shields.io/badge/◆_Developer_Documentation-d29922?style=flat-square" alt="Developer Docs"> <br>
<img src="https://img.shields.io/badge/◆_Extension_Development_Kit-d29922?style=flat-square" alt="Extension SDK"> <br>
<img src="https://img.shields.io/badge/◆_Extension_Signing-d29922?style=flat-square" alt="Extension Signing"> <br>
<img src="https://img.shields.io/badge/◆_Pipeline_YAML_Reference-d29922?style=flat-square" alt="Pipeline YAML Reference">
</p>

</td>
<td valign="top">

<p align="center">
<sub><b>PLANNED</b></sub> <br><br>
<img src="https://img.shields.io/badge/○_Distributed_Scheduling-6e7681?style=flat-square" alt="Distributed Scheduling"> <br>
<img src="https://img.shields.io/badge/○_Pipeline_Caching-6e7681?style=flat-square" alt="Pipeline Caching"> <br>
<img src="https://img.shields.io/badge/○_Extension_Marketplace-6e7681?style=flat-square" alt="Extension Marketplace"> <br>
<img src="https://img.shields.io/badge/○_Secret_Management-6e7681?style=flat-square" alt="Secret Management"> <br>
<img src="https://img.shields.io/badge/○_Audit_Logging-6e7681?style=flat-square" alt="Audit Logging"> <br>
<img src="https://img.shields.io/badge/○_Extension_Developer_Guide-6e7681?style=flat-square" alt="Extension Developer Guide"> <br>
<img src="https://img.shields.io/badge/○_Example_Pipeline_Library-6e7681?style=flat-square" alt="Example Pipeline Library">
</p>

</td>
</tr>
</table>

---

<p align="center">
  <a href="https://labhit.dev"><b>Website</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><b>Specification</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/.github/blob/main/SECURITY.md"><b>Security</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><b>License</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="mailto:hello@labhit.dev"><b>Contact</b></a>
</p>
