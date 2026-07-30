# SpiderCob DLP Secret Scan — ML-Powered GitHub Action

> **Best results** — uses SpiderCob ML models for high-accuracy detection with minimal false positives. Free account required.
> Looking for offline/no-signup scanning? Try [spidercob/scan-action](https://github.com/marketplace/actions/spidercob-security-scan).

Scan your code for hardcoded secrets, PII, and vulnerabilities on every push and pull request — powered by the SpiderCob ML engine.

- **ML-powered** — intent classification reduces false positives (real secrets vs test mocks)
- Detects 40+ secret patterns: AWS, GitHub, OpenAI, Anthropic, Stripe, Slack, Google, and more
- Detects PII: SSN, credit cards, email addresses, phone numbers
- Detects vulnerable code: SQL injection, command injection, XSS, path traversal, and more
- Smart diff detection — only scans files changed in the PR
- Inline GitHub annotations on the PR diff
- Free — sign up at [spidercob.com](https://spidercob.com/register)

## Quick start

```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  spidercob:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: SpiderCob/dlp-scan-action@v1
        with:
          token: ${{ secrets.SPIDERCOB_TOKEN }}
```

Add `SPIDERCOB_TOKEN` to your repository secrets — get one free at [spidercob.com/register](https://spidercob.com/register).

## Inputs

| Input | Default | Description |
|---|---|---|
| `token` | required | SpiderCob API token (`secrets.SPIDERCOB_TOKEN`) |
| `paths` | auto | Files/dirs to scan. Auto-detects changed files if omitted. |
| `fail-on` | `HIGH` | Minimum severity to fail: `CRITICAL`, `HIGH`, `MEDIUM`, `LOW` |
| `track` | `guardian` | `guardian` (secrets + PII) or `sentinel` (malware) |
| `secrets-only` | `false` | Only scan for secrets and credentials (faster) |

## Outputs

| Output | Description |
|---|---|
| `findings-count` | Total findings across all scanned files |
| `highest-severity` | Highest severity found: `CRITICAL`, `HIGH`, `MEDIUM`, `LOW`, `NONE` |

## Examples

**Fail only on CRITICAL:**
```yaml
- uses: SpiderCob/dlp-scan-action@v1
  with:
    token: ${{ secrets.SPIDERCOB_TOKEN }}
    fail-on: CRITICAL
```

**Malware scan:**
```yaml
- uses: SpiderCob/dlp-scan-action@v1
  with:
    token: ${{ secrets.SPIDERCOB_TOKEN }}
    track: sentinel
```

**Use outputs:**
```yaml
- uses: SpiderCob/dlp-scan-action@v1
  id: scan
  with:
    token: ${{ secrets.SPIDERCOB_TOKEN }}
- run: echo "Found ${{ steps.scan.outputs.findings-count }} issues"
```

## Which action should I use?

| | `dlp-scan-action` (this) | `scan-action` |
|---|---|---|
| **Detection** | ML-powered, high accuracy | Regex only |
| **False positives** | Minimal (ML filters test data) | Higher |
| **Account needed** | Yes (free) | No |
| **Internet required** | Yes | No |
| **Recommendation** | Most users | Air-gapped / privacy-first |

## Learn more

- [SpiderCob on PyPI](https://pypi.org/project/spidercob/)
- [SpiderCob Enterprise](https://spidercob.com) — full DLP platform with dashboard, ICAP proxy, audit trail

