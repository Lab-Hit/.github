<p align="center">
  <a href="https://labhit.dev">
    <img src="https://raw.githubusercontent.com/Lab-Hit/.github/main/profile/banner.svg" alt="LabHit — The modular CI/CD engine" width="100%">
  </a>
</p>

<p align="center">
  <a href="https://labhit.dev"><img src="https://img.shields.io/badge/labhit.dev-4F8EF7?style=for-the-badge&logoColor=white" alt="Website"></a>
  &nbsp;
  <a href="https://demo.labhit.dev"><img src="https://img.shields.io/badge/Live_Demo-f59e0b?style=for-the-badge&logoColor=white" alt="Demo"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/Specification-181717?style=for-the-badge&logo=github&logoColor=white" alt="Spec"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><img src="https://img.shields.io/badge/Apache_2.0-D22128?style=for-the-badge&logo=apache&logoColor=white" alt="License"></a>
  &nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/stargazers"><img src="https://img.shields.io/github/stars/Lab-Hit/labhit-spec?style=for-the-badge&logo=github&color=e3b341&logoColor=white" alt="Stars"></a>
  &nbsp;
  <a href="https://bsky.app/profile/labhit.dev"><img src="https://img.shields.io/badge/Bluesky-0285FF?style=for-the-badge&logo=bluesky&logoColor=white" alt="Bluesky"></a>
</p>

<br>

<p align="center">
  <em>"The engine should be invisible. The extensions should be infinite."</em>
</p>

<p align="center">
  LabHit is a <b>modular CI/CD engine</b> written in Rust. A minimal core handles scheduling, isolation, and policy.<br>
  Everything else — source control, builds, scans, deployments — is a sandboxed WASM extension you install at will.
</p>

<br>

---

<br>

<table>
<tr>
<td align="center" width="33%">

### :gear: &nbsp; Modular Core

The engine ships with **zero built-in integrations**. Install `source/git` for checkout, `build/container` for builds, `scan/trivy` for security scans. Your pipeline has exactly what it needs — nothing more.

</td>
<td align="center" width="33%">

### :shield: &nbsp; Secure by Default

Every extension runs in a **WASM sandbox** with deny-by-default permissions. Scoped filesystem access, network allowlists, capped memory and CPU. A bad plugin cannot touch the host.

</td>
<td align="center" width="33%">

### :package: &nbsp; One Binary

**Three modes from a single binary.** Run on your laptop with embedded storage, scale to a standalone server, or deploy a full cluster. Same config, same extensions, same results everywhere.

</td>
</tr>
</table>

<br>

---

### How it works

<p align="center">
  <img src="https://raw.githubusercontent.com/Lab-Hit/.github/main/profile/architecture.svg" alt="LabHit Architecture" width="100%">
</p>

You define your pipeline in YAML. The engine parses it, builds a dependency graph (DAG), and runs independent stages in parallel. Each stage either calls a WASM extension (`use`) or runs a shell command (`run`) inside a container sandbox.

Policy gates can block stages until approval conditions are met. Every event is published to the event bus for observability and integrations. Pipeline runs are persisted to storage for history and audit.

<br>

---

### Pipeline format

Stages declare **what** to run, **when** to run, and **how** to run. Variable interpolation connects stage outputs.

```yaml
engine: "1"
pipeline:
  name: build-and-deploy

stages:
  fetch:
    use: source/git
    with:
      repo: ${{ var.repo_url }}
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
      tag: ${{ env.REGISTRY }}/${{ var.app_name }}

  scan:
    after: [test]
    use: scan/trivy

  deploy:
    after: [build, scan]
    use: deploy/kubernetes
    gate:
      approval: required
    with:
      image: ${{ stage.build.output.image_tag }}
      run_id: ${{ run.id }}
```

