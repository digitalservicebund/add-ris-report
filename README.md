# GitHub Actions for ris-reports

## Upload a report
This action uploads reports to the [ris-reports](https://github.com/digitalservicebund/ris-reports/) s3 bucket.

It handles timestamping of the reports and only uploads them if they changed.

```yaml
- uses: digitalservicebund/add-ris-report@[commit-sha]
  with:
    filePath: build/allure-report
    destinationDir: test-reports/ris-caselaw/allure-tests
    reportIsDirectory: true
    keepAllVersions: false
    skipComparison: true
    BUCKET_ACCESS_KEY_ID: ${{ secrets.BUCKET_ACCESS_KEY_ID }}
    BUCKET_SECRET_ACCESS_KEY: ${{ secrets.BUCKET_SECRET_ACCESS_KEY }}
```

| Input                                                 | Default          | Description                                                                                        |
|-------------------------------------------------------|------------------|----------------------------------------------------------------------------------------------------|
| `filePath`                                            | –                | Path to the report file or directory (required)                                                    |
| `destinationDir`                                      | –                | Directory in the bucket for this report type. Every report type needs its own directory (required) |
| `reportIsDirectory`                                   | `false`          | Set to `true` if `filePath` is a directory                                                         |
| `keepAllVersions`                                     | `true`           | Keep prior versions. When `false`, older versions are deleted after a new one was uploaded         |
| `skipComparison`                                      | `false`          | Always upload without downloading and comparing the previous report                                |
| `BUCKET_ACCESS_KEY_ID`, `BUCKET_SECRET_ACCESS_KEY`    | –                | Bucket credentials (required)                                                                      |
| `BUCKET_ENDPOINT_URL`, `BUCKET_REGION`, `BUCKET_NAME` | STACKIT defaults | Bucket connection settings                                                                         |

Storage layout:
- Directory reports are uploaded to `<destinationDir>/<timestamp>/`.
- File reports are uploaded as `<destinationDir>/latest.<ext>`, plus `<destinationDir>/<timestamp>.<ext>` if `keepAllVersions` is `true`.

## Download latest report
Action to download the latest report (or a sub path of it) into a local directory:

```yaml
- uses: digitalservicebund/add-ris-report/download-latest@[commit-sha]
  with:
    sourceDir: test-reports/ris-caselaw/allure-tests
    subPath: history
    targetPath: core/build/allure-results/history
    BUCKET_ACCESS_KEY_ID: ${{ secrets.BUCKET_ACCESS_KEY_ID }}
    BUCKET_SECRET_ACCESS_KEY: ${{ secrets.BUCKET_SECRET_ACCESS_KEY }}
```

It skips silently (output `found=false`) if no report exists.
