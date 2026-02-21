# Security Policy

## Reporting a Vulnerability

Report security vulnerabilities to **security@labhit.io**.

Do not open a public GitHub issue for security vulnerabilities.

### What to Include

- Description of the vulnerability
- Steps to reproduce
- Impact assessment
- Suggested fix (if any)

### Response Timeline

| Severity | Acknowledgment | Fix Target |
|----------|---------------|-----------|
| Critical | 24 hours | 72 hours |
| High | 48 hours | 7 days |
| Medium | 7 days | 30 days |
| Low | 14 days | Next release |

### Scope

This policy covers:

- The LabHit engine (`labhit` repository)
- The WASM runtime and sandbox
- The API gateway (GraphQL + gRPC)
- Official extensions
- The web dashboard
- Infrastructure at labhit.dev

### Out of Scope

- Third-party extensions in the marketplace
- Vulnerabilities in upstream dependencies (report to the upstream project)
- Social engineering attacks

## Supported Versions

| Version | Supported |
|---------|-----------|
| 0.x (pre-release) | Best-effort |
| 1.x (when released) | Full support |

## Security Design

LabHit is built with a defense-in-depth security model:

1. **Execution Isolation** -- Container-based isolation for pipeline stages
2. **Plugin Sandboxing** -- WASM with deny-by-default capabilities
3. **Identity & Access** -- Workload identity and access control
4. **Policy Engine** -- Attribute-based policy evaluation
5. **Supply Chain** -- Extension signing and provenance verification
6. **Runtime Monitoring** -- Syscall-level observability

## Responsible Disclosure

We follow a 90-day disclosure timeline. If a fix is not available within 90 days,
the reporter may disclose the vulnerability publicly.

We credit reporters in our security advisories unless they request anonymity.