> **`use`** loads a WASM extension &nbsp;·&nbsp; **`run`** executes a shell command &nbsp;·&nbsp; **`after`** declares dependencies &nbsp;·&nbsp; **`sandbox`** sets the container image &nbsp;·&nbsp; **`gate`** enforces policy checks
>
> **`${{ var.* }}`** pipeline variables &nbsp;·&nbsp; **`${{ env.* }}`** environment &nbsp;·&nbsp; **`${{ stage.*.output.* }}`** stage outputs &nbsp;·&nbsp; **`${{ run.* }}`** run context
>
> Extensions follow the `category/name` convention. The [specification](https://github.com/Lab-Hit/labhit-spec) defines the full interface contract.

<br>

---

<h3 align="center">At a Glance</h3>

<br>

<p align="center">
  <img src="https://img.shields.io/badge/Core_Engine-Rust-DE6E34?style=for-the-badge&logo=rust&logoColor=white" alt="Rust">
  &nbsp;
  <img src="https://img.shields.io/badge/Extensions-WASM-654FF0?style=for-the-badge&logo=webassembly&logoColor=white" alt="WASM">
  &nbsp;
  <img src="https://img.shields.io/badge/API-GraphQL-E10098?style=for-the-badge&logo=graphql&logoColor=white" alt="GraphQL">
  &nbsp;
  <img src="https://img.shields.io/badge/Security-Deny_by_Default-2ea043?style=for-the-badge&logoColor=white" alt="Security">
  &nbsp;
  <img src="https://img.shields.io/badge/License-Apache_2.0-D22128?style=for-the-badge&logo=apache&logoColor=white" alt="Apache 2.0">
</p>

<br>

---

<h3 align="center">Progress</h3>

<p align="center"><b>Built in the open. Shipped when ready.</b></p>

<br>

<table width="100%">
<thead>
<tr>
<th align="center" width="33%">:white_check_mark: &nbsp; Shipped &nbsp; <code>18</code></th>
<th align="center" width="34%">:hammer_and_wrench: &nbsp; Building &nbsp; <code>4</code></th>
<th align="center" width="33%">:compass: &nbsp; Next &nbsp; <code>6</code></th>
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
<img src="https://img.shields.io/badge/✓_WASM_Plugin_Loading-2ea043?style=flat-square" alt="WASM Plugin Loading"> <br>
<img src="https://img.shields.io/badge/✓_GraphQL_API_Server-2ea043?style=flat-square" alt="GraphQL API"> <br>
<img src="https://img.shields.io/badge/✓_Extension_Developer_Guide-2ea043?style=flat-square" alt="Extension Developer Guide"> <br>
<img src="https://img.shields.io/badge/✓_Persistent_Storage_Layer-2ea043?style=flat-square" alt="Persistent Storage"> <br>
<img src="https://img.shields.io/badge/✓_Variable_Interpolation-2ea043?style=flat-square" alt="Variable Interpolation"> <br>
<img src="https://img.shields.io/badge/✓_Security_Test_Suite-2ea043?style=flat-square" alt="Security Tests">
</p>

</td>
<td valign="top">

<p align="center">
<sub><b>IN PROGRESS</b></sub> <br><br>
<img src="https://img.shields.io/badge/◆_Developer_Documentation-d29922?style=flat-square" alt="Developer Docs"> <br>
<img src="https://img.shields.io/badge/◆_Extension_Development_Kit-d29922?style=flat-square" alt="Extension SDK"> <br>
<img src="https://img.shields.io/badge/◆_Extension_Signing-d29922?style=flat-square" alt="Extension Signing"> <br>
<img src="https://img.shields.io/badge/◆_Secret_Management-d29922?style=flat-square" alt="Secret Management">
</p>

</td>
<td valign="top">

<p align="center">
<sub><b>PLANNED</b></sub> <br><br>
<img src="https://img.shields.io/badge/○_Distributed_Scheduling-6e7681?style=flat-square" alt="Distributed Scheduling"> <br>
<img src="https://img.shields.io/badge/○_Pipeline_Caching-6e7681?style=flat-square" alt="Pipeline Caching"> <br>
<img src="https://img.shields.io/badge/○_Extension_Marketplace-6e7681?style=flat-square" alt="Extension Marketplace"> <br>
<img src="https://img.shields.io/badge/○_Audit_Logging-6e7681?style=flat-square" alt="Audit Logging"> <br>
<img src="https://img.shields.io/badge/○_Pipeline_YAML_Reference-6e7681?style=flat-square" alt="Pipeline YAML Reference"> <br>
<img src="https://img.shields.io/badge/○_Example_Pipeline_Library-6e7681?style=flat-square" alt="Example Pipeline Library">
</p>

</td>
</tr>
</table>

<br>

---

<h3 align="center">:bell: &nbsp; Stay Updated</h3>

<p align="center">LabHit is in <b>pre-launch</b> — the core engine is built, and we're preparing for public release. Get early access:</p>

<br>

<table width="100%">
<tr>
<td align="center" width="33%">

**:star: Star the Spec**

Be the first to know when the engine goes public.

<a href="https://github.com/Lab-Hit/labhit-spec"><img src="https://img.shields.io/badge/Star_labhit--spec-181717?style=for-the-badge&logo=github&logoColor=white" alt="Star"></a>

</td>
<td align="center" width="33%">

**:eyes: Watch Releases**

Get notified for every milestone and release.

<a href="https://github.com/Lab-Hit/labhit-spec/subscription"><img src="https://img.shields.io/badge/Watch_for_Releases-181717?style=for-the-badge&logo=github&logoColor=white" alt="Watch"></a>

</td>
<td align="center" width="33%">

**:rocket: Try the Demo**

See the platform in action with live pipeline visualization.

<a href="https://demo.labhit.dev"><img src="https://img.shields.io/badge/demo.labhit.dev-f59e0b?style=for-the-badge&logoColor=white" alt="Demo"></a>

</td>
</tr>
</table>

<br>

---

<p align="center">
  <a href="https://labhit.dev"><b>Website</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://demo.labhit.dev"><b>Demo</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec"><b>Specification</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/.github/blob/main/SECURITY.md"><b>Security</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="https://github.com/Lab-Hit/labhit-spec/blob/main/LICENSE"><b>License</b></a>
  &nbsp;&nbsp;&middot;&nbsp;&nbsp;
  <a href="mailto:hello@labhit.dev"><b>Contact</b></a>
</p>

<p align="center">
  <sub>Built with Rust :crab: &nbsp;&middot;&nbsp; Secured with WASM :lock: &nbsp;&middot;&nbsp; Licensed under Apache 2.0</sub>
</p>
