# ciclops

👁️

*ciclops* (CI-clops) is a GitHub Action that summarizes the result of a Test
Suite executed in a series of "strategy matrix" branches, and helps orient
investigations when tests fail.

The legends tell of a mythical one-eyed creature that was cursed by the gods to
watch over Continuous Integration pipelines for all eternity.

## Usage

To use ciclops, your Test step(s) in your CI/CD pipeline shoudl be producing
JSON reports from each test executed. You can find a sample YAML file here.

If your CI/CD pipeline runs tests in several *strategy matrix* branches, you
should ensure the JSON artifacts are uploaded via the GitHub `upload` action.
You should add the summary creation step to fire once all branches have
finished, then download all the JSON artifacts created in the various branches,
and moved to one directory.

With those prerequisites, you can trigger ciclops with the `artifact_directory`
argument set to the folder containing the JSON artifacts, and the `output_file`
set to write to the $GITHUB_STEP_SUMMARY environment variable provided
by GitHub.

For example:

``` yaml
    …
    …
    sßteps:
      - uses: actions/checkout@v3

      - name: Create a directory for the artifacts
        run: mkdir test-artifacts

      - name: Download all artifacts to the directory
        uses: actions/download-artifact@v3
        with:
          path: test-artifacts

      - name: Flatten all artifacts onto directory
        # The download-artifact action, since we did not give it a name,
        # downloads all artifacts and creates a new folder for each.
        # In this step we bring all the JSONs to a single folder
        run: |
            mkdir test-artifacts/data
            mv test-artifacts/*/*.json test-artifacts/data

      - name: Compute the E2E test summary
        uses: EnterpriseDB/ciclops@main
        with:
          artifact_directory: test-artifacts/data
          output_file: "$GITHUB_STEP_SUMMARY"
```

## Origin

At EDB, working on a series of Kubernetes operators for PostgreSQL, we have an
extensive test suite that is executed for a variety of combinations of
PostgreSQL and Kubernetes versions, using GitHub's "strategy matrix" construct.

When there are failures in a given run of our CI/CD pipeline, it becomes
difficult to make sense of things. With so many tests executed in so many matrix
branches, clicking into each matrix branch and drilling in to find the failing
tests quickly becomes painful.
Systemic issues often escape scrutiny, buried in data. Information is lost
like … tears in rain.

*ciclops* adds a
[job summary](https://github.blog/2022-05-09-supercharging-github-actions-with-job-summaries/)
to the GitHub Actions output of a CI/CD pipeline. It buckets tests according to
several different criteria, doing the grunt work of figuring out if there was a
pattern to test failures.
It also displays a table of test durations, sorted by slowest.
