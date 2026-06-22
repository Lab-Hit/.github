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
  <em>"CI/CD that runs inside the tools you already use."</em>
</p>

<p align="center">
  LabHit adds <b>portable, isolated, policy-checked builds</b> to GitHub and your existing pipelines —<br>
  the same pipeline, the same result, on any CI and any cloud. Run it as a layer, or run it standalone.
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

<table>
<tr>
<td align="center" width="33%">

### :white_check_mark: &nbsp; Shipped &nbsp; `10`

Built, tested, and live in the public beta.

<img src="https://img.shields.io/badge/✓_Pipeline_Engine-2ea043?style=flat-square" alt="Pipeline Engine"> <br>
<img src="https://img.shields.io/badge/✓_Extension_System_%26_Marketplace-2ea043?style=flat-square" alt="Extension System and Marketplace"> <br>
<img src="https://img.shields.io/badge/✓_Policy_%26_Isolated_Execution-2ea043?style=flat-square" alt="Policy and Isolated Execution"> <br>
<img src="https://img.shields.io/badge/✓_Live_Build_Logs-2ea043?style=flat-square" alt="Live Build Logs"> <br>
<img src="https://img.shields.io/badge/✓_Webhooks-2ea043?style=flat-square" alt="Webhooks"> <br>
<img src="https://img.shields.io/badge/✓_Sign--in_%26_Accounts-2ea043?style=flat-square" alt="Sign-in and Accounts"> <br>
<img src="https://img.shields.io/badge/✓_Public_Beta_API_%26_Dashboard-2ea043?style=flat-square" alt="Public Beta API and Dashboard"> <br>
<img src="https://img.shields.io/badge/✓_GitHub_Integration_—_status_checks-2ea043?style=flat-square" alt="GitHub Integration"> <br>
<img src="https://img.shields.io/badge/✓_Crash_Recovery_—_Never_Lose_a_Run-2ea043?style=flat-square" alt="Crash Recovery"> <br>
<img src="https://img.shields.io/badge/✓_Build--failure_Diagnosis-2ea043?style=flat-square" alt="Build-failure Diagnosis">

</td>
<td align="center" width="33%">

### :hammer_and_wrench: &nbsp; Building &nbsp; `3`

Run LabHit inside the workflow your team already uses.

<img src="https://img.shields.io/badge/◆_Connect--your--repo_Flow-d29922?style=flat-square" alt="Connect-your-repo Flow"> <br>
<img src="https://img.shields.io/badge/◆_Per--repo_Secrets-d29922?style=flat-square" alt="Per-repo Secrets"> <br>
<img src="https://img.shields.io/badge/◆_Pipeline_Health_%26_Flaky_Detection-d29922?style=flat-square" alt="Pipeline Health and Flaky Detection">

</td>
<td align="center" width="33%">

### :compass: &nbsp; Planned &nbsp; `5`

Bring your existing pipeline, run it anywhere, or standalone.

<img src="https://img.shields.io/badge/○_LabHit_Step_for_Your_Workflow-6e7681?style=flat-square" alt="LabHit Step for Your Workflow"> <br>
<img src="https://img.shields.io/badge/○_Import_Your_Existing_Pipeline-6e7681?style=flat-square" alt="Import Your Existing Pipeline"> <br>
<img src="https://img.shields.io/badge/○_Deploy_to_Any_Cloud-6e7681?style=flat-square" alt="Deploy to Any Cloud"> <br>
<img src="https://img.shields.io/badge/○_Run_Portably_Across_Any_CI-6e7681?style=flat-square" alt="Run Portably Across Any CI"> <br>
<img src="https://img.shields.io/badge/○_Standalone_Mode%2C_Documented-6e7681?style=flat-square" alt="Standalone Mode, Documented">

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
