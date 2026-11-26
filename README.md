# GitHub Action
This action should be used to prepare a git push to the [ris-reports repository](https://github.com/digitalservicebund/ris-reports/).

It handles timestamping of the reports and only executes a `git add` if the report file has changed.

## get-latest
Downloads the latest report (or a sub path of it) into a local directory:

```yaml
- uses: digitalservicebund/add-ris-report/get-latest@main
  with:
    sourceDir: test-reports/ris-caselaw/allure-tests
    subPath: history
    targetPath: core/build/allure-results/history
    BUCKET_ACCESS_KEY_ID: ${{ secrets.BUCKET_ACCESS_KEY_ID }}
    BUCKET_SECRET_ACCESS_KEY: ${{ secrets.BUCKET_SECRET_ACCESS_KEY }}
```

It skips silently (output `found=false`) if no report exists.
