# dlp-scan-action

GitHub Action that scans files for PII, secrets, and sensitive data using the [Spidercob](https://spidercob.com) DLP engine.

## Quick start

```yaml
- name: Scan for secrets & PII
  uses: SpiderCob/dlp-scan-action@v1
  with:
    token: ${{ secrets.SPIDERCOB_TOKEN }}
    fail-on: 'HIGH'
```

No paths needed — the action auto-detects files changed in the PR or push.

## Inputs

| Input | Description | Default |
|---|---|---|
| `token` | Spidercob API token | **required** |
| `paths` | Files/dirs to scan (space-separated). Auto-detects changed files if omitted. | `''` |
| `fail-on` | Minimum severity to fail: `CRITICAL` \| `HIGH` \| `MEDIUM` \| `LOW` | `HIGH` |
| `track` | `guardian` (DLP/PII) or `sentinel` (malware) | `guardian` |
| `secrets-only` | Only scan for secrets and credentials | `false` |

## Outputs

| Output | Description |
|---|---|
| `findings-count` | Total findings across all scanned files |
| `highest-severity` | Highest severity found (`CRITICAL`, `HIGH`, `MEDIUM`, `LOW`, or `NONE`) |

## Full example

```yaml
name: Security Scan

on: [push, pull_request]

jobs:
  dlp:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0   # needed for diff detection

      - name: DLP scan
        id: dlp
        uses: SpiderCob/dlp-scan-action@v1
        with:
          token: ${{ secrets.SPIDERCOB_TOKEN }}
          fail-on: 'HIGH'

      - name: Print summary
        if: always()
        run: |
          echo "Findings: ${{ steps.dlp.outputs.findings-count }}"
          echo "Highest severity: ${{ steps.dlp.outputs.highest-severity }}"
```

## Scan specific paths

```yaml
- uses: SpiderCob/dlp-scan-action@v1
  with:
    token: ${{ secrets.SPIDERCOB_TOKEN }}
    paths: 'src/ config/ data/report.csv'
    fail-on: 'CRITICAL'
```

## Get a token

Sign up free at [spidercob.com](https://spidercob.com/register) and add `SPIDERCOB_TOKEN` to your repository secrets.
