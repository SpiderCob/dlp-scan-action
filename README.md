# dlp-scan-action

GitHub Action to scan your codebase for secrets, PII, and sensitive data using [dlp-patterns](https://github.com/SpiderCob/dlp-patterns).

Detects 50+ categories: AWS/GitHub/Stripe/Slack keys, JWTs, private keys, DB connection strings, SSNs, credit cards, and more. Powered by Luhn validation, entropy gating, and context scoring to minimise false positives.

## Usage

```yaml
- name: DLP Secret Scan
  uses: spidercob/dlp-scan-action@v1
```

## Inputs

| Input | Description | Default |
|---|---|---|
| `path` | File or directory to scan | `.` |
| `secrets-only` | Skip PII, scan only API keys and credentials | `false` |
| `fail-on` | Minimum severity to fail the build: `critical`, `high`, `medium`, `low` | `critical` |

## Outputs

| Output | Description |
|---|---|
| `findings-count` | Total number of findings |
| `highest-severity` | Highest severity found: `CRITICAL`, `HIGH`, `MEDIUM`, `LOW`, or `none` |

## Examples

**Basic secret scanning (recommended for most repos):**

```yaml
- name: DLP Secret Scan
  uses: spidercob/dlp-scan-action@v1
  with:
    secrets-only: 'true'
```

**Scan everything including PII:**

```yaml
- name: DLP Scan
  uses: spidercob/dlp-scan-action@v1
  with:
    path: 'src/'
    fail-on: 'high'
```

**Use findings in subsequent steps:**

```yaml
- name: DLP Scan
  id: dlp
  uses: spidercob/dlp-scan-action@v1
  with:
    secrets-only: 'true'

- name: Report
  run: echo "Found ${{ steps.dlp.outputs.findings-count }} issues, highest ${{ steps.dlp.outputs.highest-severity }}"
```

**Full CI pipeline example:**

```yaml
name: Security

on: [push, pull_request]

jobs:
  dlp-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Scan for secrets
        uses: spidercob/dlp-scan-action@v1
        with:
          secrets-only: 'true'
          fail-on: 'critical'
```

## Exit codes

- `0` — no findings at or above the `fail-on` severity
- `1` — findings found at or above the `fail-on` severity

## Enterprise

Need a full DLP platform with dashboards, audit logs, ICAP proxy integration, Gmail/Slack scanning, and compliance reports?

[Spidercob](https://spidercob.com) — the enterprise DLP platform this action is built on.

## License

Apache 2.0
