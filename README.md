# GitHub Action
This action should be used to upload reports to the [ris-reports](https://github.com/digitalservicebund/ris-reports/) s3
bucket.

It handles timestamping of the reports and only uploads them if the report has changed.

## Get latest report
Additional action to download the latest report (or a sub path of it) into a local directory:

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

## Performance
Directory reports with thousands of small files (e.g. Allure) are slow to compare, upload and delete.
Set `skipComparison: true` to skip downloading and diffing the previous report when it changes on every run anyway.
